# Tsumiki - AI駆動開発支援フレームワーク

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

🌐 言語: [English](../en/README.md) | **日本語** | [中文](../zh/README.md) | [한국어](../ko/README.md)

Tsumikiは、AI駆動開発のためのフレームワークです。要件定義から実装まで、AIを活用した効率的な開発プロセスを提供します。

## インストール

Tsumikiを使用するには、次のnpxコマンドでインストールしてください：

```bash
npx tsumiki install
```

このコマンドを実行すると、`.claude/commands/` にTsumikiのClaude Codeスラッシュコマンドがインストールされます。

## 概要

Tsumikiは以下の2つのコマンドセットで構成されています：

- **kairo** - 要件定義から実装までの包括的な開発フロー
- **tdd** - テスト駆動開発（TDD）の個別実行

### Kairoコマンド

Kairoは要件定義から実装までの開発プロセスを自動化・支援します：

1. **要件定義** - 概要から詳細な要件定義書を生成
2. **設計** - 技術設計文書を自動生成
3. **タスク分割** - 実装タスクを適切に分割・順序付け
4. **TDD実装** - テスト駆動開発による品質の高い実装

## 利用可能なコマンド

### Kairoコマンド（包括的開発フロー）
- `kairo-requirements` - 要件定義
- `kairo-design` - 設計文書生成
- `kairo-tasks` - タスク分割
- `kairo-implement` - 実装実行

### TDDコマンド（個別実行）
- `tdd-requirements` - TDD要件定義
- `tdd-testcases` - テストケース作成
- `tdd-red` - テスト実装（Red）
- `tdd-green` - 最小実装（Green）
- `tdd-refactor` - リファクタリング
- `tdd-verify-complete` - TDD完了確認

### リバースエンジニアリングコマンド
- `rev-tasks` - 既存コードからタスク一覧を逆生成
- `rev-design` - 既存コードから設計文書を逆生成
- `rev-specs` - 既存コードからテスト仕様書を逆生成
- `rev-requirements` - 既存コードから要件定義書を逆生成

## クイックスタート

### 包括的な開発フロー

```bash
# 1. 要件定義
/kairo-requirements

# 2. 設計
/kairo-design

# 3. タスク分割
/kairo-tasks

# 4. 実装
/kairo-implement
```

### 個別TDDプロセス

```bash
/tdd-requirements
/tdd-testcases
/tdd-red
/tdd-green
/tdd-refactor
/tdd-verify-complete
```

### リバースエンジニアリング

```bash
# 1. 既存コードからタスク構造を分析
/rev-tasks

# 2. 設計文書の逆生成（タスク分析後推奨）
/rev-design

# 3. テスト仕様書の逆生成（設計文書後推奨）
/rev-specs

# 4. 要件定義書の逆生成（全分析完了後推奨）
/rev-requirements
```

### 開発環境のクリーンアップ

```bash
# 開発環境をクリーンアップ
/clear
```

## 詳細なドキュメント

- [マニュアル](./MANUAL.md) - 使用方法の詳細、ディレクトリ構造、ワークフロー例
- [貢献ガイド](./CONTRIBUTING.md) - プロジェクトへの貢献方法
- [Claude.md](./CLAUDE.md) - Claude Codeでの使用ガイド

## ライセンス

MIT License - 詳細は [LICENSE](../../LICENSE) をご覧ください。

## コミュニティ

- [GitHub Issues](https://github.com/classmethod/tsumiki/issues)
- [Discussions](https://github.com/classmethod/tsumiki/discussions)