# LC8T-1st-report 作業ルール

このファイルは Claude Code がセッション開始時に必ず読み込む永続的な指示書です。
このリポジトリで作業する際は、以下のルールに従ってください。

## 確認用URLの提示（必須）

`git push` する際は、必ず以下の形式で確認用URLをセットで提示すること。
ブランチ名ではなく **コミットSHA** を使う（CDNキャッシュ問題を回避するため）。

形式：

```
プッシュ完了（コミット `<short-sha>`）。
確認URL: https://raw.githack.com/kazuyukikonuma/LC8T-1st-report/<full-sha>/index.html
```

例：

```
プッシュ完了（コミット `fab1305`）。
確認URL: https://raw.githack.com/kazuyukikonuma/LC8T-1st-report/fab1305cd2aae6e14f73dc9a233f64bba638ed032/index.html
```

短縮SHAでも raw.githack は動作しますが、念のため full SHA を使用すること。

## ファイル構成

- `index.html` — 本体プレゼン資料（単一HTMLファイル、約2,600行）
- `lc8t-1st-report 260505.html` — 旧版バックアップ
- `README.md` — 資料の概要と前提

## index.html の設計

### キャンバス
- **1280×720 px の固定サイズ**（`#stage` 要素にCSS固定）
- 全 22 スライド、`.slide` クラスで定義
- ナビゲーション：左右矢印キー / 上部ドット / 前後ボタン

### 言語切替（中→日）
- デフォルト：中国語（繁体・zh-TW）= 要素の innerHTML
- 日本語版：各要素の `data-ja="..."` 属性に保持
- 切替ボタン `#btn-lang` で全要素を一括変換
- 言語切替時のチャートテキスト更新は `updateChartsLang(lang)` 関数で対応

### グラフ
- Chart.js v4（CDN 経由）
- PDF 出力時のみアニメOFF（`Chart.defaults.animation = false` を一時設定、終了時に復元）
- 通常閲覧ではアニメON維持（インタラクティブ感を保つ）

### スライド配列
新規スライドを追加・順序変更する場合は **必ず3箇所** を同時に更新：
1. HTML の `<div class="slide">` 本体
2. JavaScript の `titles[]` 配列（中国語タイトル）
3. JavaScript の `titlesJa[]` 配列（日本語タイトル）
4. 必要に応じて `dotColors` オブジェクトに章カラーを追加

## レイアウトの設計制約

- 1280×720 を超えるはみ出しは原則禁止（スライドは `overflow:hidden`）
- **日本語は中国語より文字数が多くなる**ため、日本語版を基準に文字数を制限してレイアウトを設計
- フォント：`Noto Sans JP`（Google Fonts）
- 配色変数は `:root` で一括定義（`--ac-blue`, `--ac-coral`, `--ac-aqua`, `--ac-yellow` など）

## 作業フロー

1. **作業ブランチ**：`claude/...` ブランチで開発
2. **コミット**：日本語の意味のあるコミットメッセージで。1コミット1論理変更。
3. **プッシュ**：`git push -u origin <branch-name>`
4. **確認URL提示**：上記の形式で raw.githack の SHA固定URLを必ずセットで伝える
5. **main マージ**：ユーザの明示的な指示があった場合のみ fast-forward でマージしてプッシュ

## やらないこと

- ユーザの明示的指示なしに main へ直接プッシュしない
- ユーザの明示的指示なしに Pull Request を作らない
- `--no-verify` 等のフック回避フラグは使用しない
- 既存の章カラーや配色変数を勝手に変更しない（一貫性のため）
