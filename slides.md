---
marp: true
theme: default
paginate: true
---

# AI と協働して 2 時間で npm パッケージ公開

## create-marp-slides の開発体験

**2025 年 7 月 18 日 AI 発表**

---

## 今回の発表で話すこと/話さないこと

### 話すこと

- AI と協働して npm パッケージを公開するまでの体験
- 大まかな開発の流れと AI の活用方法

### 話さないこと

- 具体的な環境・手順・コマンド
  - まだベタープラクティスを知らないので間違った使い方を広めないため

---

## 背景

- ちょくちょくプレゼンテーションを作成
- プレゼン資料の管理とアップデートが面倒
  - github でまとめて管理したい
  - ついでに公開も github pages で終わらせたい
- **AI を活用した開発効率化への挑戦**
  - Gemini と Claude Code を活用

---

## create-marp-slides

### ワンコマンドで全て解決

```bash
npx create-marp-slides my-presentation
```

- 🚀 **ゼロコンフィグレーション**
- 📦 **GitHub Actions 自動設定**
- 🎨 **内蔵テーマとスタイル**
- 📱 **レスポンシブデザイン対応**
- 🔄 **即座にプレゼンテーション作成可能**

---

## 驚異的な開発速度の実証

### 2025 年 7 月 17 日 - 1 日で完成

- **開発開始から公開まで：2 時間**
- **総コミット数：6 コミット**
- **プロセス：要求定義 → 設計 → 実装 → テスト → 公開**

従来なら数日〜1 週間かかる作業が、**AI との協働で 2 時間**に短縮！

---

## 要求定義 → 設計

- gemini との協働で要求定義と設計を実施（1 時間程度）
- やりたいことはぼんやりと認識
  - git でコンテンツ管理して github pages で公開したい
- スライド作成のユーザ体験部分から gemini と相談
  - 最初は github と連携する Web サービスとして設計していたが、対象ユーザ（つまり自分）のニーズと技術的課題から CLI ツールとして実装することに
- 既存ツールの流用で実現できるか（車輪の再発明になっていないか）を調査してもらい、なさそうなことを確認
  - Marp や SliDev などのスライド作成部分は存在していたのでそれは流用することに
- 最終的なユーザ体験として具体的なスライド作成の流れをコマンド単位で確認
- 開発の大まかな手順までをここで決定

---

## Claude Code での実装

### CLAUDE.md ファイルによる詳細指示

- 先ほど gemini と相談した内容を REDME.md にまとめて`claude /init`コマンドで Claude Code が読み取れるように CLAUDE.md ファイルを作成
- 今回はそのままで良かったので、plan モードで「実装して」と伝える

---

## 開発プロセスの可視化

### コミット履歴から見る協働開発

1. **`Add create-marp-slides CLI tool`** - 基盤実装
2. **`Removed package-lock.json`** - 設定最適化
3. **`Remove automatic dependency installation`** - 機能簡素化
4. **`Fix workflow to handle missing package-lock.json`** - 品質向上
5. **`Correct package.json scripts`** - 最終調整
6. **`Updated template slides`** - ユーザビリティ改善

- 最初の commit から最後のコミットまでの時間は 34 分
- アニメを見ながらたまに Claude のコマンド実行の許可と npm と github のログインを行う
- `gh`コマンドを使っているのでコミット等も自動的に行われる
  - 作業を見てた感じだと、各フェーズでのテストが緩くてのちに修正してるパターンが多かった

---

## 技術的成果

### Node.js CLI ツールの完成

```javascript
import { program } from 'commander';
import { promptForProjectName } from './prompts.js';
import { generateProject } from './generator.js';
import { displaySuccessMessage } from './messages.js';

export async function cli() {
  program
    .name('create-marp-slides')
    .description('CLI tool for scaffolding Marp presentation projects with GitHub Pages deployment')
    .version('1.0.0')
    .argument('[project-name]', 'Name of the project directory to create')
    .parse();
```

- **Commander.js 活用**
  - 既存の便利なライブラリは活用している
- **必要なファイル分割**
  - `prompts.js` でユーザ入力を管理
  - `generator.js` でプロジェクト生成ロジックを実装
  - `messages.js` で成功メッセージを表示

---

## 主要機能デモ

### 実際の使用例

**プロジェクト作成**

```bash
npx create-marp-slides company-presentation
cd company-presentation
npm install
npm run dev
```

**生成されるファイル構成**

```
company-presentation/
├── slides.md              # プレゼンテーション内容
├── package.json           # プロジェクト設定
├── .gitignore             # Git設定
└── .github/workflows/     # 自動デプロイ設定
    └── deploy.yml
```

---

## 自動デプロイ機能

### GitHub Pages への自動公開

```yaml
name: Deploy Marp Slides to GitHub Pages

on:
  push:
    branches: [main]

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "18"

      - name: Install dependencies
        run: npm install

      - name: Build slides
        run: npm run build

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: "./dist"

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

---

## 開発効率の比較

### 従来 vs AI 協働

| 項目         | 従来開発     | AI 協働開発                                      |
| ------------ | ------------ | ------------------------------------------------ |
| 開発期間     | 数日〜1 週間 | **2 時間**                                       |
| 品質         | 標準         | **ジュニアレベルよりは高そう**                   |
| 機能数       | 限定的       | **要件定義次第**                                 |
| アイデア実現 | 遅い         | **即座**                                         |
| 学習コスト   | 高い         | **低い（しかし要件定義のクオリティに影響する）** |

---

## 学びと効果

### AI 協働開発の利点

- ⚡ **開発スピードの劇的向上**
- 💡 **アイデアの即座実現**
- 🎯 **集中すべき箇所の明確化**

---

## 学びと効果

### AI 協働開発の課題

- **自分の知識以上のことを AI に任せることへの不安**
  - やっている作業が本当に正しいのか不安になる
  - 特に今回は npm パッケージとして公開する部分の作業が未知。
- **AI にコマンド実行させることへの抵抗感**
  - 実際に実行してはいけないコマンドを実行しようとした場合があった
    - 気持ちの問題として適当に許可しそうだった
    - Claude の実行自体を仮想環境に閉じ込めるなどの対策が必要（今回はやってない）
      - 内側はそれでコマンドの実行をかなり緩くできるが、外と通信するようなコマンドで悪さをする可能性があるのでコマンド許可のリストは設定が重要

---

## 実際の成果

### npm パッケージとして公開

**リポジトリ：** https://github.com/MasakazuOhtsubo/create-marp-slides

**使用状況：**

- npm パッケージとして公開済み
- GitHub Pages で自動デプロイ動作確認
- 実際のプロジェクトで使用可能

**今この瞬間も使えるツール！**

---

## まとめ

### 2 時間で学んだこと

- 🤖 **AI は開発パートナー**
- 💡 **アイデアの即座実現**
- 🌐 **開発の民主化**
- 🚀 **未来の開発体験**

**AI 協働開発は、もはや実験段階ではなく実用段階**

---

## 質疑応答

### ディスカッション

- 💬 実際の開発体験について
- 🔧 AI 活用のコツとベストプラクティス
- 🔮 今後の開発プロセス展望
- 🤝 チーム開発での活用方法

**ありがとうございました！**

---

## 参考資料

### リンク集

- **create-marp-slides:** https://github.com/MasakazuOhtsubo/create-marp-slides
- **npm パッケージ:** https://www.npmjs.com/package/create-marp-slides
- **Marp:** https://marp.app/
- **Claude AI:** https://claude.ai/code

**今日から始められる AI 協働開発！**
