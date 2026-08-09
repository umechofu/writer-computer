# Writer 日本語組版フォーク

これは [joelbqz/writer-computer](https://github.com/joelbqz/writer-computer) をもとに、日本語本文の約物を自然に詰めて表示するための個人用フォークです。

変更は `jp-typography` ブランチにあります。

> [!NOTE]
> このフォークはWriter公式版ではありません。アプリ本体に関する説明やライセンスは、上流リポジトリの [README.md](README.md) と [LICENSE](LICENSE) を参照してください。

## 変更内容

エディター本文にOpenTypeの `palt`（プロポーショナルメトリクス）を適用しています。

```css
.cm-editor .cm-content {
  font-feature-settings: "palt";
}
```

句読点、括弧、鉤括弧などの前後が自然に詰まり、日本語の文章を読みやすい密度で表示できます。

コードフェンスとインラインコードでは `palt` を無効にし、等幅フォントの桁揃えを維持します。

```css
.cm-editor .cm-fenced-code-line,
.cm-editor .cm-inline-code {
  font-feature-settings: normal;
}
```

## 対象環境

- macOS
- Node.js 22.12.0以上
- Rust
- Xcode Command Line Tools
- Vite+（`vp`）

## ビルド

```bash
git clone --branch jp-typography https://github.com/umechofu/writer-computer.git
cd writer-computer

npm install -g vite-plus
vp install
```

開発モードで確認します。

```bash
vp run desktop#dev
```

確認用の文章：

```text
こんにちは、世界。（テストです）「約物の詰まり」を見る──ここ。
```

本番用の `Writer.app` を作ります。公式アップデーター用の秘密鍵は持っていないため、個人用ビルドではアップデーター成果物の作成を無効にします。

```bash
cd apps/desktop

vp run tauri build \
  --bundles app \
  --config '{"bundle":{"createUpdaterArtifacts":false}}'
```

完成したアプリ：

```text
apps/desktop/src-tauri/target/release/bundle/macos/Writer.app
```

起動：

```bash
open "$HOME/Developer/writer-computer/apps/desktop/src-tauri/target/release/bundle/macos/Writer.app"
```

## 上流への追従

上流の既定ブランチは `master` です。

```bash
git remote add upstream https://github.com/joelbqz/writer-computer.git
git fetch upstream
git checkout jp-typography
git rebase upstream/master
git push --force-with-lease origin jp-typography
```

競合が発生した場合は、上流の新しいCSSを残しつつ、次の2つを適用し直します。

- 本文の `font-feature-settings: "palt";`
- コード表示の `font-feature-settings: normal;`

## ライセンス

上流と同じくGPL-3.0です。詳しくは [LICENSE](LICENSE) を参照してください。
