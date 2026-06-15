# lark-doc 画板处理指南

> **前置条件：** 先阅读 [`../../lark-shared/SKILL.md`](../../lark-shared/SKILL.md) 了解认证、全局参数和安全规则。

本文件只说明文档内 `<whiteboard>` 元素的插入和编辑方式。

## 两个 Skill 的职责边界

| Skill | 职责 |
|-------|------|
| `lark-doc` | 在文档中新增 `<whiteboard>` 块，支持 `mermaid`、`plantuml`、`svg`、`blank` 类型 |
| `lark-whiteboard` | 查询、导出、编辑已有画板，或向空白画板写入复杂内容 |

## 画板类型选择

| 需求 | 写法 |
|------|------|
| 插入 Mermaid 支持的图，如思维导图、时序图、类图、饼图、甘特图 | `<whiteboard type="mermaid">...</whiteboard>` |
| 插入 PlantUML 图 | `<whiteboard type="plantuml">...</whiteboard>` |
| 插入自包含 SVG 图 | `<whiteboard type="svg"><svg ...>...</svg></whiteboard>` |
| 创建空白画板，后续交给 `lark-whiteboard` 写入 | `<whiteboard type="blank"></whiteboard>` |
| 编辑已有画板 | 先 `docs +fetch --api-version v2` 获取 `board_token`，再切到 `lark-whiteboard` |
| 查看或下载已有画板 | 切到 `lark-whiteboard` |

如果同一篇文档需要多个画板，分别为每个画板选择类型；不同画板可以使用不同写法。

## 使用 Mermaid 插入画板

```xml
<whiteboard type="mermaid">
mermaid 代码...
</whiteboard>
```

插入后从 `document.new_blocks` 获取新增画板的 `block_id` 和 `block_token`。

## 使用 PlantUML 插入画板

```xml
<whiteboard type="plantuml">
@startuml
Alice -> Bob: hello
@enduml
</whiteboard>
```

## 使用 SVG 插入画板

主 Agent 可以启动 SubAgent 生成并插入一个 SVG 画板。SubAgent 需要携带以下最小上下文：

- doc token、插入位置（标题 / block_id / command）
- 图表目标、源段落或数据
- [`lark-doc-xml.md`](lark-doc-xml.md) 路径
- 本文件的 SVG 约束

写入内容：

```xml
<whiteboard type="svg">
  <svg viewBox="0 0 1200 800" xmlns="http://www.w3.org/2000/svg">
    ...
  </svg>
</whiteboard>
```

SVG 必须完整自包含：包含 `<svg>` 根节点和 `viewBox`，不引用外部图片、脚本或远程资源。

## SVG 兼容约束

画板的 SVG parser 会把可识别元素转成可编辑节点，其余元素可能降级为内嵌图片。为减少渲染问题，优先使用下列元素。

**可识别的元素**

- 形状：`<rect>`、`<circle>`、`<ellipse>`、`<polygon>`
- 连线：`<line>`、`<polyline>`、`<path>`（自动识别为直线、折线或曲线）
- 文本：`<text>`、`<tspan>`；文字必须用 `<text>` 表达，不要把文字转成 path
- 分组：`<g>`、`<a>`、`<use>` 引用 `<symbol>`
- 变换：`translate`、`rotate`、`scale`

**避免使用的特性**

- `<radialGradient>`、`<filter>`、`<pattern>`、`<clipPath>`、`<mask>` 可能导致画板渲染问题
- 尽量避免 `skewX`、`skewY`、`matrix(...)` 这类空间扭曲变换
- 不要引用外部字体、外部图片、脚本或远程资源

## 编辑已有画板

`docs +update` 不能直接编辑已有画板的内部内容。编辑已有画板时：

1. 用 `docs +fetch --api-version v2 --doc <doc> --detail with-ids` 读取文档。
2. 找到目标 `<whiteboard token="...">` 的 token。
3. 切到 [`../../lark-whiteboard/SKILL.md`](../../lark-whiteboard/SKILL.md)，按该 skill 的流程查询、更新或导出画板。

## 完成校验

- Mermaid：确认插入的是 `<whiteboard type="mermaid">`，且 Mermaid 内容完整。
- PlantUML：确认插入的是 `<whiteboard type="plantuml">`，且 PlantUML 内容完整。
- SVG：确认插入的是 `<whiteboard type="svg">`，且内部是完整 `<svg ...>...</svg>`。
- Blank：确认后续已用 `lark-whiteboard` 写入内容；不保留无用途的空白画板。

## 关联参考

- 画板查询、导出、修改：[`../../lark-whiteboard/SKILL.md`](../../lark-whiteboard/SKILL.md)
