# release-organizer

## 什么时候使用

当用户要求或任务语义是在从 `Drafts/` 汇总、生成、重写或升级 `Releases/` 文档时，必须使用本流程。典型表达包括：

- “整理成 Release”
- “落到 Releases”
- “把这些 Drafts 汇总成正式文档”
- “基于 Drafts 生成项目总览 / 设计文档 / 规范”

用户无需额外提醒“记录灵感并归档 Drafts”；这是本流程的一部分。

## 输入边界

只处理本次相关 Draft：

- 用户明确指定的 Draft。
- Agent 本次实际读取并用于生成 Release 的 Draft。
- Release 文档中明确声明吸收或部分吸收的 Draft。

不要为了整理而全量扫描整个 `Drafts/`。不要读取 `Archive/`，除非用户明确要求、当前 Release 来源关系指向、`Drafts/00_灵感索引.md` 指向，或确有追溯需要。

## 执行步骤

1. 确认本次输入 Draft 清单。
   - 在动手前列出本次会处理的 Draft。
   - 无状态 Draft 默认视为 `unreviewed`，不要要求用户补状态。

2. 生成或更新 Release。
   - Release 必须达到“读者不用追问也能照做”的标准。
   - 不确定、未验证、仍只是灵感的内容不要写成权威结论。

3. 在 Release 末尾写入来源与替代关系。

   ```md
   ## 来源与替代关系

   本文吸收并替代：
   - Drafts/xxx.md
   - Drafts/yyy.md
   ```

   如果只是部分吸收：

   ```md
   ## 来源与替代关系

   本文部分吸收：
   - Drafts/xxx.md：已吸收 A/B，未处理 C
   ```

4. 摘录遗留灵感。
   - 从本次相关 Draft 中找出未进入 Release、但未来可能有用的点。
   - 追加到 `Drafts/00_灵感索引.md`。
   - 每条尽量写清楚：灵感、来源、适用场景、状态。
   - 不要把未确认灵感直接写入 `Releases/` 作为事实。

5. 处理 Draft 去留。
   - 完全吸收：移动到 `Archive/`。
   - 部分吸收：留在 `Drafts/`，在文件顶部补 frontmatter。
   - 仍在推进：留在 `Drafts/`，必要时标 `active`。
   - 明显过期但仍需留痕：移动到 `Archive/`。
   - 不删除 Draft。

6. 给仍留在 `Drafts/` 的已审阅文件补状态。

   ```yaml
   ---
   status: partially_absorbed
   absorbed_by: Releases/xxx.md
   remaining_value: 命名方向、增长假设
   last_reviewed: YYYY-MM-DD
   ---
   ```

   可用状态：
   - `partially_absorbed`
   - `active`
   - `reference`

7. 复核。
   - `Releases/` 中的结论不能依赖未读 Archive 才能理解。
   - `Drafts/` 中不应继续堆放已完全吸收的原始材料。
   - `Archive/` 中的文件不应被日常执行默认读取。
   - 文件移动后，Release 和灵感索引中的路径要准确。

## 命名建议

归档 Draft 时优先保留原文件名。必要时可加状态前缀：

- `Archive/已吸收_原文件名.md`
- `Archive/2026-07-11_已吸收_主题.md`

如果原文件名已经包含日期和主题，通常直接移动即可。

## 禁止事项

- 不要全量扫描 `Archive/`。
- 不要删除 Draft 或 Archive 内容。
- 不要把未确认灵感写成 Release 结论。
- 不要为了补状态而改写用户草稿正文。
- 不要处理与本次 Release 无关的 Draft。
