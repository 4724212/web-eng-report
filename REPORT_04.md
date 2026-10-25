# 第4回 Webエンジニアリング演習 レポート
## 学籍番号：4724212
## `const`と`let`の宣言・再代入に使用したコード（コードまたは確認結果）

const courseName = "Web Engineering";
let retryCount = 1;

courseName = "Other Course"; // エラー確認後に削除する
retryCount = 2;


[ELIFECYCLE] Command failed with exit code 2.
## VSCodeと`pnpm run check`に表示されたエラー番号・メッセージ（コードまたは確認結果）
@4724212 ➜ /workspaces/web-eng-typescript (main) $ pnpm run check
$ tsc --noEmit
src/index.ts:4:1 - error TS2588: Cannot assign to 'courseName' because it is a constant.

4 courseName = "Other Course"; // エラー確認後に削除する
  ~~~~~~~~~~


Found 1 error in src/index.ts:4
## 修正後の実行結果と`const`・`let`の違いの説明（コードまたは確認結果）
@4724212 ➜ /workspaces/web-eng-typescript (main) $ pnpm run check
$ tsc --noEmit
@4724212 ➜ /workspaces/web-eng-typescript (main) $ pnpm start
$ tsx src/index.ts
courseName: Web Engineering, retryCount: 2

修正後の実行結果は courseName: Web Engineering, retryCount: 2 であった。const は変数への再代入ができないため、courseName の値は変更できない。一方、let は再代入できるため、retryCount は 1 から 2 に変更できる。
