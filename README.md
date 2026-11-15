# epg-mirror

EPG 节目单镜像：每 2 小时从上游拉取 8 个源，修正时区后上传到 Cloudflare R2，供 LunaTV / IPTV 使用。

上游与原仓库 [taksssss/tv](https://github.com/taksssss/tv) 一致，见 `files.json`：

| 文件 | 上游 |
| --- | --- |
| 112114.xml.gz | epg.112114.xyz |
| 51zmt.xml.gz | epg.51zmt.top:8000/e.xml（原作者走代理拉取，本仓库先直连尝试） |
| 51zmte1.xml.gz | epg.51zmt.top:8000/e1.xml.gz |
| 51zmte2.xml.gz | epg.51zmt.top:8000/e2.xml.gz |
| erw.xml.gz | raw.githubusercontent.com/kuke31/xmlgz |
| epgpw_cn.xml.gz | epg.pw（`+0000` → `+0800`） |
| epgpw_hk.xml.gz | epg.pw（`+0000` → `+0800`） |
| epgpw_tw.xml.gz | epg.pw（`+0000` → `+0800`） |

## 对外地址

`https://<R2 公开域名>/epg/<文件名>.xml.gz`，例如：

```
https://<R2 公开域名>/epg/51zmt.xml.gz
```

## 台标

`icons.yml` 每周日 03:00（UTC）同步一次原仓库 `icon/` 目录（1000+ 个台标 png）与 `iconList_default.json` 到 R2，缓存 24 小时：

```
https://<R2 公开域名>/icon/<台标文件名>.png
https://<R2 公开域名>/iconList_default.json
```

## 运行方式

- 定时：`40 */2 * * *`（每 2 小时）
- 手动：Actions 页 workflow_dispatch
- 单个源下载失败时保留 R2 里的旧文件，不覆盖，不报错中断整轮

## 所需 Secrets

| Secret | 说明 |
| --- | --- |
| `R2_ACCOUNT_ID` | Cloudflare 账户 ID |
| `R2_BUCKET` | R2 桶名 |
| `R2_ACCESS_KEY_ID` | R2 S3 API Access Key |
| `R2_SECRET_ACCESS_KEY` | R2 S3 API Secret Key |
