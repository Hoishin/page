# Web ComponentsとPolymer

## [Web Componentsとは](https://www.webcomponents.org/introduction)

> Web components are a set of web platform APIs that allow you to create new custom, reusable, encapsulated HTML tags to use in web pages and web apps. Custom components and widgets build on the Web Component standards, will work across modern browsers, and can be used with any JavaScript library or framework that works with HTML.

> Web components are based on existing web standards. Features to support web components are currently being added to the HTML and DOM specs, letting web developers easily extend HTML with new elements with encapsulated styling and custom behavior.

一言で言えばカオスなWebの世界をシンプルにしてくれるAPIたち

- フレームワークでもライブラリでもない、Web API
- 再利用性とカプセル化が特徴

### [Custom Elements](https://w3c.github.io/webcomponents/spec/custom/)

カスタムなHTMLタグを定義できる

```html
<flag-icon country="nl"></flag-icon>
```

### [Shadow DOM](https://w3c.github.io/webcomponents/spec/shadow/)

スタイリング、マークアップをカプセル化する

### [HTML Imports](https://w3c.github.io/webcomponents/spec/imports/)

再利用のために別のHTMLを持ってくる仕組み

### [HTML Template](https://html.spec.whatwg.org/multipage/scripting.html#the-template-element/)

スクリプトによってインスタンス化されて複製したりどこかに突っ込んだりできる「断片」で、レンダリング中は全く何もしないが、その後にスクリプトによって操作される

## 使い方

使うだけならものすごく簡単

```html
<link rel="import" href="../emoji-rain/emoji-rain.html">
...
<emoji-rain active></emoji-rain>
```

インストール方法はいろいろあるが、今現在は[諸事情](https://raw.githubusercontent.com/Hoishin/polymer-intro/master/img/bower.png)で未だにBowerが多い

## 作り方

```js
class AppDrawer extends HTMLElement {...}

customElements.define('app-drawer', AppDrawer);
```

Custom Elementは例えば標準の`<div>`と全く同じ使い方ができる

```html
<script>
// Create with javascript
var newDrawer = document.createElement('app-drawer');
// Add it to the page
document.body.appendChild(newDrawer);
// Attach event listeners
document.querySelector('app-drawer').addEventListener('open', function() {...});
</script>
```

## Shadow Rootを使う

Shadow Rootを使うとShadow DOMを生成でき、そのDOMの中身をカプセル化できる

```js
const header = document.createElement('header');
const shadowRoot = header.attachShadow({mode: 'open'});
shadowRoot.innerHTML = '<h1>Hello Shadow DOM</h1>';
```

主な利点はCSSセレクターが意図しないDOMを選んだりしないようになること

## 今流行のフレームワークとの違い

...React、Vue、Angularでも自分でタグを作ってるじゃん？

- 作っているつもりになれるだけで作ってない
- フレームワークでは再利用可能にしても、それぞれのフレームワーク専用になる
- Web Componentの規格に則れば、フレームワーク関係なくなんでも使える

## ブラウザ対応
規格|Chrome|Opera|Safari|Firefox|Edge
:-:|:-:|:-:|:-:|:-:|:-:
Custom Elements|Stable|Stable|Stable|Polyfill(開発中)|Polyfill(検討中)
Shadow DOM|Stable|Stable|Stable|Polyfill(開発中)|Polyfill(検討中)
HTML Imports|Stable|Stable|Polyfill(保留)|Polyfill(保留)|Polyfill(検討中)
HTML Template|Stable|Stable|Stable|Stable|Stable
ES Module|Stable|Stable|Stable|フラグで有効|Stable

## Polymer

> **Unlock the Power of Web Components.** Polymer is a JavaScript library that helps you create custom reusable HTML elements, and use them to build performant, maintainable apps.

- Googleが開発中のWeb Componentを作る**ライブラリ**
- 実際に作られているCustom Elementはほとんどこれで作られている
- [https://www.webcomponents.org/](https://www.webcomponents.org/)
- Data Bindingなどもサポートしてるのでフレームワークとしても使える→[YouTube](https://youtube.com)

## Web Componentの今までとこれから

- 規格策定の旗振り役はGoogle
- HTML ImportsからES Modulesへ
- Polymer 3.0→Custom ElementsをJavaScriptファイルとして読み込み

## ES Modules

> Belugaチームにはおなじみ`import`と`export`

HTMLで別のJSファイルの中身を読み込みたい

### 昔: `<script>`タグ

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <script
  src="http://code.jquery.com/jquery-3.3.1.js"
  integrity="sha256-2Kok7MbOyxpgUVvAk/HJ2jigOSYS2auK4Pfzbm7uH60="
  crossorigin="anonymous"></script>
</head>
<body>
  hogehoge
</body>
</html>
```

- 生HTMLに気軽に使える
- 読み込んだファイルの中身がすぐに実行される
- HTMLファイルと密結合
- 即時関数を使わないとグローバル汚染
- 出力するもの、読み込むものを指定できない
- 読み込む順番を気にしないと動かない

### 今: CommonJS

Node.jsのmoduleの仕組みをフロントに持ってきた

```js
module.exports = () => {
  console.log('hogehoge');
};
```

```js
const shoutHoge = require('./hogehoge');
```

- 何を出して何を読み込むか指定できる
- ブラウザでnpmを使える (今となってはめちゃくちゃ大事)
- コンパイルが必要 (Webpack/Browserify)
- 同期的な読み込みしかできない

AMD (Asynchronous Module Definition) もあるけどよく知らないしみたこと無い

### 未来: ES Modules

```js
import aws from 'aws-sdk';
```

```js
export const myFunction = () => {
  console.log('hogehoge!');
};
```

- ECMAScript標準
- ブラウザでそのまま使える (IEを除く)
- コンパイルがいらない
- npmをそのまま使える仕組みがまだ無い

Polymerは今年中にこれをサポートする予定

## 他のフレームワークで使う

### Angular

設定で有効にすると公式サポートで使える

```js
import { BrowserModule } from '@angular/platform-browser';
import { NgModule, CUSTOM_ELEMENTS_SCHEMA } from '@angular/core';

import { AppComponent } from './app.component';

import '../web-components/Tabs';
import '../web-components/Tab';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent],
  schemas: [
    CUSTOM_ELEMENTS_SCHEMA
  ]
})
export class AppModule { }
```

### Vue

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Coming soon: vue build my-element.vue —target web-component</p>&mdash; Evan You (@youyuxi) <a href="https://twitter.com/youyuxi/status/958461422786301957?ref_src=twsrc%5Etfw">January 30, 2018</a></blockquote>

（無理やりだが）EntrypointのHTMLに読み込んだらどこでも使える

```html
<link rel="import" href="src/app-hoge.html">
```

```vue
<template>
  <div>
    <h1>Title</h1>
    <app-hoge/>
  </div>
</template>

<script>
export default { ... }
</script>
```

### React

使うことはできる（たぶん）
作ることはできない（たぶん）

## とりあえず何か使ってみる

→[webcomponents.org](https://www.webcomponents.org/)
