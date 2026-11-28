# owc-backend-dist

OWC V3 后端的公开**发行通道**。源码仓库保持私有；这里只放已签名、可校验的 OTA 产物。

## 固定清单地址

```
https://github.com/ethandong06-debug/owc-backend-dist/releases/latest/download/backend-manifest.json
```

兜底清单：

```
https://raw.githubusercontent.com/ethandong06-debug/owc-backend-dist/main/backend-manifest.json
```

清单签名与 App 共用同一把 Ed25519 发布公钥：

```
ffef8dfc9e818fe2a0b70c18c2f3b03608707e3d50464f40a355096f142881d5
```

清单 canonical 字符串固定为：

```
version\nsha256\nsize
```

`url` 可指向直达 CDN；GitHub Release 内的 `OWC-V3-backend-<版本>-arm64.tar.gz` 作为 `mirror_urls` 镜像。

## 发布新版本

1. 在私有源码仓库构建并验证后端包。
2. 用仓库外的 Ed25519 私钥生成签名（canonical 三段）。
3. 把包放到一个短期可下载地址，提交 `backend-release-request.json`：

```json
{
  "version": "H3.3.1",
  "tag": "H3.3.1",
  "pkg_url": "https://…",
  "download_url": "https://aka.doubaocdn.com/s/…",
  "mirror_urls": ["https://github.com/ethandong06-debug/owc-backend-dist/releases/download/H3.3.1/OWC-V3-backend-H3.3.1-arm64.tar.gz"],
  "asset_name": "OWC-V3-backend-H3.3.1-arm64.tar.gz",
  "sha256": "…",
  "size": 1234567,
  "signature": "…",
  "notes": "…"
}
```

工作流会校验 SHA256/大小，创建 Release、上传包和签名清单，并把 `backend-manifest.json` 写回 main。

## 边界

- 后端 Release 与 App Release 分仓库，避免 `releases/latest` 互相顶掉。
- CDN 主地址必须保持可长期访问；GitHub Release 必须保留为镜像。不要把短期签名链接当作唯一来源。
- 不提交签名私钥。
