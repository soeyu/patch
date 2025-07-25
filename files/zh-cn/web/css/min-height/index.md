---
title: min-height
slug: Web/CSS/min-height
---

CSS 属性 **`min-height`** 能够设置元素的最小高度。这样能够防止 {{cssxref("height")}} 属性的[应用值](/zh-CN/docs/Web/CSS/CSS_cascade/Value_processing#应用值)小于 `min-height` 的值。

{{InteractiveExample("CSS Demo: min-height")}}

```css interactive-example-choice
min-height: 150px;
```

```css interactive-example-choice
min-height: 7em;
```

```css interactive-example-choice
min-height: 75%;
```

```css interactive-example-choice
min-height: 10px;
```

```html interactive-example
<section class="default-example" id="default-example">
  <div class="transition-all" id="example-element">
    This is a box where you can change the minimum height. <br />If there is
    more content than the minimum the box will grow to the height needed by the
    content.
  </div>
</section>
```

```css interactive-example
#example-element {
  display: flex;
  flex-direction: column;
  background-color: #5b6dcd;
  justify-content: center;
  color: #ffffff;
}
```

当 `min-height` 大于 {{cssxref("max-height")}} 或 {{cssxref("height")}} 时，元素的高度会设置为 `min-height` 的值。

## 语法

```css
/* <长度> 数值 */
min-height: 3.5em;

/* <百分比> 数值 */
min-height: 10%;

/* 关键词 */
min-height: max-content;
min-height: min-content;
min-height: fit-content(20em);

/* 全局数值 */
min-height: inherit;
min-height: initial;
min-height: unset;
```

### Values

- {{cssxref("&lt;length&gt;")}}
  - : 定义 `min-height` 为一个绝对数值。
- {{cssxref("&lt;percentage&gt;")}}
  - : 定义 `min-height` 为一个相对于父容器高度的百分数。
- `auto`
  - : 浏览器将通过计算为指定元素选择一个 `min-height` 值。
- `max-content`
  - : The intrinsic preferred `min-height`.
- `min-content`
  - : The intrinsic minimum `min-height`.
- `fit-content({{cssxref("&lt;length-percentage&gt;")}})`
  - : Uses the `fit-content` formula with the available space replaced by the specified argument, i.e. `min(max-content, max(min-content, argument))`.

## Formal definition

{{cssinfo}}

## Formal syntax

{{csssyntax}}

## 示例

### Setting min-height

```css
table {
  min-height: 75%;
}

form {
  min-height: 0;
}
```

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}

## 相关链接

- [The box model](/zh-CN/docs/Web/CSS/CSS_box_model/Introduction_to_the_CSS_box_model), {{cssxref("box-sizing")}}
- {{cssxref("height")}}, {{cssxref("max-height")}}
