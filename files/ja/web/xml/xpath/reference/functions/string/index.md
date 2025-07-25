---
title: string
slug: Web/XML/XPath/Reference/Functions/string
l10n:
  sourceCommit: 3e1b5277c6451e7d27ab628f23fb9702947a7a7b
---

`string` 関数は与えられた引数を文字列に変換します。

## 構文

```plain
string( [object] )
```

### 引数

- `object` (省略可)
  - : 文字列に変換するオブジェクト。省略すると、コンテキストノードが使用されます。

### 返値

文字列。

## 解説

- オブジェクトがノード集合ならば、その集合内の最初のノードの文字列値が返されます。
- 数値は次のように変換されます。
  - NaN は文字列 NaN に変換されます。
  - 正のゼロは文字列 0 に変換されます。
  - 負のゼロは文字列 0 に変換されます。
  - 正の無限大は文字列 Infinity に変換されます。
  - 負の無限大は文字列 -Infinity に変換されます。
  - \-1 と 1 の間の小数は、小数点の前に 0 を 1 つ付けた形式で文字列に変換されます。
  - 論理値の true は文字列の true に変換されます。
  - 論理値の false は文字列の false に変換されます。

## 仕様書

[XPath 1.0 4.2](https://www.w3.org/TR/1999/REC-xpath-19991116/#function-string)

## Gecko の対応

対応済み。
