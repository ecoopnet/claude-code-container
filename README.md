# Claude Code Container

Dockerコンテナ内でClaude Codeを実行するためのツールです。

## セットアップ

1. **Container System を開始**
   ```bash
   container system start
   ```

2. **Dockerイメージをビルド**
   ```bash
   ./build.sh
   ```

## 使用方法

```bash
claude-container [workspace] [-- claude-options...]
```

### 基本的な使用例

```bash
# カレントディレクトリを使用
claude-container

# カレントディレクトリを明示的に指定
claude-container .

# 特定のディレクトリを指定
claude-container /path/to/project
```

### Claudeオプション付きの使用例

```bash
# カレントディレクトリ + Claudeオプション
claude-container -- --model claude-3-opus-20240229

# カレントディレクトリを明示 + Claudeオプション
claude-container . -- --verbose

# 特定のディレクトリ + Claudeオプション
claude-container /path/to/project -- --model claude-3-opus

# Claudeのヘルプを表示
claude-container --help
```

## 機能

- **自動設定**: ローカルのClaude設定（`.claude`ディレクトリ）を自動マウント
- **タイムゾーン検出**: ローカル環境のタイムゾーンを自動検出（Windows/macOS/Linux対応）
- **シンボリックリンク対応**: スクリプトをどこに配置・リンクしても動作
- **設定永続化**: コンテナ内での設定変更がローカルに保存

## 参考

セットアップの詳細については以下を参照してください：
https://zenn.dev/schroneko/articles/claude-code-on-apple-container
