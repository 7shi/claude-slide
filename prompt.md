YouTube動画トランスクリプトを分析し、魅力的なReactスライドプレゼンテーションに変換してください。

# 基本指示

1. MCP経由でトランスクリプトを取得して、いったんここで処理を中断し、ユーザーに確認を求めてください
2. ユーザーの指示で続行
3. トランスクリプトを日本語で詳細に分析し、主要なセクション、キーポイント、比較要素を特定してください
4. 内容に応じた最適なスライド構造を設計して、いったんここで処理を中断し、ユーザーに確認を求めてください
5. ユーザーの指示で続行
6. アーティファクトにスライドの内容をMarkdownの下書きとして作成し、いったんここで処理を中断し、ユーザーに確認を求めてください
7. ユーザーの指示で続行
8. プレビュー可能なアーティファクトに、スライド1～5までのReactコードを出力して、いったんここで処理を中断し、ユーザーに確認を求めてください（冒頭部分でまず動作確認）
9. ユーザーの指示で続行
10. スライドを5枚追加して、いったんここで処理を中断し、ユーザーに確認を求めてください（最初から書き直さないこと、削除はしないこと）
11. ユーザーの指示で続行
12. スライドに残りがあれば10.に戻り、最後まで出力できればここで完了
13. 最大文字数に達して中断された場合、「続ける」の指示で、アーティファクトの末尾で書きかけになっている箇所から書き始めてください（最初から書き直さないこと）
14. Reactコード生成後にユーザーからエラーの指摘があった場合、まずテキストエレメントの不等号`<`,`>`とアンパサンド`&`が文字実体参照になっているかを確認してください

# 分析と構造化

1. まず、トランスクリプトの内容を分析して以下を特定してください：
   - 主要なトピックとセクション（タイムスタンプがあれば活用）
   - 比較対象がある場合はそれらの要素
   - 重要な定義、例示、引用
   - 数値データや統計情報（表やグラフ化できるもの）

2. 以下のような構造を検討してください（内容に応じて調整）：
   - タイトルスライド（動画タイトル、主題を反映、リンク付きで出典情報を明示）
   - イントロダクション（主要概念の定義や説明）
   - コンテンツセクション（トピックごとに分割）
   - 比較セクション（対比要素がある場合）
   - まとめや結論
   - 参考リソース（動画内で言及されたもの）

# Markdown下書きについて

1. 各スライドの区切りを明確にし、スライド番号を付けてください
2. 見出しレベルを使って階層構造を表現してください
3. リスト、表、引用などのMarkdown記法を活用してください
4. 2段組レイアウトを活用して、左右に配置する内容を明示してください
5. 関連する話題や対比などは、2段落レイアウトで表現できないか検討してください
6. 重要なポイントや視覚的強調要素を注記してください
7. 各スライドの最後には、可能な限り出典となる発言をトランスクリプトから引用してください
8. 最終スライドは参考資料としてください

# Reactコンポーネント設計

1. インデント最小化：
   コードのインデントは最小限に抑え、可能な限り改行のみで構造を表現してください。ネストが深くなる場合でもインデントを増やさないでください。この対応はトークン数の節約が目的です。

2. 以下のようなコンポーネント構造を使用してください：（`Presentation()`における`<link>`は必須、省略不可）

```jsx
function Quote({ original, translation, author }) {
return (
<div className="quote-container my-4 pl-4 border-l-4 border-gray-400">
<blockquote className="text-gray-700 italic">"{original}"</blockquote>
<p className="text-gray-600 mt-1">{translation}</p>
{author && <p className="text-right text-gray-500 text-sm mt-2">— {author}</p>}
</div>
);
}

function Slide({ title, bgColor = "bg-gray-50", children }) {
return (
<div className={`slide-container ${bgColor} w-full max-w-screen-lg mx-auto m-4 rounded-lg shadow`}>
<div className="container mx-auto px-4 py-6">
<h2 className="slide-title text-3xl font-bold mb-6">{title}</h2>
{children}
</div>
</div>
);
}

function TwoColumn({ leftTitle, rightTitle, leftContent, rightContent }) {
return (
<div className="grid grid-cols-1 md:grid-cols-2 gap-6">
<div>
{leftTitle && <h3 className="text-xl font-semibold mb-3">{leftTitle}</h3>}
<div>{leftContent}</div>
</div>
<div>
{rightTitle && <h3 className="text-xl font-semibold mb-3">{rightTitle}</h3>}
<div>{rightContent}</div>
</div>
</div>
);
}

function Card({ title, icon, children, accentColor = "border-indigo-500" }) {
return (
<div className={`card rounded-lg shadow-md p-5 bg-white mb-6 border-l-4 ${accentColor}`}>
{title && <h3 className="card-title text-xl font-semibold mb-3">{icon} {title}</h3>}
<div className="card-content">{children}</div>
</div>
);
}

function IconList({ items }) {
return (
<ul className="space-y-2 my-4">
{items.map((item, index) => (
<li key={index} className="flex items-start">
<span className="mr-2 text-indigo-500">{item.icon}</span>
<span>{item.text}</span>
</li>
))}
</ul>
);
}

// その他必要なコンポーネント...

// メインアプリケーション
function Presentation({ slides }) {
return (
<>
<link href="https://cdnjs.cloudflare.com/ajax/libs/tailwindcss/2.2.19/tailwind.min.css" rel="stylesheet" />
<div className="presentation-container">
<Slide title="タイトル" bgColor="bg-indigo-100">
  {/* スライド内容 */}
</Slide>
{/* 他のスライド */}
</div>
</>
);
}

export default Presentation;
```

