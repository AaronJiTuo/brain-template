<p align="center">
  <img src="https://repository-images.githubusercontent.com/1253325468/d3f20b07-c7e0-4c8b-ac8f-b5761b0a1e31" alt="brain-template" width="100%">
</p>

# brain-template

这是一个用于创建 **`*-brain` 项目** 的模板仓库。每个 brain 都是人与 AI agent 共同维护的项目知识系统，而不是项目代码的主仓库。

它把散落在对话、草稿和不同工作阶段中的项目知识持续整理下来，让成熟成果可以直接执行，让重要决策有迹可循，也让后来的人或 agent 能够可靠接手。

---

## 立即开始

### 创建一个新 brain

1. 点击 GitHub 上的 **「Use this template」**，创建一个名为 `<项目名>-brain` 的仓库。
2. 用可以操作仓库的 AI agent 打开它。
3. 把下面这段话发给 agent：

```text
请先读 AGENTS.md。若这是尚未初始化的新 brain，请按 .skills/brain-initializer/SKILL.md 开始；先通过对话了解项目，等我确认后再落盘。
```

### 让新 agent 接手已有 brain

```text
请先读 AGENTS.md，并按其中的接手顺序阅读 Releases/ 的核心入口和 .records/CURRENT.md。先复述你对项目和当前状态的理解，等我确认后再执行具体任务。
```

即使使用的工具会自动读取 `AGENTS.md`，显式要求 agent 先读它，仍能让不同工具从同一套规则开始工作。

---

## brain 如何工作

```text
对话、资料与工作成果
        │
        ├─ 尚未定稿 ─→ Drafts/
        ├─ 已经确认 ─→ Releases/
        └─ 决策与状态 ─→ .records/

被替代或完成使命的内容 ─→ Archive/
```

- 平时可以直接和 agent 探索、设计和推进工作，不需要先学会维护整套目录。
- 尚未定稿但值得保留的材料进入 `Drafts/`；成熟到读者无需追问也能照做的内容进入 `Releases/`。
- 重要决策和实际状态由 agent 记录到 `.records/`，让项目规范与现实进度都不会在交接时丢失。
- 新 agent 接手时先读 `Releases/` 和 `.records/CURRENT.md`，再按需追溯少量历史，避免从头重建上下文。

---

## 内容去哪里找

| 你想找什么 | 去哪里 |
| --- | --- |
| 已经确认、可以照着执行的内容 | `Releases/` |
| 草稿、资料和正在形成的成果 | `Drafts/` |
| 项目当前实际进度 | `.records/CURRENT.md` |
| 重要决策和历史状态变化 | `.records/events/` |
| 已被替代或只在追溯时需要的内容 | `Archive/` |

`README.md` 是项目入口；`AGENTS.md`、`.skills/` 和 `.brain-template.json` 组成供 agent 使用的模板协议，分别负责长期规则、按需工作流程和协议版本。它们通常不需要用户手工维护。

---

## 维护与反馈

- 已初始化的 brain 会静默检查正式发布的稳定协议版本；发现新版并经用户明确同意后，会直接完成标准稳定升级，不重复请求确认。升级成功后才可能单独显示完全可选的 Star 邀请。详细规则见 [`AGENTS.md`](./AGENTS.md) 和 [`.skills/template-upgrader/SKILL.md`](./.skills/template-upgrader/SKILL.md)。
- 如果你在某个 `*-brain` 中摸索出了可以复用的结构或工作方式，欢迎通过 [Issues](https://github.com/AaronJiTuo/brain-template/issues) 反馈给模板项目。
- `brain-template` 本身采用 [MIT License](./LICENSE)；由模板创建的具体项目可以根据自身情况决定许可证。
