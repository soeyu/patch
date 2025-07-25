---
title: キーボードで操作可能な JavaScript ウィジェット
slug: Web/Accessibility/Guides/Keyboard-navigable_JavaScript_widgets
original_slug: Web/Accessibility/Keyboard-navigable_JavaScript_widgets
l10n:
  sourceCommit: eda72fa0408af18fba06416b616308084c903fee
---

{{AccessibilitySidebar}}

### 概要

ウェブアプリケーションは、メニュー、ツリービュー、リッチテキストフィールド、タブパネルなどのデスクトップウィジェットを模倣するために JavaScript を使用することがよくあります。 これらのウィジェットは通常、 {{ HTMLElement("div") }} 要素や {{ HTMLElement("span") }} 要素で構成されています。これらの要素は本来、デスクトップのものと同じキーボード機能を提供しません。 このドキュメントは JavaScript ウィジェットをキーボードでアクセス可能にするためのテクニックを説明します。

### tabindex を使う

デフォルトでは、人々がウェブページを閲覧するために <kbd>Tab</kbd> キーを使うとき、（リンクやフォームコントロールのような）インタラクティブ要素だけがフォーカスされます。 [`tabindex`](/ja/docs/Web/HTML/Reference/Global_attributes/tabindex) [グローバル属性](/ja/docs/Web/HTML/Reference/Global_attributes)を使うと、作成者は他の要素もフォーカス可能にできます。 `0` に設定すると、要素はキーボードとスクリプトによってフォーカス可能になります。 `-1` に設定すると、要素はスクリプトによってフォーカス可能になりますが、キーボードによるフォーカスの順序の一部にはなりません。

キーボードを使用したときに要素がフォーカスを得る順序は、デフォルトではソースの順序です。 例外的な状況では、作成者は順序を再定義したいと思うかもしれません。 これを行うために、作成者は `tabindex` を任意の正数に設定することができます。

> [!WARNING]
> `tabindex` に正の値を使わないでください。 `tabindex` に正の値を持つ要素は、ページ上のデフォルトのインタラクティブ要素の前に配置されます。 つまり、ページ作成者は、`tabindex` に 1 以上の正の値を使用する時は必ず、ページ上の全てのフォーカス可能要素に対して `tabindex` の値を設定（および維持）する必要があります。

次の表は、最新のブラウザーにおける `tabindex` の動作を説明しています。

<table>
  <thead>
    <tr>
      <th><code>tabindex</code> 属性</th>
      <th>JavaScript で <code>element.focus()</code> を介してフォーカス可</th>
      <th>タブで移動可</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>存在しない</td>
      <td>要素のプラットフォームの習慣に従う（フォームコントロールやリンクなどでは可）。</td>
      <td>要素のプラットフォームの習慣に従う。</td>
    </tr>
    <tr>
      <td>負（すなわち <code>tabindex="-1"</code>）</td>
      <td>可</td>
      <td>不可。矢印キーなどの押下に応答して <a href="/ja/docs/Web/API/HTMLElement/focus"><code>focus()</code></a> で要素をフォーカスさせる必要があります。</td>
    </tr>
    <tr>
      <td>Zero （すなわち <code>tabindex="0"</code>)</td>
      <td>可</td>
      <td>文書内の要素の位置からの相対的なタブ順序（なお、 {{HTMLElement('a')}} のような操作可能な要素は既定でこの動作をするので、この属性は必要ありません。）。</td>
    </tr>
    <tr>
      <td>Positive (e.g. <code>tabindex="33"</code>)</td>
      <td>可</td>
      <td><code>tabindex</code> の値は、この要素がタブ順序のどこに位置するかを決定します。小さい値を指定すると、大きい値よりタブ順序の早い位置に要素を配置します（例えば、<code>tabindex="7"</code> は <code>tabindex="11"</code> より前に配置されます）。</td>
    </tr>
  </tbody>
</table>

#### ネイティブでないコントロール

{{ HTMLElement("a") }}、{{ HTMLElement("input") }}、{{ HTMLElement("select") }} のような操作可能なネイティブ HTML 要素はすでにキーボードによりアクセス可能であるため、これらのいずれかを使用することが、コンポーネントをキーボードで使えるようにするための最速の方法です。

{{ HTMLElement("div") }} や {{ HTMLElement("span") }} をキーボードでアクセス可能にするには、 `tabindex` を `0` で追加することによって実現できます。これは、 HTML には存在しない操作可能な要素を使用するコンポーネントに特に役立ちます。

