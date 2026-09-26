# Mempool count assert: rusty-kaspa v2.1.0 excerpts (read 26 Sep 2026)

Source: https://github.com/kaspanet/rusty-kaspa/blob/v2.1.0/mining/src/mempool/validate_and_insert_transaction.rs and
https://github.com/kaspanet/rusty-kaspa/blob/v2.1.0/mining/src/mempool/model/transactions_pool.rs

Flow inside `post_validate_and_insert_transaction`:

1. `execute_replace_by_fee(...)`: may remove a double-spent transaction (RBF).
2. `limit_transaction_count(&transaction, transaction_size)?`:
   - If `len < maximum_transaction_count` and the byte size fits, it returns an empty list (no eviction).
   - Otherwise it walks `ready_transactions` by ascending feerate, **only `Priority::Low`** (RPC-submitted txs are high priority), skipping the new tx's own ancestors.
   - If a candidate pays a higher feerate than the new tx, it returns `Err(RejectMempoolIsFull)`.
   - It returns the smallest prefix such that `len + 1 - k <= max` and the bytes fit.
   - If it runs out of candidates, it returns `Err(RejectMempoolIsFull)`.
3. The caller removes each selected tx (with its in-pool dependants), breaking early once `len < max` and the bytes fit.
4. `assert!(len < maximum_transaction_count && size + tx_size <= mempool_size_limit, "Transactions in mempool: {len+1}, max: {max}, …")`.
5. `add_transaction(...)`.

The panic we logged in round 1 was `Transactions in mempool: 100001, max: 100000` at `--ram-scale=0.1`. It means `len == max` at step 4, after step 2 returned `Ok`. By the code above, that should not happen: refusal is step 2's `RejectMempoolIsFull`. So the assert is an invariant that broke, not a refusal mechanism. We have not identified the path. It is open.

`mining/src/mempool/config.rs`: default max count 1,000,000. `apply_ram_scale` only scales down. kaspad refuses `--ram-scale` below 0.1 (checked on a throwaway simnet node, 15:47–15:50 CEST).
