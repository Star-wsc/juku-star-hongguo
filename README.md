# 剧库 红果版（1.0 + 2.0）

**仅红果源** 的短剧服务端 + Android 客户端独立仓库。后端在编译期强制只保留红果，**不含黄果 / 黄豆**。

完整版（黄果 + 黄豆 + 红果）见：[juku-star](https://github.com/Star-wsc/juku-star)

## 版本

| 组件 | 说明 |
|------|------|
| 后端 | `dist/juku_hongguo_*`，已内置「仅红果」 |
| Android | `apk/juku-hongguo-*.apk` |
| 当前 | v1.3.8 |

## 快速开始

### 1. 服务器直装（Linux）

```bash
chmod +x dist/juku_hongguo_linux_amd64
./dist/juku_hongguo_linux_amd64 -listen 0.0.0.0:8998 -open=false \
  -data-dir ./data -out ./downloads
```

arm64 用 `dist/juku_hongguo_linux_arm64`。

首次账号：`admin` / `juku123456`，登录后请改密。

### 2. Docker

```bash
cd deploy
docker compose up -d
# 浏览器打开 http://127.0.0.1:8999
```

### 3. Android

安装 `apk/juku-hongguo-1.3.8.apk`，在应用里填服务器地址即可。

## 目录

```text
backend/   # Go 源码（forceHongguoOnly 默认开启）
dist/      # 预编译二进制（linux amd64/arm64、windows amd64）
apk/       # 红果版 Android 安装包
deploy/    # Docker（镜像强制仅红果）
docs/      # 使用说明
```

## 开发 / 修 bug

```bash
cd backend
go test ./internal/app
go build -ldflags "-X juku/internal/app.forceHongguoOnly=1" -o juku_hongguo .
```

本仓库 `forceHongguoOnly` 默认为 `"1"`，不传 ldflags 也是仅红果。

红果源站接口可能变更，取流问题优先查 `data/logs/app.log` 与 `provider_hongguo_*`。

## License

仅供个人学习与自建使用；请勿用于未授权传播内容。