#### コントロールのグループ化

メニュー、タブリスト、グリッド、ツリービューなどのウィジェットをグループ化するには、親要素をタブ順序に入れて（`tabindex="0"`）、各子孫の選択項目/タブ/セル/行をタブ順序から除く（`tabindex="-1"`）必要があります。 ユーザーは矢印キーを使って子孫要素に移動できるようにすべきです。 （典型的なウィジェットに通常期待されるキーボードサポートの詳細な説明については、[WAI-ARIA のオーサリングプラクティス](https://www.w3.org/TR/wai-aria-practices-1.1/)（英語）を参照してください。）

以下の例は、入れ子になったメニューコントロールで使用されるこのテクニックを示しています。 キーボードフォーカスが含まれている {{ HTMLElement("ul") }} 要素に到達したら、JavaScript 開発者はプログラム的にフォーカスを管理し、矢印キーに応答する必要があります。 ウィジェット内でフォーカスを管理するためのテクニックについては、以下の「グループ内のフォーカス管理」を参照してください。

```html
<ul id="mb1" tabindex="0">
  <li id="mb1_menu1" tabindex="-1">
    フォント
    <ul id="fontMenu" title="フォント" tabindex="-1">
      <li id="sans-serif" tabindex="-1">サンセリフ</li>
      <li id="serif" tabindex="-1">セリフ</li>
      <li id="monospace" tabindex="-1">モノスペース</li>
      <li id="fantasy" tabindex="-1">ファンタジー</li>
    </ul>
  </li>
  <li id="mb1_menu2" tabindex="-1">
    スタイル
    <ul id="styleMenu" title="スタイル" tabindex="-1">
      <li id="italic" tabindex="-1">斜体</li>
      <li id="bold" tabindex="-1">太字</li>
      <li id="underline" tabindex="-1">下線</li>
    </ul>
  </li>
  <li id="mb1_menu3" tabindex="-1">
    位置揃え
    <ul id="justificationMenu" title="位置揃え" tabindex="-1">
      <li id="left" tabindex="-1">左</li>
      <li id="center" tabindex="-1">中央</li>
      <li id="right" tabindex="-1">右</li>
      <li id="justify" tabindex="-1">両端</li>
    </ul>
  </li>
</ul>
```

#### 無効なコントロール

カスタムコントロールが無効になったら、`tabindex="-1"` を設定して、タブ順序から除いてください。 グループ化されたウィジェット内の無効な項目（メニュー内のメニュー項目など）は、矢印キーを使用して移動可能なままにするべきであることに注意してください。

### グループ内のフォーカス管理

ユーザーがウィジェットからタブで離れてから戻ると、フォーカスはフォーカスを持っていた特定の要素、例えば、ツリーアイテムやグリッドセルに戻るべきです。 これを実現するには次の 2 つのテクニックがあります。

1. 動き回る `tabindex`: プログラム的にフォーカスを移動
2. `aria-activedescendant`: 「仮想」フォーカスの管理

#### テクニック 1: 動き回る tabindex

フォーカスされた要素の `tabindex` を `"0"` に設定すると、ユーザーがウィジェットからタブで離れてから戻ってきた場合でも、グループ内の選択された項目にフォーカスが保持されます。 `tabindex` を "0" に更新したら、以前に選択した項目を `tabindex="-1"` に更新する必要もあることに注意してください。 このテクニックでは、キーイベントに応答してプログラムでフォーカスを移動し、現在フォーカスされた項目を反映するように `tabindex` を更新します。 次を行います。

キーダウンハンドラーをグループ内の各要素にバインドし、矢印キーを使用して別の要素に移動した場合、

1. プログラム的に新しい要素にフォーカスを適用します。
2. フォーカスされた要素の `tabindex` を "0" に更新します。
3. 以前にフォーカスされた要素の `tabindex` を "-1" に更新します。

これはこのテクニックを使った [WAI-ARIA ツリービュー](https://files.paciellogroup.com/training/WWW2012/samples/Samples/aria/tree/index.html)の例です。

### ヒント

#### element.focus() を使ってフォーカスを設定する

要素にフォーカスを移すために `createEvent()`、`initEvent()`、および `dispatchEvent()` を使用しないでください。 DOM フォーカスイベントは情報提供のみを目的としています。 何かがフォーカスされた後にシステムによって生成されますが、実際にはフォーカスの設定には使用されません。 代わりに `element.focus()` を使用してください。

#### onfocus を使って現在のフォーカスを追跡する

全てのフォーカスの変更がキーイベントやマウスイベントを介して行われるとは限りません。 スクリーンリーダーなどの支援技術では、フォーカスを任意のフォーカス可能な要素に設定できます。 代わりに `onfocus` と `onblur` を使ってフォーカスを追跡します。

`onfocus` と `onblur` は全ての要素で使用できるようになりました。 現在のドキュメントのフォーカスを取得するための標準的な DOM インターフェイスはありません。 フォーカスの状態を追跡したい場合は、[document.activeElement](/ja/docs/Web/API/Document/activeElement) を使ってアクティブな要素を取得できます。 [document.hasFocus](/ja/docs/Web/API/Document/hasFocus) を使って、現在のドキュメントのフォーカスかどうかを確認することもできます。

### テクニック 2: `aria-activedescendant`

このテクニックでは、単一のイベントハンドラーをコンテナーウィジェットにバインドし、`aria-activedescendant` を使用して「仮想」フォーカスを追跡します。 （ARIA に関する詳細は、[アクセス可能なウェブアプリケーションとウィジェットの概要](/ja/docs/Web/Accessibility/Guides/Accessible_web_applications_and_widgets)を参照してください。）

`aria-activedescendant` プロパティは、現在仮想フォーカスを持っている子孫要素の ID を識別します。 コンテナーのイベントハンドラーは、`aria-activedescendant` の値を更新し、（例えば、境界線や背景色で）現在の項目が適切にスタイル設定されていることを確実にすることで、キーイベントおよびマウスイベントに応答する必要があります。

### 一般的なガイドライン

#### onkeypress ではなく onkeydown を使ってキーイベントをトラップする

IE は、英数字以外のキーに対する `keypress` イベントを発生させません。 代わりに `onkeydown` を使用してください。

#### キーボードとマウスが同じ操作を生み出すことを確実にする

ユーザーの操作が入力機器に関係なく一貫していることを保証するために、キーボードとマウスのイベントハンドラーは適切な場所でコードを共有するべきです。 例えば、ユーザーが矢印キーを使用して移動したときに `tabindex` やスタイルを更新するコードは、マウスクリックハンドラーでも同じ変化を生み出すために使用するべきです。

#### キーボードを使用して要素をアクティブにできることを確実にする

キーボードを使用して要素をアクティブにできることを確実にするには、マウスイベントにバインドされているハンドラーもキーボードイベントにバインドするべきです。 例えば、`onclick="doSomething()"` がある場合、<kbd>Enter</kbd> キーで要素がアクティブにできることを確実にするには、`onkeydown="return event.keyCode !== 13 || doSomething();"` で `doSomething()` をキーダウンイベントにもバインドするべきです。

#### tabindex="-1" の項目およびプログラムでフォーカスを受け取る要素に常にフォーカスを描画する

プログラム的にフォーカスを受け取る項目には、IE は自動的にフォーカスの輪郭を描画しません。 背景色を `this.style.backgroundColor = "gray";` のようなもので変更するか、または `this.style.border = "1px dotted invert"` で点線の境界線を追加することの中から選らんでください。 点線の境界線の場合は、最初から見えない 1px の境界線があるようにして、境界線スタイルを適用しても要素が大きくならないようにする必要があります（境界線はスペースを取り、IE は CSS アウトラインを実装しません）。

#### 使用したキーイベントがブラウザー機能を実行しないようにする

ウィジェットがキーイベントを処理する場合は、イベントハンドラーのリターンコードを使用して、ブラウザーもそれを処理しないようにします（例えば、矢印キーに応答してスクロールするなど）。 イベントハンドラーが `false` を返した場合、そのイベントはハンドラーを超えて伝播しません。

例えば、

```html
<span tabindex="-1" onkeydown="return handleKeyDown();">…</span>
```

`handleKeyDown()` が `false` を返すと、イベントは消費され、ブラウザーはキーストロークに基づいたアクションを実行できなくなります。

#### 現時点では、キーリピートに一貫した動作を当てにしないこと

残念ながら `onkeydown` は、使用しているブラウザーや OS によってはリピートするかもしれないし、しないかもしれません。
