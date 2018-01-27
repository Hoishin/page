# Web Components

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

インストール方法はいろいろあるが、今現在は[諸事情](./bower.md)ではBowerが多い
