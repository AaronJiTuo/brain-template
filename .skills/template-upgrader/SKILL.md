# template-upgrader

## 什么时候使用

当当前 `*-brain` 项目需要同步 `brain-template` 的模板协议时，使用本流程。典型触发条件：

- 根目录没有 `.brain-template.json`。
- 缺少 `.skills/release-organizer/SKILL.md` 或 `.skills/template-upgrader/SKILL.md`。
- 缺少 `.skills/chat-capture/SKILL.md`。
- 缺少 `.skills/draft-organizer/SKILL.md`。
- 缺少 `Drafts/00_灵感索引.md`。
- `.brain-template.json` 中的 `protocol_version` 低于模板权威来源的当前协议版本。
- `AGENTS.md` 未声明项目内 skills、Draft 归组与程序目录、Release 后处理或模板协议自检。
- 用户要求“升级 brain-template 协议”“同步新版模板”“给旧 brain 补新版机制”。

## 核心原则

升级必须非破坏性：

- 不删除 `Drafts/` 或 `Archive/` 内容。
- 不改写 `Releases/` 正文。
- 不覆盖 `AGENTS.md` 的「项目专属约束」。
- 不覆盖项目已经改写过的 root `README.md` 项目介绍。
- 只补齐或更新模板协议相关文件和说明。

## 升级步骤

1. 读取当前结构。
   - 查看 `AGENTS.md`、`README.md`、`Releases/README.md`、`Drafts/README.md`、`Archive/README.md`。
   - 查看是否存在 `.brain-template.json`、`.skills/`、`Drafts/00_灵感索引.md`。
   - 查看 git 状态，避免覆盖用户正在编辑的改动。

2. 判断升级范围。
   - 缺文件则补文件。
   - 旧说明缺少新版协议时，只追加或局部替换相关小节。
   - 已有项目内容优先保留，尤其是 `README.md` 顶部介绍和 `AGENTS.md` 项目专属约束。

3. 补齐模板协议文件。
   - `.brain-template.json`
   - `.skills/chat-capture/SKILL.md`
   - `.skills/draft-organizer/SKILL.md`
   - `.skills/release-organizer/SKILL.md`
   - `.skills/template-upgrader/SKILL.md`
   - `Drafts/00_灵感索引.md`

4. 更新说明文件。
   - `AGENTS.md` 增加模板协议自检、项目内 skills、Draft 主题归组与程序目录规则、chat-capture 触发规则、Release 后处理、Draft 状态和 Archive 新职责。
   - `Drafts/README.md` 增加无状态 Draft、主题归组、程序目录、AI 对话抓取、灵感索引和 Release 后处理说明。
   - `Releases/README.md` 增加来源与替代关系说明。
   - `Archive/README.md` 增加已吸收 Draft 可归档、默认不读 Archive 的说明。
   - `README.md` 增加 `.brain-template.json`、`.skills/`、`Drafts/00_灵感索引.md` 的结构说明。

5. 复核。
   - 确认 `AGENTS.md` 没有丢失项目专属约束。
   - 确认没有删除历史内容。
   - 确认 `.skills/draft-organizer/SKILL.md` 已存在，且协议明确禁止程序平铺在 `Drafts/` 根目录。
   - 确认 `.brain-template.json` 的 `template_authority`、`template_ref`、`protocol_version`、`protocol_released_at` 和 `managed_files` 与实际文件一致。
   - 用 git diff 检查升级只触及模板协议相关内容。

## `.brain-template.json` 建议内容

```json
{
  "template": "brain-template",
  "template_authority": "github.com/AaronJiTuo/brain-template",
  "template_ref": "main",
  "protocol_version": "1.3.0",
  "protocol_released_at": "2026-07-30",
  "managed_files": [
    "AGENTS.md",
    "README.md",
    "Releases/README.md",
    "Drafts/README.md",
    "Drafts/00_灵感索引.md",
    "Archive/README.md",
    ".brain-template.json",
    ".skills/chat-capture/SKILL.md",
    ".skills/draft-organizer/SKILL.md",
    ".skills/release-organizer/SKILL.md",
    ".skills/template-upgrader/SKILL.md"
  ]
}
```

`template_authority` 指向 GitHub 上的 `AaronJiTuo/brain-template`，这是模板协议的权威来源。`template_ref` 默认使用 `main`。升级判断只比较标准 semver 字段 `protocol_version`；`protocol_released_at` 只表示该协议版本发布日期，不参与版本大小判断。

## 旧项目 bootstrap

旧 `*-brain` 项目不会因为模板仓库升级而自动知道新协议。第一次升级需要用户或 agent 明确触发，例如：

```text
请按最新 brain-template 协议升级当前 brain，读取 GitHub AaronJiTuo/brain-template 仓库中 .skills/template-upgrader/SKILL.md 执行。
```

完成 bootstrap 后，该项目未来就能通过 `.brain-template.json` 和本 skill 做后续自检与升级。

## 禁止事项

- 不要把模板仓库的 `README.md` 整篇覆盖到已项目化的 brain。
- 不要删除、重命名或批量移动旧 Draft。
- 升级协议本身不要顺手重组旧 Draft；后续仅在创建或整理相关 Draft 时按 `draft-organizer` 的触发规则归组。
- 不要替用户判断旧 Releases 已过期并自动归档。
- 不要改写项目事实，只升级流程协议。
