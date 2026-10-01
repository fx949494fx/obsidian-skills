# 配置与属性规范

## 模板配置

Obsidian 核心 Templates 插件的目录设为：

```json
{
  "folder": "05 Research Ideas/99 Templates",
  "dateFormat": "YYYY-MM-DD",
  "timeFormat": "HH:mm"
}
```

若 Vault 已有模板目录，不要直接替换；将 `Research Idea.md` 放入原模板目录，或由用户决定是否改为新的目录。

## 核心属性

| 属性 | 类型 | 规则 |
| --- | --- | --- |
| `type` | text | 固定为 `research-idea` |
| `title` | text | 简短、可检索，避免“想法1” |
| `created` | date | 首次捕捉日期 |
| `updated` | date | 实质更新日期 |
| `status` | text | `seed/developing/testing/parked/archived` |
| `maturity` | number | 1–5，表示成形程度 |
| `priority` | text | `low/medium/high` |
| `topics` | list | 1–3 个稳定主题词 |
| `source_type` | text | 如 `observation/reading/discussion/experiment/question` |
| `next_action` | text | 一个具体、可执行的最小行动 |
| `related_projects` | list | 项目笔记 Wikilink 列表 |
| `related_papers` | list | 论文笔记 Wikilink 列表 |
| `tags` | list | 必含 `research-idea` |

不要把 `maturity` 当作价值评分，也不要用 `priority` 暗示科学可信度。

## 最低内容标准

一条可用的灵感笔记至少包含：

1. 一句话想法。
2. 触发来源，允许是个人观察。
3. 下一步最小行动。
4. “证据与反证”占位，提醒后续验证。

## 质量检查

- frontmatter 是合法 YAML，日期无引号也能被识别为日期。
- `type`、`status`、`maturity` 使用约定值。
- 内部关联使用 Wikilink；外部来源使用 Markdown 链接。
- Base 只纳入 `type == "research-idea"` 的 Markdown 笔记。
- 模板、README、Hub 和 Base 自身不应出现在想法列表中。
