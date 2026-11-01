# 第5回 Webエンジニアリング演習 レポート

## 学籍番号

4724212k

## TODO配列のコード

`src/index.ts` に、4件のTODOをオブジェクトの配列として定義した。すべての要素で同じプロパティ名（`id` / `title` / `completed`）と型を使っている。未完了のTODOは3件含めている。

```ts
const todos = [
  { id: 1, title: "型を復習する", completed: true },
  { id: 2, title: "配列を学ぶ", completed: false },
  { id: 3, title: "レポートを書く", completed: false },
  { id: 4, title: "コミットする", completed: false },
];
```

## TODOを更新した条件

`for...of` で配列を先頭から1件ずつ取り出し、`if` で「未完了（`completed === false`）」のTODOだけを選んで `completed = true` に更新した。ID 2・3・4 の3件が更新対象となる。

```ts
for (const todo of todos) {
  if (todo.completed === false) {
    todo.completed = true;
    console.log(`更新後: ${todo.title} [完了]`);
  }
}
```

更新の前後で完了件数を数えるため、同じ条件の `filter` と `length` を使った。

```ts
const completedBefore = todos.filter((todo) => todo.completed).length;
// （上記の for...of による更新処理）
const completedAfter = todos.filter((todo) => todo.completed).length;
console.log(`完了件数: 更新前 ${completedBefore}件 -> 更新後 ${completedAfter}件`);
```

`filter` は元の配列を変更せず新しい配列を返すため、更新処理をはさんだ前後で同じ式を使って件数を比較できる。

## 完了件数の更新前後の実行結果

`pnpm run check` で型検査を行い、エラーがないことを確認したうえで `pnpm run start` を実行した。

```
$ pnpm run start

更新後: 配列を学ぶ [完了]
更新後: レポートを書く [完了]
更新後: コミットする [完了]
完了件数: 更新前 1件 -> 更新後 4件
```

更新前は完了済みが1件（ID 1）のみだったが、未完了の3件を完了にしたため、更新後は4件（全件）になった。更新によって完了件数が 1件 → 4件 に増えたことを確認できた。
