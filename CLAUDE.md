# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 最高原则

- 无论任务类型如何，Claude Code 都必须使用中文与用户交流，并使用中文登记新增内容、设定和工作记录；文件名、既有专有名词或代码语法需要保留原文时除外。
- 未经用户明确要求，不得擅自代替用户运行或执行 `git diff`、`git status`、`git log` 及其他同类 Git 检查命令。需要用户检查时，只说明建议执行的命令，由用户在本地手动完成。
- 在回答任何问题、提出设定判断或编辑任何文件前，必须先查询与当前问题相关的现行世界观知识库：优先阅读 `README.md`、`docs/00-设定索引.md` 及相关 `docs/` 页面，并以查到的内容为依据。不得把模型记忆、聊天上下文或 `archive/` 内容当作现行事实；资料没有明确记载时，必须说明“未知”“待定”或“需要用户确认”，不得擅自编造、推断或扩展用户的世界观。

## 子 Agent 路由

- 凡需要与用户探讨世界观、比较方案、处理正典冲突、进行设定决策或形成大纲，必须委派 `setting-architect`，使用 `gpt-5.6-sol`。
- 凡需要广泛读取上下文，或依据已确认大纲进行具体撰写、改写和文档整合，必须委派 `context-writer`，使用 `gpt-5.6-terra`。
- 凡结果明确且不涉及语义判断的格式、链接、索引、元数据、检查和精确替换任务，委派 `mechanical-editor`，使用 `gpt-5.6-luna`。
- 不得因为任务看似简单而让 `context-writer` 或 `mechanical-editor` 作出设定决策。
- `context-writer` 或 `mechanical-editor` 发现未决事项、资料冲突或多个合理方案时，必须停止并上报，不得自行补全。
- 涉及世界观的完整流程默认采用：`context-writer` 检索并整理上下文 → `setting-architect` 与用户讨论并确定大纲 → `context-writer` 具体撰写 → `mechanical-editor` 完成机械检查和收尾。
- 如果用户已经给出明确、完整且不需要进一步取舍的决策或大纲，可以直接从 `context-writer` 阶段开始；纯机械任务可以直接使用 `mechanical-editor`。

## Repository Overview

《白色高塔》 is a Chinese-language worldbuilding and setting repository. It is a Markdown knowledge base, not an application: there is no package manager, build system, executable, or automated test suite in the repository.

The current story baseline is 332 years after the 点燃历史 event, with a human population of approximately 800 million. The repository's canonical source is the current material under `docs/`; `archive/` contains historical material for traceability only.

## Common Commands

Run these from the repository root (`The-White-Tower`):

```powershell
# Inspect the working tree
git status --short

# Review recent history
git log -5 --oneline --decorate

# Review unstaged and staged changes
git diff
git diff --cached

# Check for whitespace errors
git diff --check
```

There are currently no configured build, lint, or test commands. Do not claim that a test suite passed; for documentation changes, use `git diff --check` and inspect the rendered Markdown links and headings as the available validation. A single-test command does not exist because no test framework is configured.

## Structure and Architecture

- `README.md` is the repository-level orientation: current baseline, navigation, authority rules, and maintenance principles.
- `docs/00-设定索引.md` is the main content index and records the authority order for current, tentative, discussion-only, and archived material.
- `docs/01-世界观/` defines the world model, anomaly generation, epistemic limits, and the three-dimensional anomaly evaluation system.
- `docs/02-历史/` contains the history of the pre-civilization anomaly era, the White Tower, 历史啃噬, 点燃历史, year 0, and post-event reconstruction. `白色高塔与点燃历史.md` is a navigation document for the split history volume.
- `docs/03-地理/` contains global geography, anomalous geology, and location dossiers such as 摩柯 and 薄雾群屿.
- `docs/04-神秘学/` contains the magic-user system, casting model, color traditions, education and regulation, civilization roles, anomalous astronomy, and 禁绝金属. `神秘使体系.md` is a navigation document for the split system volume.
- `docs/05-人物/` contains character profiles. Character pages link to related anomaly dossiers where applicable.
- `docs/06-异常档案/` contains anomaly dossiers. `档案说明.md` defines the minimum dossier format, three evaluation dimensions, temporary tag conventions, and rights/consent principles for anomalous persons.
- `archive/` contains superseded worldbuilding and character material. Its contents must not be used as current canon; `archive/README.md` explains the purpose of the archive.
- `CHANGELOG.md` records dated setting changes and major reorganizations.

The documentation is organized as a linked knowledge base rather than as independent essays. When changing a concept, inspect its index entry, navigation page, related character/location/anomaly pages, and relevant changelog entry so links and cross-document terminology stay coherent.

## Canon and Uncertainty Rules

- Treat an explicit, current statement in `docs/` as authoritative over older material.
- Preserve the distinction between author-confirmed fact, a White Tower engineering model, an academic hypothesis, and an unknown/open question. The world model explicitly does not provide a final explanation of why meaning can affect reality.
- When documents conflict and the repository does not establish a resolution, mark the issue as `待定` or record it for discussion. Do not silently infer a new canon answer.
- Do not resurrect retired systems from the archive, including the old Reality/Distortion, fixed memetic-depth, or `Kt`/`Oc` frameworks, unless the user explicitly requests historical analysis.
- Distinguish engineering ratings from metaphysical truth. Anomaly `强度`, `外显效力`, and `危害度` are separate measures and must not be treated as interchangeable.
- An anomalous person remains an autonomous person. A dossier is not, by itself, a declaration of containment, ownership, loss of legal status, or consent to research.

## Authoring Conventions

- Write new canon into the appropriate topic file under `docs/`; do not treat chat history as the formal record.
- Preserve the existing Chinese titles, filenames, directory numbering, and relative Markdown link style. Use the repository's established full-width bracket notation for confirmed anomaly tags, such as `【智能】`.
- Keep navigation pages as navigation pages. Put substantive setting changes in the relevant split topic document and update the index when adding a new document.
- Keep document metadata consistent with neighboring pages: a status line, a `最后更新` date, and explicit labels such as `现行设定`, `暂定`, or `现行工作格式` where appropriate.
- For anomaly dossiers, follow `docs/06-异常档案/档案说明.md`: summary, three-dimensional evaluation, handling measures, description/mechanism, boundaries, and handling history or appendices as applicable. Use `未定`, `未单独定级`, or `证据不足` instead of filling gaps with speculation.
- Move superseded material into `archive/` rather than deleting it, and update archive documentation when the archive structure changes.
- Update `CHANGELOG.md` for meaningful setting changes, new dossiers, reorganizations, and retcons. Keep a focused commit to one topic when practical.
- `.gitattributes` requires Markdown files to use LF line endings. Preserve that convention when editing files, even on Windows.

## Change Workflow

1. Start at `README.md` and `docs/00-设定索引.md` to identify the canonical location and related pages.
2. Read the target document and its directly linked navigation, character, location, or anomaly documents before editing.
3. Make the smallest coherent set of Markdown changes; update cross-links, metadata, and the changelog when the change warrants them.
4. Run `git diff --check`, inspect `git diff`, and verify that changed relative links still point to existing files.
5. Keep archived material separate from current `docs/` content and leave unresolved questions explicitly marked rather than resolving them by assumption.
