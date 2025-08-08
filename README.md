# claude-code-actions-litellm-experiment
⚡ Claude Code Actions を LiteLLM プロキシで検証するための実験的レポジトリ - API ルーティング、パフォーマンス、互換性のテスト

## 概要
このレポジトリは Claude Code Actions の動作を検証するためのテスト環境です。

## Claude Code Actions ワークフローについて
`.github/workflows/claude-code.yml` では以下の GitHub Actions ワークフローを定義しています：

### トリガー条件
- **Issue コメント**: `@claude` を含むコメントが作成された時
- **プルリクエストレビューコメント**: `@claude` を含むレビューコメントが作成された時  
- **Issue**: `@claude` を含む Issue が作成・割り当てされた時
- **プルリクエストレビュー**: `@claude` を含むレビューが提出された時

### 主な機能
1. **自動応答**: `@claude` メンションに対して Claude が自動的に応答
2. **コード編集**: `Edit`, `Replace`, `View` ツールを使用してコードの編集が可能
3. **GitHub 操作**: Issue や Pull Request の作成が可能
4. **セキュリティ**: 必要最小限の権限（contents, pull-requests, issues, id-token への write アクセス）

### 設定
- **API キー**: `ANTHROPIC_API_KEY` を GitHub Secrets で設定
- **モデル**: `MODEL` 変数で Claude モデルを指定
- **ベース URL**: `ANTHROPIC_BASE_URL` で API エンドポイントを設定（LiteLLM プロキシ用）
- **高速モデル**: `ANTHROPIC_SMALL_FAST_MODEL` で軽量タスク用のモデルを設定
