# Data Guide

(中文版见 数据使用说明.md)

## Layout

The unpacked sample has exactly the layout of the paid archive:

```
predict-fun-lite-data-samples/
  data/predict-fun/orderbook/BTC-5M/BTC-5M-predict-orderbook-<date>.jsonl.gz
  data/predict-fun/markets/predict-markets-<date>.jsonl.gz
```

## <ASSET>-<INTERVAL>-predict-orderbook-<date>.jsonl.gz — order-book snapshots

Series key is `<ASSET>-<INTERVAL>`; the lite edition carries the 5M / 15M series only, e.g. `BTC-5M`, `ETH-15M`.

| field | meaning |
|---|---|
| market_id | upstream market id |
| category_slug | market slug; suffix = slot start (unix sec) |
| update_ts_ms | upstream book update time (ms) |
| recv_ms | collector receive time (ms) |
| payload | the upstream snapshot object, stored verbatim |

`payload` contains `bids` / `asks` as `[price, size]` pairs, plus the upstream's
own `version`, `marketId`, `orderCount`, `lastOrderSettled` and
`settlementsPending` fields exactly as received.

Note: these are **full snapshots only**. Predict.fun's stream does not publish
order-book deltas, so unlike our Polymarket dataset there is no `price_change`
series and no trade tape. Book state at time t = the market's latest snapshot
with `recv_ms <= t`.

Throttling (disclosed), which changed and matters if you span the date:

- through 2026-08-24: kept at most 1 snapshot per market per second
- **from 2026-08-25: no throttling at all**

The earlier throttle dropped a large majority of upstream snapshots, so a day
from 2026-08-25 onward carries roughly seven times the book detail of an earlier
one. Compare for yourself: the sample day holds 478,603 snapshots for BTC-5M
where 2026-07-24 held 68,398.

## predict-markets-<date>.jsonl.gz — market metadata and settlement outcome

One file per day covering every asset and interval.

| field | meaning |
|---|---|
| category_slug | market id; suffix = slot start (unix sec) |
| asset | btc / eth / bnb |
| interval_label | `5m` / `15m` / `hourly` / `daily` — see the note below |
| market_id | upstream market id (joins to the order-book files) |
| price_feed_id / price_feed_symbol | which feed settles this market |
| price_feed_provider | settlement source for this market: `CHAINLINK` for 5m and 15m, `BINANCE` for daily |
| condition_id | on-chain condition id |
| start_sec / end_sec | slot boundaries (unix sec) |
| start_price | the strike — Up must close strictly above it |
| end_price | the settlement price |
| status | the market state as of our **last read** of it upstream — not a settlement flag, see below |

Interval labels: files exported **before 2026-09-05** label the 1-hour markets
as `daily`. That was our own slug-parsing bug — a single `up-or-down` pattern
matched both families — and it is corrected from that date on. The 24h market is
the one whose slug contains `-on-` (`bitcoin-up-or-down-on-september-5-2026`);
the 1-hour one ends in the hour (`bitcoin-up-or-down-september-5-2026-5am-et`).
If you need the true interval on older files, `end_sec - start_sec` is always
authoritative.

Settlement rule, three outcomes: `end_price > start_price` → Up wins;
`end_price < start_price` → Down wins; `end_price == start_price` → the slot is
a **push**, where the venue resolves both sides as won and stakes are returned.
A tie is not an Up win here — the opposite of Polymarket — and about 1 in 100
five-minute slots closes flat, so a binary `>=` predicate will misscore them.
Both values come from the upstream market object, so a market's outcome is
verifiable from this file alone.

Do **not** use `status` to decide whether a slot has settled. We stop re-reading
a market once it has an `end_price`, and at that moment the venue very often
still reports it as `OPEN` — so `status` freezes at whatever it was then. Most 5m/15m slots do read `RESOLVED` (about 92%),
but the Binance-settled families almost never do — **0 of 255 settled 24h
markets and 0.65% of settled hourly ones** — so filtering on
`status == 'RESOLVED'` silently drops nearly all of them. The reliable test is whether `end_price` is
present; in our whole history no row carries `RESOLVED` without one.

## samples/manifest.json — per-file row counts and sha256 checksums for this sample

Every sample file here is byte-identical to the corresponding file in the paid
dataset; the sha256 values match the archive's own checksum index.
