<p align="center">
  <img src="https://repository-images.githubusercontent.com/1253325468/d3f20b07-c7e0-4c8b-ac8f-b5761b0a1e31" alt="brain-template" width="100%">
</p>

# brain-template

这是一个用于创建 **`*-brain` 项目** 的模板仓库。每当你要开始一个新项目时，点击 GitHub 上的 **「Use this template」** 即可基于它快速建立一个属于该项目的「数字大脑」。

> `*-brain` 不是代码仓库，而是一个项目的 **「思考 — 留痕 — 沉淀 — 交付」系统**：把零散灵感、实质成果、重要决策和项目状态逐步整理成可执行、可接手、可追溯的项目上下文。

---

## 这个仓库是给谁看的？

- **主要给 AI agent 看**：让任何一个 agent 在介入项目时，能快速、全面地了解项目背景、目标、决策与现状，从而更好地协助你。
- **人类主要看 `Releases/`**：定稿的内容都在这里；`.records/` 是隐藏的 Agent 留痕层，通常无需逐篇阅读。`Drafts/` 和 `Archive/` 更多是给 AI 提供工作素材与历史脉络。

如果你是一个刚接入本项目的 AI agent，请先阅读 [`AGENTS.md`](./AGENTS.md)。

---

## 目录结构

```
<your-project>-brain/
├── README.md          # 项目入口与说明（你正在看的这种文件）
├── AGENTS.md          # 给 AI agent 的操作指南（最重要）
├── .brain-template.json # 模板协议版本与托管文件清单
├── .skills/           # 项目内流程技能：留痕、Draft、对话抓取、Release 整理等
├── .records/          # 隐藏留痕层：当前状态入口与重要历史事件
├── Releases/          # 规范性事实：定稿的想法、设计、文档
├── Drafts/            # 草稿区：根目录作收件箱，多文件主题与程序使用独立目录
└── Archive/           # 归档区：已完成使命或被弃用的内容
```

| 目录 | 定位 | 放什么 | 原则 |
| --- | --- | --- | --- |
| **`Releases/`** | 规范性事实来源 | 定稿的项目说明、设计方案、开发文档、规范、路线图 | 回答“应该是什么”；默认「可照着做」 |
| **`.records/`** | Agent 留痕与当前状态 | `CURRENT.md` 紧凑状态入口、重要决策与状态变化 | 回答“现在怎样、发生过什么”；按需检索，不全文读取历史 |
| **`Drafts/`** | 当前工作台 / AI 上下文素材 | 零碎想法、灵感、外部资料、与 AI 的对话记录、半成品梳理、验证程序 | 根目录只作临时收件箱；同主题 3 个文件时归组；程序从第 1 个文件起独立成目录 |
| **`Archive/`** | 低频历史归档 | 已被吸收的 Draft、被替代的旧 Release、废弃方案、原始材料 | 默认不读，不删；仅用于追溯 |
| **`.skills/`** | 项目内流程技能 | 检查点留痕、Draft 归组、AI 对话抓取、Release 整理、模板协议升级等流程说明 | 不假设全局安装；由 `AGENTS.md` 按任务显式读取 |

---

## 推荐工作流（从 0 到 1 的演进）

1. **探索与工作** —— 纯头脑风暴和临时举例默认留在对话中；用户明确要求不记录时，brain 不发生留痕写入。
2. **保全实质成果** —— 对话中已经形成、如果不落盘就会丢失的文案、内容或设计成果，由 `.skills/checkpoint-recorder/SKILL.md` 判断并按 `.skills/draft-organizer/SKILL.md` 保存到 `Drafts/`。
3. **记录重要检查点** —— 明确决策、成果、发现、里程碑或状态变化形成后，agent 主动创建结果级 Record，并更新 `.records/CURRENT.md`，无需用户额外提醒。
4. **持续打磨和归组 Draft** —— 普通 Draft 在同主题达到 3 个文件时进入主题目录；验证脚本、测试页面或可运行原型从第 1 个文件起进入独立目录。
5. **达到可交付标准后升级为 `Releases/`** —— 标准是读者不用追问也能照做。凡是从 Draft 或 Record 生成 Release，agent 必须执行 `.skills/release-organizer/SKILL.md`。
6. **同步处理来源材料** —— 已完全吸收的 Draft 移入 `Archive/`；部分吸收的 Draft 留在 `Drafts/` 并补状态；Record 作为历史证据始终保留，不移动、不删除。
7. **被替代或过时后移入 `Archive/`** —— Release 和 Draft 按协议归档，并在新文档中写明替代关系 / 迁移指引。

> 在项目推进过程中，这个 brain 会持续增删、调整。保持它与项目同步，就是保持你和 AI agent 之间的信息同步。