3. 繰り返し要素はインラインで記述して、文字列に引用符（特にダブルクォーテーション）が含まれる場合、必ずエスケープしてください：
```jsx
// 使用例
<IconList items={[
{icon: "🔍", text: "I say, \"What is this?\"."},
{icon: "🌐", text: "You say, \"This is a pen\"."},
]} />
   ```

4. `<li>` の先頭には "•" や "- " は付けないでください：
```jsx
// 誤った例
<ul>
<li>• foo</li>
<li>- bar</li>
</ul>

// 正しい例
<ul>
<li>foo</li>
<li>bar</li>
</ul>
```

5. テキストエレメントの不等号`<`,`>`とアンパサンド`&`は文字実体参照を使用してください。
```jsx
// 誤った例
<p>条件: a < 1 && b > 2</p>

// 正しい例
<p>条件: a &lt; 1 &amp;&amp; b &gt; 2</p>
```

# デザイン要素

Tailwind CSSを使用し、視覚的に魅力的なデザインを実現してください。

1. スライドレイアウト：
   - 各スライドは横長のコンテナコンポーネントで表現
   - スライドナビゲーションは不要
   - 各スライドは完全に可変長とし、高さを固定値にしない（`height: 100%`や`vh`を使わない）
   - 内容に合わせて自然な高さになるようにする
   - 横幅はビューポートの幅に合わせる
   - 内容を16:9のアスペクト比に収まるよう適切に分割する（ただし内容が少ない場合は無理に埋める必要はない）
   - placeholderによるパディングは、レスポンシブデザインの障害になるため使用しない
   - 各スライドには明確な背景色でのスタイリングで視覚的区分
   - 各スライドの最後にある引用は`TwoColumn`の外に配置することで、1段組とする

2. 2段組レイアウト：
   - 専用の`TwoColumn`コンポーネントを使用
   - デスクトップ表示時：各スライド内部をグリッドで2カラムに分割
   - 左右の列の幅比率は内容に応じて調整（props経由で指定可能）
   - モバイル表示時：レスポンシブに1段組に自動切り替え
   - 見出し部分のみ全幅（コンテナの外に配置）
   - 情報量が多い`Card`は2段組の外に出すなど、柔軟にレイアウトする

3. トピック区分の視覚表現：
   - 専用の`Card`コンポーネントを使用
   - 関連するトピックには同系色のアクセントカラーをpropsで指定
   - accentColor, bgColor等のpropsでコンポーネントごとに視覚的な区別を付ける
   - 重要度に応じた階層を視覚的に表現（サイズ、色、ボーダーなど）

4. 視覚的階層：
   - 見出しは大きく目立つデザイン（太字、大きいフォント、アクセントカラー）
   - サブトピックは段階的に小さくなるフォントサイズとウェイト
   - 重要ポイントは強調コンポーネントを使用
   - 補足情報は視覚的に区別（特定のスタイルクラスを適用）

5. 色彩とコントラスト：
   - トピックに合った色のテーマ設定（メインカラー変数で管理）
   - 対比されるコンセプトには明確に異なる色調を使用
   - 背景と文字のコントラスト比は4.5:1以上を確保
   - 色だけでなく形や記号でも情報を区別（色覚多様性に配慮）

6. 視覚的要素：
   - 各ポイントのiconプロパティでUnicode絵文字を指定
   - 比較表は専用の`ComparisonTable`コンポーネントを使用
   - コードブロックは専用の`CodeBlock`コンポーネントで実装
   - 重要なコンセプトは`Highlight`コンポーネントで強調

# Tailwind CSSクラス例

- スライドコンテナ: `w-full py-8`
- グリッド: `grid grid-cols-1 md:grid-cols-2 gap-6`
- カード: `rounded-lg shadow-md p-6 bg-white dark:bg-gray-800`
- 見出し: `text-3xl font-bold text-indigo-600 mb-4`
- 段落: `text-gray-700 dark:text-gray-300 leading-relaxed`
- ハイライト: `bg-yellow-100 dark:bg-yellow-900 px-1 rounded`
- アクセント: `border-l-4 border-indigo-500 pl-4`
- リスト: `space-y-2 my-4`

# 追加検討事項

1. トランスクリプトから以下の要素を特定し強調することを検討してください：
   - 定義されている専門用語
   - 明確な対比や比較
   - 数値データや統計
   - 具体的な例示
   - 時系列的な発展や変化
   - 問題と解決策のパターン

2. 最適な視覚化方法：
   - 表：複数項目の比較に (`ComparisonTable`コンポーネント)
   - 引用ブロック：重要な発言に (`Quote`コンポーネント) - 原語を使用し、その下に日本語訳を添える
   - コードブロック：専門的な例や表記に (`CodeBlock`コンポーネント)
   - アイコン付きリスト：特徴や利点の列挙に (`IconList`, `IconListItem`コンポーネント)
   - 色分け：対比される概念の区別に (accentColorプロパティ)
   - グリッドレイアウト：関連情報のグループ化に (`Grid`コンポーネント)
   - 図・グラフ：視覚的な理解のために (必要に応じて`Chart`コンポーネントを追加)
   - 数式：数学的な内容を扱う場合に (`Formula`コンポーネント - KaTeXを使用)

内容の正確さと視覚的な魅力のバランスを取りながら、視聴者が理解しやすいReactスライドを作成してください。
