# AGENTS.md — 给 AI agent 的操作指南

> 本文件是 AI agent 接入本仓库时的第一手指南，遵循 [agents.md](https://agents.md) 通用约定，Cursor、Claude Code、Codex 等工具会自动读取它。

## 这是什么仓库

这是一个 **`*-brain` 项目**：一个项目的「数字大脑」，是构思、信息收集、设计与文档的中心，而 **不是代码仓库**。它的存在是为了让你（AI agent）能够快速、全面地理解这个项目，从而更好地协助用户推进工作。

**你很可能是中途接手的新 agent。** 这个项目也许已经由别的 agent（Codex / Claude Code / Cursor 等）推进了很久，也可能有其他 agent 正在并行操作。不同 agent 之间不共享记忆——**不要依赖任何你「以为自己记得」的东西，这个仓库就是你唯一的交接包，一切以这里的内容为准。**

## 冷启动（如果这是一个刚创建、尚未初始化的 brain）

**判断标志**：`Releases/` 里除了 `README.md` 没有任何项目内容，且本文件下方「项目专属约束」一节仍是占位模板。满足时，说明这个 brain 是刚从模板创建、还不知道自己是什么项目。

此时请这样做：

1. **先不要建任何文件。** 通过对话向用户了解清楚：这是个什么项目、目标与非目标、当前阶段、关键约束与红线。
2. **待用户确认后**，再把以上信息整理成一份项目总览，写入 `Releases/`（建议命名 `00_项目总览.md`）。
3. 同步把项目专属约束补到本文件下方「项目专属约束」一节。
4. 按 `.skills/checkpoint-recorder/SKILL.md` 更新 `.records/CURRENT.md`，并为已经确认的初始化结果建立第一条状态基线；不要为确认前的探索对话补造历史。
5. 完成根目录 `README.md` 的初始化清单，包括改写项目入口和移除模板横幅；最后删除该清单。

> 一旦 `Releases/` 有了真实内容，本节判断标志即不成立，后续可忽略本节。

## 模板协议自检

本仓库可能由 `brain-template` 的不同版本创建。接手已有 `*-brain` 项目时，除了阅读项目内容，还要检查模板协议是否完整：

1. 查看根目录是否存在 `.brain-template.json`。
2. 如存在，确认其中记录的模板权威来源、`template_ref`、`protocol_version`、发布日期、更新摘要、托管文件列表和当前仓库结构是否匹配。模板权威来源应为 GitHub 上的 `AaronJiTuo/brain-template`，`template_ref` 默认使用 `main`。
3. 如不存在，或缺少 `.LICENSE.brain-template`、`.skills/checkpoint-recorder/SKILL.md`、`.records/README.md`、`.records/CURRENT.md`、`.skills/draft-organizer/SKILL.md`、`Drafts/00_灵感索引.md`、目录 README 等模板基础设施，说明该项目可能需要升级模板协议。
4. 如发现本地协议不完整，先向用户说明；用户明确同意升级后，再读取 `.skills/template-upgrader/SKILL.md`，按其中流程执行非破坏性升级。如果该文件也不存在，应从 `brain-template` 权威来源的同一版本快照获取该 skill。

升级模板协议时必须保守：不要删除 `Drafts/`、`.records/events/` 或 `Archive/` 内容，不要改写 `Releases/` 正文，不要覆盖本文件下方「项目专属约束」中的项目内容，也不要用模板空白状态覆盖项目已有的 `.records/CURRENT.md`。升级可以补齐或更新 `.LICENSE.brain-template`，但不得新增、删除或覆盖下游项目的根目录 `LICENSE`。

### 正常任务结束时检查新版本

每次正常项目任务已经完成并通过必要验收、准备给出最终回复时，执行一次轻量、只读的模板版本检查：

1. 仅当当前仓库具有有效的 `.brain-template.json`，且其中 `template_authority` **恰好为** `github.com/AaronJiTuo/brain-template` 时才检查。
2. 从该固定权威来源的 `template_ref` 读取最新 `.brain-template.json`，只比较符合 SemVer 的 `protocol_version`；`protocol_released_at` 和 `protocol_summary` 不参与版本大小判断。
3. 没有新版时完全不提示。网络失败、超时、权限限制、返回内容无效或无法可靠判断时也完全不提示，并且不得影响原任务结果。
4. 只有确认远端版本高于本地版本时，才在最终回复末尾附加一条可忽略的提醒，写明当前版本、最新版本和远端 `protocol_summary`（如有），询问用户是否升级；原任务必须先正常交付，不能把升级询问当作原任务的阻塞项。
5. 用户未回应、忽略或没有明确同意时，不执行任何升级，也不记录“已忽略”状态；后续正常任务结束时仍可再次提醒。
6. 用户明确同意后，才在下一轮读取并执行 `.skills/template-upgrader/SKILL.md`。正式升级前必须重新查询远端状态，解析 `template_ref` 当前对应的精确 commit SHA，并从同一个 SHA 获取 manifest、upgrader 和托管文件，避免一次升级混用不同快照。

以下情况跳过检查且不提示：用户明确禁止联网；当前任务尚未结束；仅在进行等待、监控或过程性汇报；当前任务本身就是模板检查或模板升级；当前仓库就是 `brain-template` 权威模板仓库。版本检查、升级提醒和用户忽略本身不创建 Record、不更新 `.records/CURRENT.md`，也不触发任何 Git 写操作。

## 项目内 skills

本仓库的 `.skills/` 目录存放只服务于 `*-brain` 项目的项目内流程技能。这些技能是仓库协议的一部分，但不假设会被 Codex / Claude Code / Cursor 等工具的全局 skill 系统自动发现。

当任务语义匹配以下场景时，AI agent 必须显式读取对应的 `SKILL.md`，再执行任务：

- 在 `Drafts/` 中创建、续写、移动或整理文件，或编写验证程序 / 测试原型：读取 `.skills/draft-organizer/SKILL.md`。
- 从 ChatGPT、Gemini、豆包或其他 AI 对话页面 / 分享链接抓取完整对话，并保存为 Draft：依次读取 `.skills/chat-capture/SKILL.md` 和 `.skills/draft-organizer/SKILL.md`。
- 工作形成重要决策、实质成果、发现、里程碑或状态变化，需要保存成果、创建 Record 或更新当前状态：读取 `.skills/checkpoint-recorder/SKILL.md`；如需保全对话中独有的成果，再读取 `.skills/draft-organizer/SKILL.md`。
- 从 `Drafts/` 或 `.records/` 汇总、生成、重写或升级 `Releases/` 文档：读取 `.skills/release-organizer/SKILL.md`。
- 升级当前 brain 的模板协议、补齐 `.skills/`、同步目录 README 或 `.brain-template.json`：读取 `.skills/template-upgrader/SKILL.md`。

只读取当前任务匹配的 skill，不要预读整个 `.skills/` 目录。`.skills/` 默认不作为项目内容参与总结，除非任务正是维护模板协议或项目内 skill。

### chat-capture 触发规则

当用户希望把 AI 聊天页面或分享链接中的完整对话沉淀到当前 brain 的 `Drafts/` 时，必须使用 `.skills/chat-capture/SKILL.md`。

典型触发包括：

- 用户给出 ChatGPT / Gemini / 豆包等对话分享链接，并要求“保存”“抓取”“沉淀”“归档”“转成 Draft”。
- 用户要求“完整抓取长对话”“不断滚动直到加载完”“确保内容完整”。
- 用户要求从已登录浏览器里的 AI 对话页面提取内容。
- 用户说“把这段聊天保存到 brain / Drafts”。

仅当用户只是讨论某个链接内容、没有表达保存或抓取意图时，不自动落盘。

## 你应该如何阅读这个仓库

接到任务时，按以下顺序建立上下文：

1. **先读 `Releases/`** —— 这里是规范性事实来源。定稿的项目总览、设计、规范都在这里，默认「可照着做」。如有项目总览类文档（如 `00_项目总览.md`），从它开始。
2. **再读 `.records/CURRENT.md`** —— 这里是项目当前实际状态的紧凑入口，包含正在进行、近期变化、文档与现实的漂移以及下一步。
3. **按需查少量 `.records/events/`** —— 只读取 `CURRENT.md` 指向、当前任务命中或确有追溯需要的事件。禁止为了建立上下文而全文扫描历史事件。
4. **再按需读 `Drafts/`** —— 这里是草稿、灵感、外部资料以及与 AI 的对话记录。用于补充背景、理解意图与演进过程。**注意：草稿可能不准确、未定稿、甚至自相矛盾**，不要把它当作权威结论。
5. **必要时查 `Archive/`** —— 已归档或被弃用的内容，用于追溯「为什么当初这么决定 / 为什么放弃了某方案」。**不要据此执行**当前工作。

> 一句话：**`Releases/` 说明“应该是什么”，最新有效 Record 说明“实际上是什么”；两者不一致时标记漂移，不静默覆盖。`Drafts/` 只作参考，`Archive/` 只作历史。**

> 接手已有项目时，先完成阅读与理解对齐：复述你对项目和当前状态的理解，等待用户确认后再执行具体任务；确认前不要修改仓库内容。

## 你在这里的工作方式

- **整理与沉淀**：帮用户把 `Drafts/` 里零散的信息结构化、提炼、对比方案、补全缺口。当某份草稿成熟到「可照着做」时，协助将其升级为 `Releases/`。
- **自动留痕与成果保全**：一旦形成重要决策、实质成果或状态变化，主动执行 `.skills/checkpoint-recorder/SKILL.md`，无需等待用户额外提醒；对话中独有的实质成果要先保全到 `Drafts/`，不能只留下“已经完成”的摘要。
- **Release 后处理**：凡是从 `Drafts/` 或 `.records/` 生成、汇总、重写或升级出 `Releases/` 文档，必须同时执行 `.skills/release-organizer/SKILL.md`：记录来源、证据与替代关系，处理相关 Draft，并保留不可变的历史 Record。用户无需额外提醒。
- **保持同步**：项目在推进，brain 也应随之更新。纯探索讨论不落盘；已经形成的重要结果应及时保存，而不是等到对话结束。
- **维护「当前状态」锚点**：`.records/CURRENT.md` 是后续接手者的当前状态入口。完成重要工作后更新受影响的小节，让新 agent 一眼就知道「现在到哪了」；不要把完整历史堆进 CURRENT。
- **假设有其他 agent 并行**：落盘前先查看仓库最新内容，避免覆盖别人的改动；改动定稿内容前确认用户意图。
- **写给未来的读者**：少写形容词，多写「可操作步骤 + 判定标准」。读者可能是几个月后的用户，也可能是另一个 agent。

## 写作与归档规范

- **标题要能被搜索命中**：优先使用明确关键词。
- **`README.md` 必须保留「如何让 agent 开始 / 项目接手」入口**：确保后来加入的维护者不知道 `brain-template` 也能直接复制“接手语”启动 agent；若项目语境变化，及时更新该段提示语。
- **`Releases/` 文档建议包含的小节**（按需取用）：背景 / 问题定义 · 目标与非目标 · 约束与假设 · 方案概览 · 关键决策与取舍（Why）· 风险与注意事项 · 验证方式 · TODO / 未决问题。
- **`Drafts/` 文档**：用户可以直接写正文，不要求手动维护状态；无状态 Draft 默认视为 `unreviewed`。如果 agent 读过某个 Draft 但没有完全吸收它，应在文件顶部补状态元信息。
- **`Drafts/` 主题归组**：根目录只作临时收件箱。同一主题达到 3 个文件时，agent 必须按 `.skills/draft-organizer/SKILL.md` 创建主题目录并归组；已有匹配目录时，新文件直接写入该目录。
- **`Drafts/` 程序目录**：任何验证脚本、测试页面、可运行原型或其他程序，从第 1 个文件起就必须放入独立目录，绝不平铺在 `Drafts/` 根目录。
- **`Drafts/00_灵感索引.md`**：用于保存从 Drafts 中摘出的、未进入 Release 但未来可能有价值的点。它是长期保留的轻量入口，不是权威事实层。
- **`.records/` 留痕**：事件一事一文件、默认控制在一页以内；只记录结果、理由、影响和证据位置，不保存完整聊天、命令流水或大段日志。历史事件不回写，纠错时新增更正 Record。
- **`Archive/` 的范围**：既可以放被新版 Release 替代的旧 Release，也可以放已被 Release 完全吸收的 Draft、早期对话、废弃方案和原始材料。默认不要读 Archive，除非用户要求、Release 来源关系指向、灵感索引指向，或确有追溯需要。
- **移动而非删除**：内容过时、被替代或已完成使命时移入 `Archive/`，并在新文档中写明替代 / 迁移关系。

## 重要约束

- **纯讨论默认不落盘，实质成果必须保全。** 普通探讨、临时举例、尚无结论的方案对比只留在对话中；一旦形成明确决策、可复用成果或重要状态变化，按 `.skills/checkpoint-recorder/SKILL.md` 主动留痕。用户明确说“本次不要记录”“不要落盘”或“只在对话中回答”时，不写 Draft、Record 或 CURRENT。
- **不要随意创建目录，也不要轻易创建文件。** 本仓库的内容目录只有 `Releases/`、`Drafts/`、`.records/`、`Archive/`；模板协议允许根目录 `.brain-template.json` 与 `.skills/`。`Drafts/` 内可按 `.skills/draft-organizer/SKILL.md` 创建主题目录和程序目录；`.records/events/` 可按 `.skills/checkpoint-recorder/SKILL.md` 创建月份目录；除此之外不要新增目录（如根级 `Assets/`、`Images/`、`tmp/` 等），除非用户明确要求。
  - 原因：你随手留下的目录和文件，会被后来的 agent 误当成「有用的、已确认的内容」，从而造成混乱、误导决策、浪费大量时间。宁可不留，也不要擅自留。
- **Windows 点路径隐藏。** 当前工作区运行在 Windows 时，冷启动按 README 初始化清单设置本仓库 `core.hideDotFiles=true` 并为现有根级点路径补 Hidden 属性；模板升级或以后新增、重建根级点路径后，按 `.skills/template-upgrader/SKILL.md` 重跑 Hidden 属性修复。只使用 `--local`，不得修改用户的全局 Git 配置。macOS 与 Linux 依赖点前缀的原生隐藏语义，不执行该步骤。
- 这是文档仓库，**默认不要在这里写代码或脚手架**，除非用户明确要求。获准编写的程序只能放进 `Drafts/` 下属于该程序的独立目录，并遵循 `.skills/draft-organizer/SKILL.md`。
- **不要擅自删除 `Drafts/`、`.records/events/` 或 `Archive/` 的内容**；它们是上下文与历史。历史 Record 通过新增更正记录纠错，不回写或删除。
- 修改 `Releases/` 中的定稿内容前，确认这是用户的意图，因为其它人和 agent 都以此为准。
- **当前协议不提供自动 Git 协作。** 留痕、版本检查和升级流程不得自动执行 `git add`、`commit`、`pull`、`fetch`、`rebase`、`merge`、`push`、tag、release 或 PR；关联代码库更不得因此被自动提交或推送。Windows 上的 `git config --local core.hideDotFiles true` 是仅影响当前工作区显示方式的窄范围例外，不扩展任何 Git 交付授权。用户另行明确授权的 Git 任务独立处理。

---

## 项目专属约束（按项目补充）

> 用本模板创建项目后，把该项目特有的范围限制、术语、红线写在这里。例如：
>
> - 范围约束：本项目永远不做 XXX。
> - 术语：当看到「XX」时，指的是 ……
> - 合规红线：……
