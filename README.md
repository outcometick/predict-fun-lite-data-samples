# Predict.fun Crypto Up/Down Market Data — Lite Edition · Free Sample

[Predict.fun historical data](https://outcometick.com/predict-fun-data) for the crypto Up/Down markets, collected 24/7 by [OutcomeTick](https://outcometick.com). This repository hosts a **free sample** of the
lite edition: one real, unmodified UTC day (2026-09-08), laid out exactly as the delivered archive, so code
written against the sample runs unchanged on the full data.

> **中文：** 本仓库是 [OutcomeTick](https://outcometick.com/zh) 采集的[Predict.fun 历史数据](https://outcometick.com/zh/predict-fun-data)（加密 Up/Down 市场）lite 版的**免费样本**：一个真实、未经修改的
> UTC 日（2026-09-08），目录结构与正式交付的数据完全一致，针对样本写的代码可以原样用在正式数据上。

## Download / 下载

**[predict-fun-lite-data-samples.tar.gz](https://github.com/outcometick/predict-fun-lite-data-samples/releases/latest/download/predict-fun-lite-data-samples.tar.gz)**

```bash
curl -L https://github.com/outcometick/predict-fun-lite-data-samples/releases/latest/download/predict-fun-lite-data-samples.tar.gz | tar xz
```

Per-file row counts and sha256: [`samples/manifest.json`](samples/manifest.json).
Field reference: [DATA_GUIDE.md](DATA_GUIDE.md) · 中文字段说明：[数据使用说明.md](数据使用说明.md)

## What the lite edition contains / lite 版包含的数据

- **Markets** — opening price, closing price and settled outcome for every market on the venue
- **[Order book](https://outcometick.com/predict-fun-order-book-data) snapshots** — full depth on both sides, 5-minute and 15-minute markets
- Every asset we collect, the latest 90 days

> **中文：** 市场信息（全部市场的开盘价、收盘价、结算结果）、5 分钟 / 15 分钟市场的盘口快照（完整深度）；全部币种、最近 90 天。

## Files in this sample / 样本包含的文件

| path | rows |
|---|---|
| `data/predict-fun/orderbook/BTC-5M/BTC-5M-predict-orderbook-2026-09-08.jsonl.gz` | 478,603 |
| `data/predict-fun/markets/predict-markets-2026-09-08.jsonl.gz` | 1,245 |

## Learn more / 了解更多

- [OutcomeTick](https://outcometick.com) — prediction-market data for Polymarket and Predict.fun · [中文站](https://outcometick.com/zh)
- [Documentation](https://outcometick.com/docs) — quickstart and complete task examples · [中文文档](https://outcometick.com/zh/docs)
- [API reference](https://outcometick.com/docs/api) · [Data schemas](https://outcometick.com/docs/schemas) · [Settlement rules](https://outcometick.com/docs/settlement)
- [Settlement statistics for BTC](https://outcometick.com/data/predict-fun/btc) — per-asset data page
- [Backtest in the browser](https://outcometick.com/backtest) — run a strategy on the archive without downloading anything

The full data is delivered with an API key and downloaded by day, asset and interval.
正式数据通过 API key 交付，按天、按币种、按周期下载。

For research and backtesting only; not investment advice. / 仅供研究与回测，不构成投资建议。
