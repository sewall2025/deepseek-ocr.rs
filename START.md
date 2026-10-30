# 本机启动方式（macOS + Metal）

## 编译

```bash
cargo build --release -p deepseek-ocr-cli -p deepseek-ocr-server --features metal
```

## 启动 Server

```bash
export DEEPSEEK_OCR_CACHE_DIR=/Volumes/Fandata/models/deepseek-ocr
./target/release/deepseek-ocr-server --config ./config.toml
```

- 模型缓存位置：`/Volumes/Fandata/models/deepseek-ocr/models/deepseek-ocr-2/`
- 必须带 `--config ./config.toml`，否则会使用 `~/Library/Application Support/deepseek-ocr/config.toml`（默认 CPU、端口 8000）。
- 外置盘 `/Volumes/Fandata` 需已挂载。

## 配置要点（`./config.toml`）

| 项 | 值 | 说明 |
| --- | --- | --- |
| `models.active` | `deepseek-ocr-2` | 当前模型 |
| `inference.device` | `metal` | 等同 `--device metal` |
| `inference.precision` | `f16` | 等同 `--dtype f16` |
| `server.port` | `8100` | 监听端口，host 为 `0.0.0.0` |

优先级：命令行参数 → 模型默认值 → `config.toml` 的 `[inference]`。

## 验证

```bash
curl http://127.0.0.1:8100/v1/health
curl http://127.0.0.1:8100/v1/models
```
