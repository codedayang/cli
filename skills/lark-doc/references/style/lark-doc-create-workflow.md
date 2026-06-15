# 创建文档操作流程

用户需要生成新的飞书文档时，按本流程选择命令和写入方式。

## 一、创建前

1. 确认使用 `--as user`，并固定传 `--api-version v2`。
2. 确认内容格式：
   - 用户提供 `.md` 文件或明确要求 Markdown 时，用 `--doc-format markdown`。
   - 其他情况默认 XML。
3. 确认创建位置：
   - 默认创建到当前用户空间。
   - 指定父文件夹或知识库节点时，使用 `--parent-token` 或 `--parent-position`。

## 二、写入内容

- XML 内容遵循 [`lark-doc-xml.md`](../lark-doc-xml.md)。
- Markdown 内容遵循 [`lark-doc-md.md`](../lark-doc-md.md)。
- 文档元素和文字样式用法见 [`lark-doc-style.md`](lark-doc-style.md)。
- 禁止使用行内代码块；代码片段使用 XML `<pre><code>...</code></pre>` 或 Markdown fenced code block。
- 可以使用加粗、下划线、文字颜色、文字背景色等文字样式提升可读性。

创建较长文档时，建议先创建标题和章节骨架，再用 `docs +update --command block_insert_after` 分段写入正文，避免单次 `--content` 过长。

`--content @file` 只接受当前工作目录下的相对路径。需要文件传参时，把文件放在 cwd 下，用完后自行清理。

## 三、创建后

1. 从返回值读取 `document.document_id` 和 `document.url`。
2. 如果返回 `document.new_blocks`，记录新增资源块的 `block_id` 和 `block_token`。
3. 后续继续编辑时，用 `docs +fetch --api-version v2 --detail with-ids` 获取最新 block ID。
4. 如果需要插入本地图片或附件，使用 `docs +media-insert`；如果使用网络图片，可在 XML 中写 `<img href="https://..."/>`。
