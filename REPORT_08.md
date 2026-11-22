# 第8回 Webエンジニアリング演習 レポート

## 学籍番号

4724212

## 氏名

櫨山凱斗

## コンポーネント構成

ViteでReact + TypeScriptのプロジェクトを作り、TODO一覧を次のコンポーネントに分けた。

```
App
└── TodoList
    ├── TodoItem
    ├── TodoItem
    └── TodoItem
```

| ファイル | 役割 |
| --- | --- |
| `src/types.ts` | `Todo` 型（`id`・`title`・`completed`・任意の `dueDate`）を定義する |
| `src/data.ts` | 初期データ（3件）を置く |
| `src/components/TodoItem.tsx` | 1件のTODOを表示する |
| `src/components/TodoList.tsx` | TODO配列を受け取り、`map` で `TodoItem` の並びに変換する |
| `src/App.tsx` | データを持ち、`TodoList` に渡して一覧画面を組み立てる |

```tsx
// src/App.tsx
import { todos } from "./data";
import { TodoList } from "./components/TodoList";

export default function App() {
  return (
    <main>
      <h1>React TODO</h1>
      <TodoList todos={todos} />
    </main>
  );
}
```

## `TodoItem` の Props の型

`TodoItem` は `todo` という1つのPropsを受け取る。型は `types.ts` の `Todo` を使う。

```ts
// src/types.ts
export type Todo = {
  id: number;
  title: string;
  completed: boolean;
  dueDate?: string;
};
```

```tsx
// src/components/TodoItem.tsx
import type { Todo } from "../types";

type TodoItemProps = {
  todo: Todo;
};

function isOverdue(dueDate: string): boolean {
  const today = new Date();
  today.setHours(0, 0, 0, 0);
  return new Date(`${dueDate}T00:00:00`) < today;
}

export function TodoItem({ todo }: TodoItemProps) {
  return (
    <li>
      <strong>{todo.title}</strong>
      <span> {todo.completed ? "(完了)" : "(未完了)"}</span>
      {todo.dueDate && (
        <span style={{ color: isOverdue(todo.dueDate) ? "red" : undefined }}>
          {" "}期限: {todo.dueDate}
        </span>
      )}
    </li>
  );
}
```

- `{todo.dueDate && ...}` の論理積で、`dueDate` があるTODOだけ期限を表示する（条件付きレンダリング）。
- `isOverdue` で期限の日付と今日の日付（0時）を比べ、期限が過ぎていれば `color: red` で赤文字にする。
- 子コンポーネントの `TodoItem` は受け取ったPropsを書き換えず、表示に変換するだけにしている。

## 期限あり・なしの画面確認結果

`src/data.ts` で、3件のうち2件に期限を設定した。

```ts
export const todos: Todo[] = [
  { id: 1, title: "Reactを学ぶ", completed: true },
  { id: 2, title: "Propsを確認する", completed: false, dueDate: "2026-11-01" },
  { id: 3, title: "Stateを更新する", completed: false, dueDate: "2026-12-31" },
];
```

`pnpm run build` で型検査とビルドが成功したことを確認したうえで、`pnpm run dev` で開発サーバを起動し、Codespacesの転送ポート（5173）をブラウザで開いた（確認日: 2026-11-23）。

```
React TODO
・Reactを学ぶ (完了)
・Propsを確認する (未完了) 期限: 2026-11-01   ← 赤文字
・Stateを更新する (未完了) 期限: 2026-12-31   ← 通常の文字色
```

- 期限なし（Reactを学ぶ）: 期限は表示されない。
- 期限あり・期限内（Stateを更新する）: 「期限: 2026-12-31」が通常の色で表示された。
- 期限あり・期限切れ（Propsを確認する）: 「期限: 2026-11-01」が赤文字で表示された。

## コミット

https://github.com/4724212/web-eng-react-practice/commit/728f10076c354259884be6c1f68ed6e978247fe5
