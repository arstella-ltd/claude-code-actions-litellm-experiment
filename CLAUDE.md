# Claude Code プロジェクトルール

## 対話ルール
- 日本語で会話する
- 簡潔で分かりやすい説明を心がける

## コミットメッセージ形式
- Conventional Commits形式を使用する
- 日本語でコミットメッセージを記載する
- 形式: `<type>: <description>`
  - feat: 新機能
  - fix: バグ修正
  - docs: ドキュメント変更
  - style: コードスタイル変更
  - refactor: リファクタリング
  - test: テスト追加・修正
  - chore: ビルドプロセスや補助ツールの変更

## GitHub Actions
- `.github/workflows/claude-code.yml`にClaude Code用のワークフローを定義
- `@claude`メンションでワークフローをトリガー
- 必要な環境変数:
  - `ANTHROPIC_API_KEY` (secret)
  - `MODEL` (variable)
  - `ANTHROPIC_BASE_URL` (variable)