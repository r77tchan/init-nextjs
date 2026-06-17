---
name: init-nextjs
description: Next.js をセットアップする手順書
disable-model-invocation: true
---

# 前提

- この手順書に記載されていない、想定外の状態を検知した場合、AskUserQuestionツール でユーザーに確認するか、中断してユーザーに報告する。
- 手順書の改善の為、この手順をこなす中で迷った点や、手順書に不備を感じた場合は、ユーザーに指摘する。

## 0. 前提報告

スキルを読み込んだこと、これからNext.jsのセットアップを行うことをユーザーに報告する。

## 1. プロジェクトの現状確認

`ls -A` でディレクトリを確認する。

- **完全に空**の場合 → ステップ2-Aへ進む。
- **それ以外（何らかのファイルやディレクトリが存在）** → ステップ2-Bへ進む。

## 2-A. Next.js の導入

以下のコマンドを実行する。

```sh
npx create-next-app@latest . --yes
```

- 導入に失敗した場合 → 中断してユーザーに報告する。
- インタラクティブに質問された場合（コマンドの対話的な入力要求が発生した場合） → 中断してユーザーに報告する。
- 問題なく完了した場合 → `package.json` の `next` バージョンを表示してユーザーに報告し、ステップ3へ進む。

## 2-B. プロジェクト確認

リポジトリが `create-next-app` 実行直後の状態であることを以下の3点で確認する。

```sh
git rev-list --count HEAD   # 期待値: 1
git log -1 --pretty=%s      # 期待値: Initial commit from Create Next App
git status --porcelain      # 期待値: 空
```

- **3つすべてが期待値と一致**する場合 → ステップ3へ進む。
- **いずれかが一致しない**場合（既存プロジェクト由来、追加コミット済み、未コミット変更ありなど） → 中断し、状況をユーザーへ報告する。

## 3. プロジェクト整理

- 以下のコマンドを実行し、 `public/` の中身が全て削除され、空であることを確認する。

  ```
  rm -f public/file.svg public/globe.svg public/next.svg public/vercel.svg public/window.svg ; ls -A public/
  ```

- `app/page.tsx` を以下の内容に置き換える。

  ```tsx
  export default function Home() {
    return <div></div>;
  }
  ```

- `app/layout.tsx` の `<html lang="en">` を `<html lang="ja">` に変更する。

## 4. prettier-plugin-tailwindcss の導入

以下のコマンドを実行する。

```sh
npm install -D prettier prettier-plugin-tailwindcss
```

完了後、プロジェクトルートに `.prettierrc` を作成し、以下の内容を書き込む。

```json
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

## 5. `package.json` に typecheck script を追加

`package.json` の `scripts` セクションに以下のエントリを追加する。

```json
"typecheck": "tsc --noEmit"
```

追加後、`npm run typecheck` がエラーなく実行できることを確認する。

## 6. コミット

ここまでの変更をコミットする。リモートリポジトリの作成や push は行わない。

```sh
git add -A
git commit -m "init project"
```

## 7. セッション再起動の案内

行った内容を報告し、設定反映の為、ユーザーに **Claude Code セッションの再起動** を促す。