> 协议 2.1.0 在留痕与成果保全之外，增加正常任务结束时的静默版本检查：没有新版或检查失败时不提示，只有发现新版才询问用户，并在用户明确同意后升级。检查和升级都不会自动执行 `git add`、`commit`、`pull`、`push`，也不会对关联代码仓库执行任何 Git 写操作。

## 半自动协议更新

升级到 2.1.0 后，agent 会在每次正常项目任务完成并验收后，对固定权威来源 `github.com/AaronJiTuo/brain-template` 做一次轻量只读检查：

- 本地已是最新版：不提示。
- 网络不可用、超时或无法可靠判断：不提示，不影响原任务。
- 发现更高版本：在原任务结果末尾说明当前版本、最新版本和更新摘要，询问是否升级。
- 用户忽略或没有明确同意：不升级、不写入忽略状态。
- 用户明确同意：下一轮按 `.skills/template-upgrader/SKILL.md` 从同一个远端 commit 快照执行非破坏升级。

版本检查本身不创建 Record、不修改 `.records/CURRENT.md`、不执行 Git 写操作。现有 brain 需要再升级到 2.1.0 一次，之后才会自行发现未来版本。

---

## 如何让 agent 开始

无论项目进行了多久，建议都在 `README.md` 保留这一节，专门给后来加入的维护者做「即插即用」的接手入口。

`AGENTS.md` 会被 Cursor、Codex 等自动读取，理想情况下你几乎不用多说。但显式让 agent「先读 `AGENTS.md`」是个好习惯，也能兜住个别工具不自动加载的情况。下面两段可直接复制：

**启动新项目（冷启动）：**

```
这是一个刚用 brain-template 创建的新 `*-brain`。请先读 `AGENTS.md`，按其中的「冷启动」流程：先通过对话了解本项目，等我确认后再整理项目总览，并完成 `README.md` 中的初始化清单。
```

**新 agent 接手已有项目：**

```
请先读 `AGENTS.md`，再读 `Releases/` 的核心入口和 `.records/CURRENT.md`；只按 CURRENT 的指向读取少量相关 Record，不要全文扫描历史。读完后，请用几句话复述你对项目的理解和当前推进状态；在我确认之前，不要执行任何具体任务，也不要修改仓库内容。
```

---

## 用本模板创建新项目后（初始化清单）

- [ ] 把仓库名改为 `<你的项目>-brain`，并设置好仓库描述。
- [ ] 删除本 `README.md` 顶部的 brain-template 横幅图（那是模板自身的展示图，不属于你的项目）。
- [ ] 删除模板自带的根目录 `LICENSE`，保留 `.LICENSE.brain-template`；与移除横幅一样直接执行，无需用户参与或选择项目许可证。
- [ ] 如果当前工作区运行在 Windows：设置当前仓库的 `core.hideDotFiles=true`，再为根目录所有以 `.` 开头的文件和目录补上 Hidden 属性；macOS 与 Linux 跳过。

    ```powershell
    git config --local core.hideDotFiles true
    Get-ChildItem -Force -LiteralPath . |
      Where-Object { $_.Name.StartsWith('.') } |
      ForEach-Object { attrib.exe +H "$($_.FullName)" }
    ```

- [ ] 用项目实际信息重写本 `README.md` 顶部的项目简介。
- [ ] 在 `README.md` 保留一个「如何让 agent 开始 / 项目接手」小节，并把“新 agent 接手已有项目”的提示语改成你的项目语境（新人应可直接复制使用）。
- [ ] 检查 [`AGENTS.md`](./AGENTS.md)，按项目需要补充「项目专属约束」。
- [ ] 保留 `.brain-template.json` 与 `.skills/`，它们是模板协议的一部分；如未来升级模板，按 `.skills/template-upgrader/SKILL.md` 执行。
- [ ] 保留 `.records/README.md` 与 `.records/CURRENT.md`；前者定义留痕协议，后者是 agent 的当前状态入口。
- [ ] 保留 `.skills/checkpoint-recorder/SKILL.md`；它负责结果级留痕、成果保全、按需读取和敏感信息边界。
- [ ] 保留 `.skills/draft-organizer/SKILL.md`；它负责 Draft 主题归组，并保证程序不会平铺在 `Drafts/` 根目录。
- [ ] 保留 `Drafts/00_灵感索引.md`，作为长期轻量灵感入口。
- [ ] 在 `Releases/` 写下第一份核心文档（建议先写一份项目总览，作为 agent 的入口）。
- [ ] 项目身份确认后，按 `checkpoint-recorder` 建立第一条状态基线并更新 `.records/CURRENT.md`；不要为确认前的对话补造历史。
- [ ] 删除本清单。

---

## 关于这个模板本身

`brain-template` 会随着使用经验不断迭代优化。如果你在某个 `*-brain` 项目里摸索出了更好的结构或写法，欢迎反哺回这个模板。
