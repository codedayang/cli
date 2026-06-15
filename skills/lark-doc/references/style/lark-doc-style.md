# 文档元素用法参考

本文件只说明飞书文档元素和文字样式的用法。具体内容结构以用户需求为准。

## 一、元素选择

| 需要表达的内容 | 可用元素 |
|----------------|----------|
| 普通段落、标题、列表 | `<p>`、`<h1>`-`<h9>`、`<ul>`、`<ol>`、`<li>` |
| 摘要、说明、注意事项 | `<callout>` |
| 多列并排内容 | `<grid>` + `<column>` |
| 结构化数据、指标、字段说明 | `<table>`、`<thead>`、`<tbody>`、`<tr>`、`<th>`、`<td>` |
| 任务、检查项 | `<checkbox>` |
| 代码片段 | `<pre lang="x" caption="说明"><code>...</code></pre>` |
| 引用、公式 | `<blockquote>`、`<latex>` |
| 链接、预览卡片、操作入口 | `<a>`、`<a type="url-preview">`、`<button>` |
| 图片、附件 | `<img>`、`<source>` |
| 图表、流程图、架构图、示意图 | `<whiteboard>` |
| @人、@文档 | `<cite type="user">`、`<cite type="doc">` |

## 二、文字样式

可以使用文字样式丰富文档展示、提升可读性：

| 效果 | XML 写法 |
|------|----------|
| 加粗 | `<b>重点文本</b>` |
| 下划线 | `<u>需要关注的文本</u>` |
| 斜体 | `<em>强调文本</em>` |
| 删除线 | `<del>废弃文本</del>` |
| 文字颜色 | `<span text-color="green">绿色文本</span>` |
| 文字背景色 | `<span background-color="light-yellow">带背景的文本</span>` |

行内样式标签必须按固定顺序嵌套（外 -> 内），关闭顺序反转：

`<a> -> <b> -> <em> -> <del> -> <u> -> <span> -> 文本内容`

禁止使用行内代码块 / inline code：

- Markdown 中不要用单反引号包裹文本。
- XML 中 `<code>` 只能作为 `<pre>` 的子标签表示代码块，不能作为行内样式使用。

## 三、颜色属性

颜色优先使用命名色，也可写 `rgb(r,g,b)` / `rgba(r,g,b,a)`。

基础色：`red`、`orange`、`yellow`、`green`、`blue`、`purple`、`gray`

| 属性 | 支持的命名色 |
|------|--------------|
| 文字颜色 `<span text-color>` | 基础色 |
| 高亮框字色 `<callout text-color>` | 基础色 |
| 高亮框边框 `<callout border-color>` | 基础色 |
| 文字背景 `<span background-color>` | 基础色 + `light-{色}` + `medium-gray` |
| 高亮框填充 `<callout background-color>` | `gray` + `light-{色}` + `medium-{色}` |
| 单元格背景 `<th/td background-color>` | 同文字背景 |
| 按钮背景 `<button background-color>` | 同文字背景 |

## 四、画板元素

涉及图表或可视化内容时，按需求选择画板写法：

| 内容类型 | 可用方式 |
|----------|----------|
| 思维导图、时序图、类图、饼图、甘特图 | `<whiteboard type="mermaid">...</whiteboard>` |
| PlantUML 支持的图 | `<whiteboard type="plantuml">...</whiteboard>` |
| 自定义 SVG 图形 | `<whiteboard type="svg"><svg ...>...</svg></whiteboard>` |
| 需要后续用 lark-whiteboard 写入的复杂画板 | `<whiteboard type="blank"></whiteboard>` |

具体插入流程和 SVG 约束见 [`lark-doc-whiteboard.md`](../lark-doc-whiteboard.md)。
