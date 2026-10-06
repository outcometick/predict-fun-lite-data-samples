# Predict.fun 加密涨跌市场数据 · lite 版 · 免费样本

Predict.fun 加密 Up/Down 市场数据lite 版的**免费样本**：一个真实、未经修改的 UTC 日（2026-09-08），
目录结构与正式交付的数据完全一致，针对样本写的代码可以原样用在正式数据上。

Free sample of the Predict.fun crypto Up/Down market data (lite edition): one real,
unmodified UTC day (2026-09-08), laid out exactly as the delivered archive.

## 下载 / Download

**[predict-fun-lite-data-samples.tar.gz](https://github.com/outcometick/predict-fun-lite-data-samples/releases/latest/download/predict-fun-lite-data-samples.tar.gz)**

```bash
curl -L https://github.com/outcometick/predict-fun-lite-data-samples/releases/latest/download/predict-fun-lite-data-samples.tar.gz | tar xz
```

每个文件的行数与 sha256 见 [`samples/manifest.json`](samples/manifest.json)；字段说明见
[数据使用说明.md](数据使用说明.md)（English: [DATA_GUIDE.md](DATA_GUIDE.md)）。

## 样本包含的文件 / Files in this sample

| path | rows |
|---|---|
| `data/predict-fun/orderbook/BTC-5M/BTC-5M-predict-orderbook-2026-09-08.jsonl.gz` | 478,603 |
| `data/predict-fun/markets/predict-markets-2026-09-08.jsonl.gz` | 1,245 |

## 正式数据 / Full data

正式数据通过 API key 交付，按天、按币种、按周期下载。接口文档：https://outcometick.com/docs

仅供研究 / 回测，不构成投资建议。
