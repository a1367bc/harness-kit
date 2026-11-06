# harness-kit

DeepSeek Harness（DSH）本地补丁插件。装好后在会话标题右侧多一个入口，点开是右侧 dock，两个页签：

- **Clean** —— 补丁状态分组显示，Apply / Restore / Uninstall，提示词编辑器，多套规则集
- **Drill** —— 演练台：资产与本机环境，附带技能库与知识库

启动时会自查并重新应用，`node_modules` 被 npm/pnpm 覆盖后不用手改文件。

## 适用环境

| 项 | 要求 |
|---|---|
| 宿主 | 官方 `dsh web`，或官方桌面 EXE（两者独立安装、独立打补丁） |
| DSH 版本 | 0.2.1-rc.2（0.2.0 仍可用）；0.2.1-rc.3 及更新的 RC 不支持 |
| Node | ≥ 18 |
| 依赖 | 无第三方运行时依赖 |

补丁只作用于本机官方 `@deepseek-ai` 包与本地配置，不改 Harness 源码树，不碰别的目录。

## 安装

**不要走插件市场**（会卡在 `already queued` 且不执行命令），也不要 `pnpm add`（不在 npm registry 上）。

装了 `dsh` 且已在 PATH 上，一条命令 + 重启 + 点一次 Apply：

```sh
# 官方 dsh web
dsh plugin --profile web add https://codeload.github.com/a1367bc/harness-kit/tar.gz/refs/heads/main

# 官方桌面 EXE
dsh plugin --profile desktop add https://codeload.github.com/a1367bc/harness-kit/tar.gz/refs/heads/main
```

1. **完全退出并重开**对应宿主（web 要停掉再起；桌面版连托盘图标一起退）
2. 会话标题旁点 **harness-kit** → **Clean** → **Apply**。Apply 成功会重启宿主一次

URL 必须是 `codeload.github.com`，不要用 `github.com/.../archive`（会 302，pnpm 报 `fetch failed`）。已是本地克隆就直接用 `.`。

## 结构

```
lib/                引擎：补丁应用/回滚、dock 后端、规则、身份注入、
                    红队事实库（store/tools/schema/score/evidence）、网络范围与授权闸
skills/redteam/     23 个技能文档（Markdown runbook），首次启动同步到 $DSH_HOME/redteam/skills
client.js           dock 前端产物（双页签 UI）
presets/            预设：红队模式（主会话 + 5 个执行角色）
scripts/            构建与装配脚本（embed-prompt / build:client / profile-bundle / 报告导出）
bin/harness-kit.js  CLI：--status / --apply / --revert / --uninstall / --edit
cordis.patch.yml    宿主补丁点声明
docs/               授权与法律说明、上游合规声明
```

数据落本地：状态、补丁记录、演练事实库都在用户目录下，不上传任何服务器。

## CLI

```sh
harness-kit --status      # 看当前状态
harness-kit --apply       # 应用（提示词 + 代码 + shim + override）
harness-kit --revert      # 回滚
harness-kit --uninstall   # 卸载（已应用会先回滚）
harness-kit --edit        # 编辑注入文件
```

## 说明

- 代码里的 `dsh-purge` 标识符是**模块 id、补丁锚点、localStorage key**，属功能键，改名会导致插件加载失败、Apply/Restore 失配，所以保持原样。
- `lib/default-prompt-inject.md` 未包含在本仓库（上游也不在仓库里发布，构建时才 embed）。
- `scripts/embed-prompt.mjs` 依赖上面那个文件，没有它这条 script 跑不起来。

## 来源与许可

Apache License 2.0，见 [LICENSE](./LICENSE)。本仓库是既有开源项目的本地副本，来源、版权与改动说明见 [THIRD_PARTY_NOTICES.md](./THIRD_PARTY_NOTICES.md)。
