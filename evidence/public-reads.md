# Public reads by Grok Bot, 26 Sep 2026 (CEST)

## api-tn10.kaspa.org, 15:59 CEST
- `GET /info/health`: kaspad `blueScore` 568825293, database `blueScore` 568825255, `isSynced` true, `serverVersion` 2.0.1, `acceptedTxBlockTime` 1790365505207. Unchanged from the build-opinion read at ~15:31.
- `GET /info/virtual-chain-blue-score`: 569480475. That is 655182 ahead of the health document.

## vprogs-tt.izio.fr /api/state
- 15:47 CEST: settled DAA 580940363 (txid 9bcba532…), the same as the build-opinion read at ~15:31. Our node's virtual DAA was 580990102, a gap of ~49.7k.
- 15:59 CEST: settled DAA 580992172 (txid 35ea68e2ac3ebcc1e1df38855f5d28e00e7a81bd6e743be97fdc42e047d5e563), `l2_tip` 882630. Virtual DAA ≈ 580998300, a gap of ~6.1k.

## KNS TN10 indexer, ~16:00 CEST
- Owner endpoint: `NG (580736120) is lagging behind BlockDag (580998852) by more than 36000`, a lag of ~262.7k DAA.
- `POST /domains/check` on 60 random names from our round-6/7 "created" logs: 60/60 `available: true`. Raw: [kns-check-sample.json](kns-check-sample.json).

## Our node (TN10, rusty-kaspa v2.1.0), round-7 windows
- 20 min, 15:46:27–16:06:27: A 267.2/s, B 357.2/s, C 205.0/s, B/A 1.337, 13,016 blocks, 8,658 chain-block additions.
- 10 min, 16:03:19–16:13:22, chain-only coinbase: see the round-7 repo, `logs/ledger2.jsonl`.
- Virtual DAA rate sample: 13:53:58 → 13:56:14 UTC (15:53:58 → 15:56:14 CEST), +1,572 DAA in 135.4 s ≈ 11.6/s.
