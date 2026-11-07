# agents-md-templates

由 [agents-md-sync](https://github.com/trick77/agents-md-sync) 合成 `AGENTS.md` 时使用的**源片段模板仓库**。

这里只放规范片段（`.md` 源文件），生成的 `AGENTS.md` 落在各目标仓库和 `~/.codex/AGENTS.md` 中，**不要手改生成产物**。

## 安装

### 前置要求

- Node.js ≥ 22（agents-md-sync 依赖）
- git、ssh（克隆与推送本仓库）

### 1. 安装 agents-md-sync

```bash
npm install -g agents-md-sync
agents-md-sync --version   # 验证
```

### 2. 克隆模板仓库

```bash
git clone git@github.com:tzpeng/agents-md-templates.git ~/agents-md-templates
```

### 3. 软链封装脚本

```bash
mkdir -p ~/.local/bin
ln -s ~/agents-md-templates/bin/sync-agents ~/.local/bin/sync-agents
rehash                       # zsh 刷新命令缓存（bash 用 hash -r）
sync-agents --help
```

> `~/.local/bin` 需在 PATH 中；若不在，在 `~/.zshrc`（或 `~/.bashrc`）加一行：
> `export PATH="$HOME/.local/bin:$PATH"`

### 4. 初始化全局（Codex）与各仓库

首次生成全局指令（Codex 所有会话）：

```bash
mkdir -p ~/.codex/.agents
# 写一份 ~/.codex/.agents/PROJECT.md（仓库专属的 PROJECT 片段）
agents-md-sync --template-dir ~/agents-md-templates --profile ROB006 --repo ~/.codex
```

每个要接入的仓库：

```bash
mkdir -p <仓库>/.agents
# 写 <仓库>/.agents/PROJECT.md
cd <仓库> && sync-agents
```

之后日常只需 `sync-agents`（或 CI 用 `sync-agents --check`）。

## 目录结构

```text
agents-md-templates/
├── common/                 # 所有 profile 共享的片段（兜底）
│   ├── REVIEW.md
│   └── DO_NOT.md
└── profiles/
    └── ROB006/             # 一个 profile = 一套技术栈/团队规范
        ├── CODING.md
        ├── TESTING.md
        └── BUILD.md
```

## 片段查找顺序

对每个 `NAME.md`，按以下顺序查找，命中即用：

1. 仓库本地 `.agents/NAME.md`（仅影响该仓库，排在最前）
2. `profiles/<profile>/NAME.md`
3. `common/NAME.md`
4. 都没有 → 合成报错

内置骨架默认章节：`PROJECT → CODING → TESTING → BUILD → REVIEW → DO_NOT`。其中 `PROJECT` 没有中央默认，必须由各仓库的 `.agents/PROJECT.md` 提供。

## 常用操作

### 新增一个 profile

```bash
mkdir -p profiles/<技术栈>
# 写入 CODING.md / TESTING.md / BUILD.md 等片段
```

未提供的片段会自动回落到 `common/`，所以新 profile 只需写「与默认不同的部分」。

### 自定义章节集合/顺序

在 profile 目录放一个 `AGENTS.md.tmpl`，用 include 标记声明顺序：

```markdown
<!-- include: PROJECT.md -->
<!-- include: CODING.md -->
<!-- include: TESTING.md -->
<!-- include: REVIEW.md -->
```

### 共享 vs 专属

- **通用规则**（对所有 profile、所有仓库生效）→ 改 `common/*.md`
- **某技术栈专属** → 改 `profiles/<name>/*.md`
- **某个仓库专属** → 改该仓库的 `.agents/*.md`

## 使用方式

合成 `AGENTS.md`：

```bash
# 全局（Codex 所有会话）
agents-md-sync --template-dir ~/agents-md-templates --profile ROB006 --repo ~/.codex

# 某个仓库
cd <仓库> && agents-md-sync --template-dir ~/agents-md-templates --profile ROB006
```

或使用封装脚本（本仓库 `bin/sync-agents`，已软链到 `~/.local/bin/`）：

```bash
sync-agents              # 同步全局 + 默认仓库
sync-agents --check      # 只检查漂移（CI 门禁）
```

## 维护约定

- 改动提交用标准 git 流程：`git add -A && git commit`
- 加新 profile 前，先确认是否已有可复用的 `common/` 片段，避免重复
