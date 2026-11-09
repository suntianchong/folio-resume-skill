# Folio Resume Workbench — Agent Skill

把 [Folio 简历工作台](https://resume.puppytodo.ai) 的能力交给**你自己的 Agent**：
读取已保存的简历版本、创建/复制草稿、诊断排版、归档/恢复，并把整批修改提交为
**Proposal 供人审**。

Folio 产品内部没有任何 AI。这个仓库只提供「Agent 与 Folio 通信」的 Skill 与
命令行客户端：所有智能来自你选择的 Agent，所有高风险动作仍由人确认。

## 仓库内容

| 路径 | 说明 |
|---|---|
| `SKILL.md` | Skill 主文件（Agent 从这里开始读） |
| `START-HERE.md` | 安装与最短上手说明 |
| `references/tasks.md` | 读取/创建/复制/归档/Proposal 工作流 |
| `references/complete-resume.md` | 整篇重写与合成头像的边界 |
| `COMPLETE-RESUME-TASK.md` | 虚构样例任务卡（仅演示） |
| `folio` | 启动包装器（解析 Node 与 CLI，不自动安装任何软件） |
| `bin/folio.mjs` | 已构建的 Folio CLI（随仓库分发，无需本地源码） |

## 要求

- Node.js **24+**（包装器不会自动安装）
- 一个可访问的 Folio 服务（默认 `https://resume.puppytodo.ai`，可用
  `FOLIO_ORIGIN` 指定；本地开发用 `http://127.0.0.1:4180`）
- 配对批准必须由**人在浏览器里**完成，Agent 无法代批

## 安装

推荐方式：把整个仓库放进你 Agent 的 skills 目录。

```bash
# 以 Codex 为例；换成你所用 Agent 的实际 skills 目录
DEST="${CODEX_HOME:-$HOME/.codex}/skills/folio-resume-workbench"
test -e "$DEST" && { echo "已存在同名 Skill，请先确认如何更新"; exit 1; }
git clone <本仓库地址> "$DEST"
chmod +x "$DEST/folio"
```

然后让 Agent 读取 `$DEST/SKILL.md` 即可。仓库自带 `bin/folio.mjs`，**不需要**
本地 Folio 源码；若你另有已安装的 CLI，可设置 `FOLIO_CLI_BIN` 指向其绝对路径。

不推荐 `curl | sh` 一类的管道安装；安装与更新都应由人确认。

## 首次连接

```bash
export FOLIO_ORIGIN="https://resume.puppytodo.ai"
./folio doctor                 # 服务是否可达、契约版本是否匹配
./folio auth status            # 当前 profile 与已连接账号
./folio auth login --name "my-agent" --email "you@example.com" --no-wait
```

`auth login` 会返回一个 `verificationUri`：**在浏览器中核对账号与权限并确认**，
确认后再运行：

```bash
./folio auth complete --no-wait
./folio auth status
```

`awaiting_browser_approval` 表示仍在等待人工批准，不是成功。Token 不要发给
任何 Agent 或聊天窗口。

## 环境变量

| 变量 | 用途 |
|---|---|
| `FOLIO_ORIGIN` | 目标服务地址（默认可访问线上环境） |
| `FOLIO_CLI_BIN` | 已安装 CLI 的绝对路径（覆盖仓库自带 CLI） |
| `FOLIO_NODE` | 指定 Node 24+ 可执行文件 |
| `FOLIO_CONFIG_DIR` | 独立的凭证 profile 目录 |
| `FOLIO_PROJECT_ROOT` | 从 Folio 源码目录使用时的工作区根 |
| `FOLIO_NO_OPEN` | 禁止自动打开浏览器 |

## 安全边界（Skill 硬性约束）

- 只用用户选定的那份简历；提交前必须重读最新版本，绝不虚构经历、公司、
  时间、数据、奖项或技能
- 修改统一走**整批 Proposal**；提交不等于生效，需要人在 Folio 里审阅
- **不做**：接受/拒绝 Proposal、删除、导出、备份、分享、投递
- 遇到 `STALE_VERSION` 必须重新读取并整批重评，不允许用旧请求体重试

## 版本

当前 Skill/CLI 版本 **0.1.0**，对应 Folio 服务契约 `version: 2`。
CLI 产物 `bin/folio.mjs` 的 SHA-256 记录在 `SHA256SUMS`。

## 许可

本仓库为私有分发材料，**保留所有权利**，未授予开源许可。
第三方组件与素材声明见 [NOTICE.md](NOTICE.md)。
