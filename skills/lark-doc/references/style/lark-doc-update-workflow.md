# 更新文档操作流程

用户提供已有文档链接或 token，需要修改、补充、删除或重排内容时，按本流程选择读取范围和更新命令。

## 一、读取范围

按目标范围选择 `docs +fetch --api-version v2` 的 `--scope`，避免无必要地读取全文：

| 目标 | 读取方式 |
|------|----------|
| 只改某一节 | `--scope outline --max-depth 2` 找标题，再 `--scope section --start-block-id <标题id> --detail with-ids` |
| 精确跨节区间 | `--scope range --start-block-id xxx --end-block-id yyy --detail with-ids` |
| 只知道关键词 | `--scope keyword --keyword xxx --context-before 1 --context-after 1 --detail with-ids` |
| 明确改整篇 | `--detail with-ids` |

详见 [`lark-doc-fetch.md`](../lark-doc-fetch.md) 的 `--scope` 说明。

## 二、选择更新命令

| 目标 | 命令 |
|------|------|
| 替换一段普通文本 | `str_replace` |
| 在指定 block 后插入内容 | `block_insert_after` |
| 替换指定 block | `block_replace` |
| 删除指定 block | `block_delete` |
| 复制已有 block | `block_copy_insert_after` |
| 移动已有 block | `block_move_after` |
| 文档末尾追加 | `append` |
| 全文重建 | `overwrite` |

优先使用 block 级命令做精确修改。只有用户明确要求完全重建文档时，才使用 `overwrite`。

## 三、写入内容

- XML 内容遵循 [`lark-doc-xml.md`](../lark-doc-xml.md)。
- Markdown 内容遵循 [`lark-doc-md.md`](../lark-doc-md.md)。
- 文档元素和文字样式用法见 [`lark-doc-style.md`](lark-doc-style.md)。
- 禁止使用行内代码块；代码片段使用 XML `<pre><code>...</code></pre>` 或 Markdown fenced code block。
- 可以使用加粗、下划线、文字颜色、文字背景色等文字样式提升可读性。

改写已有内容时，原文里的 `<cite type="user">`、`<cite type="doc">`、`<img>`、`<source>`、`<whiteboard>`、`<sheet>`、`<bitable>`、`<synced_reference>` 等行内组件和资源块应保留其 token / user-id / doc-id 等属性。不要替换成纯文本姓名、链接或占位符。

## 四、更新后

1. 如果后续还要继续按 block ID 操作，重新 `docs +fetch --api-version v2 --detail with-ids` 获取最新 block ID。
2. `block_replace` 后旧 block ID 不保证继续可用。
3. 新增画板、图片等资源块时，从返回值的 `document.new_blocks` 中读取 `block_id` 和 `block_token`。
