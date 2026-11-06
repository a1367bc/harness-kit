# 第三方声明 / Third-party notices

本仓库不是原创项目，是下列既有开源项目的**本地副本**（裁剪 + 重写元数据），代码版权归原作者。
本文件保留上游 NOTICE 原文，以满足 Apache License 2.0 第 4(d) 条「保留 NOTICE 文件内容」的要求。

## 上游

- 仓库：https://github.com/HunYiSage/dsh-purge
- 版本：v1.1.92
- 许可证：Apache License 2.0（全文见 [LICENSE](./LICENSE)）
- 商业使用：上游另有商业授权条款（转售 / 白标 / 作为商业产品分发需另行取得授权），本仓库不附带该项授权。

### 上游 NOTICE 原文

```
dsh-purge
Copyright 2026 HunYiSage

Source: https://github.com/HunYiSage/dsh-purge
Maintained by the dsh-purge maintainers team.

This product is licensed under the Apache License, Version 2.0.
Derivative works must retain this NOTICE and attribution.

Bundled skills under skills/redteam/ originate from the dsh-purge upstream project (YuJunZhiXue/dsh-purge), retrieved via the mirror mmparko/dsh-purge.
```

### 红队模块的再上游

`lib/redteam/` 部分源自 `Jueze-2019/dsh-redteam-mode`（dual-use 声明见 [docs/redteam-upstream-DISCLOSURE](./docs/redteam-upstream-DISCLOSURE)，其许可证见 [docs/redteam-upstream-LICENSE](./docs/redteam-upstream-LICENSE)）。

## 本副本做了什么

**未改动**：`lib/`、`client.js`、`presets/`、`scripts/`、`cordis.patch.yml` 内容与上游对应版本一致（仅 `bin/` 的入口文件名与其中的提示文案改了名）。

**移除**：

| 移除内容 | 原因 |
|---|---|
| `README.md` / `README.zh-CN.md` / `README.ja.md` | 含推广文案与社群入口，本副本已重写 |
| `release-notes.md` | 上游版本发布记录 |
| `screenshots.json`、`docs/preview/`、`docs/banner.svg` | 上游宣传物料 |
| `docs/notice/` | 上游作者的个人照片、微信二维码等（隐私） |
| `LICENSE-COMMERCIAL.md` | 上游商业授权说明，不随本副本分发 |
| `.github/` | 上游发布流水线 |
| `docs/v1.1.34-prompt-inject.md` | 完整提示词原文，不随本副本再分发 |

**重写**：`README.md`、`package.json`（包名 `harness-kit`，移除 author / homepage / repository / bugs 字段）。

**保留但未改**：代码内的 `dsh-purge` 标识符（模块 id、补丁锚点、localStorage key），改名会导致插件加载失败与 Apply/Restore 失配。

## 使用前提

本插件会修改本机官方 DeepSeek Harness 的运行时行为。仅在你自己的机器上、你自行承担后果的前提下使用；用于他人系统或对外服务前，请先确认当地法律与目标方授权。
