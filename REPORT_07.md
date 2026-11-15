# 第7回 Webエンジニアリング演習 レポート

## 学籍番号

4724212k

## 2画面の見出しと本文

`csr/index.html` に「比較」「詳細」の2つのリンクを置き、行き先をそれぞれ `#compare` と `#detail` にした。最初のHTMLには見出しも本文も書かず、`#app` を空のまま残している。

```html
<nav>
  <a href="#compare">比較</a>
  <a href="#detail">詳細</a>
</nav>
<main id="app"></main>
<script type="module" src="./app.js"></script>
```

`csr/app.ts` で、ハッシュごとに表示する見出しと本文を決めた。

| ハッシュ | 見出し | 本文 |
| --- | --- | --- |
| `#compare`（初期表示・それ以外も同じ） | 比較 | MPAとSPAを比較する画面。 |
| `#detail` | 詳細 | CSRとSSRの違いを説明する画面。 |

```ts
const app = document.querySelector("#app");

if (app === null) {
  throw new Error("#appが見つからない");
}

function render(): void {
  if (app === null) return;
  switch (location.hash) {
    case "#detail":
      app.innerHTML = `<h1>詳細</h1><p>CSRとSSRの違いを説明する画面。</p>`;
      break;
    case "#compare":
    default:
      app.innerHTML = `<h1>比較</h1><p>MPAとSPAを比較する画面。</p>`;
  }
}

window.addEventListener("hashchange", render);
render();
```

## クリック後のURLとNetworkの確認

`pnpm run check` で型検査を行い、エラーがないことを確認したうえで `pnpm run build` で `app.js` を生成した。続けて `pnpm run start` で開発サーバを起動し、`http://localhost:3000/csr/` をChromeで開いた。DevToolsのNetworkタブを開いてログを消してから、「詳細」「比較」の順にリンクをクリックした。

```
$ pnpm run check
$ pnpm run build
$ pnpm run start
```

- 「詳細」をクリックした後のアドレスバー: `http://localhost:3000/csr/#detail`
- 「比較」をクリックした後のアドレスバー: `http://localhost:3000/csr/#compare`
- Networkタブ: リンクをクリックしても、新しいHTML（`index.html`）のリクエストは追加されなかった。

アドレスバーの末尾に `#detail` と `#compare` が表示され、見出しと本文だけが切り替わった。画面を切り替えても新しいHTMLを取得していないことを確認できた。

## 画面を切り替える処理の要点

URLの `#` 以降は同じページの中の位置を表すため、ハッシュだけが変わってもサーバへ新しいHTMLは要求されない。この性質を使って、同じHTMLの中で画面名をURLに残している。

```ts
window.addEventListener("hashchange", render);
render();
```

`href="#detail"` のリンクを押すと `location.hash` が `#detail` に変わる。`hashchange` イベントがこの変化をきっかけに `render` を実行する。`render` は `location.hash` の値を `switch` で判定し、`#app` の `innerHTML` だけを書き換える。ページ全体は作り直さない。

`hashchange` はページを最初に開いたときには発生しない。そこで最後に `render()` を直接呼び、最初の表示でも `location.hash` から画面を決めるようにした。`#compare` 以外や空のハッシュは `default` で比較画面になる。

`document.querySelector` は要素が見つからないと `null` を返す。`app` の `null` を先に除くことで、TypeScriptの型エラーを避けている。

最初のHTMLが空で、ブラウザ上のJavaScriptが画面を組み立てる点で、これはSPAとCSRの組み合わせの小さな例になっている。

