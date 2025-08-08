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

## トラブルシューティング

### LiteLLM 統合で Issue コメントが空になる問題

**症状**: 
- Claude Code Action が実行されるが、Issue へのコメントが空になる
- ログには `output_tokens` が記録されているのに、実際のコメント内容が反映されない

**原因**:
LiteLLM プロキシのレスポンス形式が Claude Code Action の期待する形式と異なる場合があります。特に以下のケースで問題が発生します：
- 非 Claude モデル（Qwen、GPT など）を使用している場合
- LiteLLM のレスポンス形式が Anthropic API と完全互換でない場合

**診断方法**:
1. GitHub Actions のログで以下を確認：
   - `output_tokens` の値（0でないことを確認）
   - `claude-execution-output.json` の内容
2. デバッグステップが追加されている場合は、raw output を確認

**対処法**:
1. **モデルの確認**: Claude 互換モデルを使用しているか確認
2. **LiteLLM 設定の確認**: 
   ```yaml
   # LiteLLM config.yaml で streaming を無効化
   model_list:
     - model_name: your-model
       litellm_params:
         streaming: false
   ```
3. **API 形式の確認**: LiteLLM が Anthropic API 形式でレスポンスを返すよう設定
4. **デバッグログの確認**: ワークフローにデバッグステップが含まれている場合は、詳細なログを確認

**既知の制限事項**:
- 一部の LLM モデルでは、Claude Code Action のツール使用機能が正しく動作しない場合があります
- ストリーミングレスポンスは現在サポートされていません
