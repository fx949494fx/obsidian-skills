# 与 fx949494fx/literature-skills 的集成

## 定位

`fx949494fx/literature-skills` 是医学文献综述流水线的数据层和写作层，负责检索、筛选、Zotero 导入计划、evidence packet、精读强化、综述框架和初稿。

`fx949494fx/obsidian-skills` 是 Obsidian vault 内的知识沉淀层，负责文献笔记、wikilink、MOC、概念页、证据矩阵、论文论点、Canvas 和 Bases。

两者可以串联，但不应互相替代。

## 推荐串联方式

```text
literature-search
  -> paper-screening-score
  -> zotero-manager
  -> paper-metadata-extractor
  -> paper-deep-reading
  -> review-framework-builder
  -> review-draft-writer
  -> obsidian-literature-notes
  -> obsidian-literature-synthesis
  -> obsidian-bases / json-canvas
```

## 可直接消费的上游文件

从 `literature-skills` 接收：

- `papers.jsonl`
- `screening_scores.jsonl`
- `zotero_items.jsonl`
- `paper_evidence_packets.jsonl`
- `deep_reading_evidence_packets.jsonl`
- `evidence_claims.csv`
- `review_framework.md`
- `evidence_allocation_matrix.csv`
- `review_draft.md` 或 `review_body.md`

其中 `deep_reading_evidence_packets.jsonl` 优先级高于 `paper_evidence_packets.jsonl`。

## 字段映射

| literature-skills | Obsidian |
|---|---|
| `paper_id` | `paper_id` |
| `pmid` | `pmid` |
| `doi` | `doi` |
| `include_decision` / `screening_decision` | `decision` |
| `total_score` / `screening_score` | `screening_score` |
| `review_contribution` | `review_contribution` |
| `contribution_points` | 文献笔记中的“对本课题的启发”或“具体贡献” |
| `usable_claims` | `03 Synthesis/论文讨论可用论点.md` 的候选证据句 |
| `needs_full_text_check` | `needs_full_text_check` |
| `citation_placeholder` | 文献笔记和论点页引用占位 |
| `zotero_item_key` | `zotero_item_key` |
| `zotero_attachment_key` | `zotero_attachment_key` |

## Vault 分流

- `decision: Include` 且 `evidence_basis: full_text` 或已完成精读：进入 `01 Papers`。
- `decision: Include` 但 `needs_full_text_check: true`：进入 `00 Inbox`。
- `decision: Maybe`：进入 `00 Inbox`。
- `decision: Need Full Text`：进入 `00 Inbox`。
- `decision: Exclude`：进入 `Other` 或只保留审查表。

## 知识提炼规则

当上游来自 `literature-skills` 时，`obsidian-literature-synthesis` 应优先使用：

1. `review_contribution` 作为文献的一句话证据角色。
2. `contribution_points` 作为概念页和证据矩阵候选条目。
3. `usable_claims` 作为论文讨论论点页的候选表述。
4. `evidence_allocation_matrix.csv` 作为章节分配和 MOC 证据地图的初始骨架。

不要把 `review_draft.md` 当作事实来源。综述初稿可作为写作结构参考，事实和证据仍应回到文献笔记、evidence packets 和全文核验结果。

## 增量维护

处理新增上游文献时：

1. 用 `paper_id`、PMID、DOI 检查 vault 中是否已有笔记。
2. 新文献只追加到受影响的概念页、证据矩阵和论文论点页。
3. 保留人工已经修改过的 MOC、概念定义和论点表述。
4. 对 `needs_full_text_check: true` 的观点，不写成定稿级结论。
5. 更新 Canvas/Bases 时校验文件路径、wikilink 和 frontmatter 字段。

## 质量检查

- `01 Papers` 中的文献应至少有 `review_contribution` 或完整精读小节。
- 概念页中的证据应链接到 `01 Papers`，不要只复制摘要。
- 证据矩阵应覆盖所有已纳入且与主题相关的 `01 Papers` 文献。
- 论文论点页应区分上游 evidence packet、全文事实和作者推论。
- Canvas 节点必须指向存在的文件。
