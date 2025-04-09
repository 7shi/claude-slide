# スライド作成プロンプト

Claude で YouTube 動画の字幕情報からスライドを作成するためのプロンプトです。スマートフォンでの閲覧も想定して、レスポンシブデザインを採用しています。

※ スライドは動画の要約を目的としており、内容をすべて含むわけではありません。また、内容の正確さも保証できません。詳細な内容を知りたい場合、動画を視聴してください。

## サンプル

サンプルとして、このプロンプトで生成したスライドをいくつか示します。

- [ループ量子重力論争：カルロ・ロヴェリの反論](https://codepen.io/7shi/embed/LEYooBK?default-tab=result)
- [存在するはずのない重力理論](https://codepen.io/7shi/embed/QwWRPvL?default-tab=result)
- [日本語は本当に難しくないのか？- 8年の学習経験から](https://codepen.io/7shi/embed/EaxzzGr?default-tab=result)
- [中国語は英語に代わる共通語になるか？](https://codepen.io/7shi/embed/XJWwLbQ?default-tab=result)
- [言語の標準化とは](https://codepen.io/7shi/embed/ByaegbO?default-tab=result)
- [現代ローマ人はラテン語を理解できるか？](https://codepen.io/7shi/embed/xbxNNeN?default-tab=result)
- [イタリア人はメキシコのスペイン語を理解できるか？](https://codepen.io/7shi/embed/YPzboQN?default-tab=result)
- [エスペラント理解度テスト: イタリア語話者の視点から](https://codepen.io/7shi/embed/zxYQgoo?default-tab=result)
- [インターリングア VS ネオラテン ロマンス言語を統一するのはどちら？](https://codepen.io/7shi/embed/ZYEdYQb?default-tab=result)

※ これらは CodePen で動作しています。詳細は後述の「CodePen」を参照してください。

## 準備

Claude デスクトップアプリで [mcp-youtube](https://github.com/anaisbetts/mcp-youtube) を有効にします。

## 設定

Claude でプロジェクトを作成します。

プロジェクトナレッジ欄で指示の「編集」をクリックして、以下の内容を貼り付けます。

- [prompt.md](prompt.md)

## 使い方

プロジェクトの新規チャットで URL を入力すれば、作業が始まります。

ステップごとに確認を求められるので、問題なければ `ok` と入力することで次に進みます。

1. 字幕データのダウンロード
2. 内容の分析・スライド構造の設計
3. Markdown 形式で下書き
4. React コードの生成（スライド 5 枚ずつ）

※ たまに飛ばして先に進むことがあります。

## トラブルシューティング

よくある問題とその対処法を示します。

- 「クロードがメッセージの最大文字数に達したため、応答を一時停止しています。「続ける」と入力してチャットを継続できます。」
  - 生成するコードが長すぎると中断されます。「続ける」と入力すれば、続きから生成します。
  - 末尾ではない箇所から生成が再開されることがあります。その場合、React コードの生成から「再試行」してください。

- 「アーティファクトの実行エラー 生成された成果物の実行中にエラーが発生しました。 `Unexpected token (xx:xx)`」
  - 直接入力できない文字が原因になっていることが多いです。「テキストエレメントの不等号とアンパサンドが文字実体参照になっているかを確認してください」と指示してみてください。

## デバッグ

トラブルシューティングで直らない場合、修正を Claude に任せても解決することは少ないため、React コードをローカルでデバッグする必要があります。

このリポジトリの以下のファイルにアーティファクトで生成された React コードを貼り付けてください。

- [src/App.js](src/App.js)

`npm start` としてデバッグ用のサーバーを起動すれば、自動的にブラウザが開くはずです。開かない場合は手動で以下の URL を開いてください。

- http://localhost:3000

エラーの行数などが表示されているはずなので、その箇所を確認してください。

## CodePen

ローカルに React の環境構築ができない場合、[CodePen](https://codepen.io/) を使用するのが手軽です。

1. 新規に Pen を作成
2. HTML に以下の内容を記述
```html
<div id="root"></div>
```
3. JS タブの [⚙] をクリック
4. JavaScript Preprocessor に Babel を選択
5. [Save & Close] をクリック
6. JS (Babel) の欄に React コードを貼り付け
7. 先頭の `import` 文を以下に置き換え（存在しない場合は追加）
```jsx
import React from "https://esm.sh/react@19";
import { createRoot } from "https://esm.sh/react-dom@19/client";
```
8. 末尾の `export` 文を以下に置き換え
```jsx
createRoot(root).render(<Presentation />);
```

公開リンクは、右下の Embed から Iframe の `src` 属性の値をコピーしてください。

## 参考

CodePen での React 19 の使い方は以下の Pen を参照しました。

- https://codepen.io/chriscoyier/pen/RNboKBL

このプロンプトは [Genspark](https://www.genspark.ai/) のスーパーエージェントに触発されて作成しました。そちらの方がより高性能なため、比較してみると良いでしょう。

- https://x.com/tetumemo/status/1907519767394787804

[Maki](https://github.com/Sunwood-ai-labs) さんのグラフィックレコーディング風プロンプトは、非常に完成度が高いです。

- https://x.com/hAru_mAki_ch/status/1896533569968984546
- https://x.com/hAru_mAki_ch/status/1902366494237016271

アーティファクトでのレスポンシブデザインの使用にはハマり所があります。

- [Claude のアーティファクトで Tailwind CSS のレスポンシブデザインにハマった - Qiita](https://qiita.com/7shi/items/a056f343ea4ee03285d9)
