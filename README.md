# gh-scaffold

**Your workflow, any repository.**

gh-scaffold is a GitHub CLI extension that provides a consistent workflow for creating Issues and Pull Requests across repositories without requiring repository-specific templates.

リポジトリやOrganizationに依存せず、一貫したIssue・Pull Requestの作成体験を提供するGitHub CLI Extension。

## Overview

GitHubでは、IssueやPull Requestのテンプレートをリポジトリ単位で管理できる。また、Organizationや個人アカウントの `.github` リポジトリを利用すれば、共通テンプレートを設定することも可能である。

しかし、これらの仕組みは基本的にリポジトリの所有者が管理するものであり、複数のOrganizationや他者のリポジトリを横断して作業する開発者が、自身の開発ワークフローを一貫して利用することには適していない。

gh-scaffoldは、Issue・Pull RequestのテンプレートをCLI側に保持することで、この課題を解決する。

### Goals

- リポジトリごとにテンプレートを配置する手間をなくす
- Issue・Pull Requestの記述構造を標準化する
- 複数リポジトリ間で一貫した開発ワークフローを提供する
- GitHub CLIの既存機能を活用し、シンプルな操作体系を維持する
- 将来的なユーザー定義テンプレートやAI Agentとの連携を可能にする

### Non-Goals

- GitHub IssuesやPull Requestsの管理機能そのものを置き換えること
- GitHub Projectsの代替となること
- 独自のGitHub認証機構を実装すること
- 特定のプログラミング言語やフレームワークに依存した開発規則を強制すること
- MVP段階でAIによる本文生成を提供すること

## Core Concepts

### Portable Templates

リポジトリの設定に依存せず、CLIに内蔵されたテンプレートを使用できる。

### Zero Configuration

追加設定なしで、インストール直後から標準テンプレートを利用できる。

### Repository-Aware

リポジトリに既存のテンプレートが存在する場合は、それを優先的に利用できる。

ただし、ユーザーが明示的に指定した場合は、リポジトリのテンプレートより内蔵テンプレートを優先する。

### Consistent Workflow

IssueやPull Requestを作成する際の入力・編集・確認・投稿の流れを統一する。

### Agent-Friendly

将来的にAI Agentからも利用できるよう、対話UIと内部処理を分離できる設計を目指す。

## Features

### Issue Creation

内蔵テンプレートまたはリポジトリのテンプレートを使用してIssueを作成する。

```
gh scaffold issue
```

特定のIssue種別を指定することも可能。

```
gh scaffold issue --type refactor
```

### Pull Request Creation

テンプレートを利用してPull Requestの本文を作成する。

```
gh scaffold pr
```

PRの作成自体はGitHub CLIの既存機能を利用する。

### Template Source Selection

テンプレートの取得元を明示できる。

```
# 内蔵テンプレートを使用
gh scaffold issue --source builtin --type feature

# リポジトリのテンプレートを使用
gh scaffold issue --source repository --type bug
```

`--source` を指定しない場合は、対応するリポジトリテンプレートを優先し、利用できない場合に内蔵テンプレートへフォールバックする。

### Interactive Editing

テンプレートを選択した後、Issue・PRのタイトルおよび本文を編集できる。

本文の編集には、ユーザーが設定したエディタを利用する。

### Dry Run

Issue・PRを実際に作成することなく、生成した内容を確認できる。

```
gh scaffold issue --type bug --dry-run
```

## Default Templates

gh-scaffoldは、開発作業で一般的に利用されるテンプレートを標準で提供する。

### Issue Templates

| Type           | Description       |
| -------------- | ----------------- |
| Bug            | 不具合の報告と再現条件       |
| Feature        | 新機能・既存機能の改善       |
| Task           | 一般的な作業            |
| Epic           | 複数Issueの統括        |
| Refactor       | 外部動作を維持した内部構造の改善  |
| Dependencies   | パッケージ・ライブラリの更新    |
| Documentation  | ドキュメントの追加・修正      |
| Maintenance    | CI/CDや開発環境などの保守   |
| Test           | テスト追加・テスト品質の改善    |
| Performance    | 性能改善              |
| Security       | セキュリティ強化・予防的対策    |
| Technical Debt | 技術的負債の管理          |
| Investigation  | 技術調査・PoC・実現可能性の検証 |

これらのテンプレートは、特定の技術スタックに依存しない構造とする。

なお、未公開の脆弱性に関する報告は通常のSecurity Issueとして公開せず、GitHubのPrivate Vulnerability Reportingなどの適切な報告経路を利用する。

### Pull Request Templates

MVPでは汎用的なPull Requestテンプレートを1種類提供する。

主な記載項目は以下とする。

- Summary：変更の概要
- Related Issues：関連Issue
- Changes：主要な変更内容
- Verification：検証方法と結果
- Impact / Risks：影響範囲とリスク
- Checklist：最終確認項目

## Template Resolution

MVPでは、以下の優先順位でテンプレートを解決する。

1. CLI引数による明示的なテンプレートソース指定
2. リポジトリ内の対応するMarkdownテンプレート
3. gh-scaffoldに内蔵されたデフォルトテンプレート

リポジトリ内に複数の候補がある場合は、対話的に選択できるようにする。

MVPではMarkdown形式のテンプレートを対象とし、GitHub Issue FormsのYAML形式には対応しない。

## MVP Scope

最初のリリース（v0.1.0）では、以下の機能の実装を目標とする。

### Functional Requirements

- GitHub CLI Extensionとしてインストール・実行できる
- Goで実装された単一の実行ファイルとして配布できる
- Issueテンプレートをバイナリに内蔵できる
- Pull Requestテンプレートをバイナリに内蔵できる
- Issueの種類を選択できる
- リポジトリの既存Markdownテンプレートを検出できる
- テンプレートの取得元を明示的に指定できる
- リポジトリのテンプレートがない場合、内蔵テンプレートにフォールバックする
- Issue・PRのタイトルと本文を編集できる
- GitHub CLIを利用してIssueを作成できる
- GitHub CLIを利用してPull Requestを作成できる
- Dry Runで投稿内容を確認できる
- 投稿を取り消した場合、Issue・PRが作成されない

### Acceptance Criteria

**AC-01: Zero Configuration**

テンプレートが存在しないリポジトリにおいて、追加設定なしで内蔵テンプレートからIssueを作成できる。

**AC-02: Portability**

異なるOrganizationまたはリポジトリ間でも、同一の内蔵テンプレートを使用できる。

実際のIssue・PR作成には対象リポジトリへの適切な権限が必要となる。

**AC-03: Repository Awareness**

リポジトリに対応するMarkdownテンプレートが存在する場合、そのテンプレートを利用できる。

**AC-04: Explicit Override**

リポジトリのテンプレートが存在しても、明示的なCLI引数によって内蔵テンプレートを利用できる。

**AC-05: Safe Preview**

Dry Runでは、GitHub上にIssue・Pull Requestが作成されない。

**AC-06: Editable Content**

生成された本文をユーザーが確認・編集した後に投稿できる。

**AC-07: Standalone Operation**

独自のバックエンド、外部LLM、APIキーを必要とせず、認証済みGitHub CLI環境で利用できる。

## Technology Stack

| Component            | Technology           |
| -------------------- | -------------------- |
| Programming Language | Go                   |
| CLI Framework        | Cobra                |
| Interactive CLI      | huh                  |
| GitHub Integration   | go-gh / gh CLI       |
| Template Storage     | Go embed             |
| Template Format      | Markdown             |
| Editor Integration   | VISUAL / EDITOR      |
| Testing              | Go testing           |
| CI/CD                | GitHub Actions       |
| Distribution         | GitHub CLI Extension |

### Architecture

```
gh-scaffold/
├── main.go
├── cmd/
│   ├── root.go
│   ├── issue.go
│   └── pr.go
├── internal/
│   ├── template/
│   ├── editor/
│   └── github/
├── templates/
│   ├── issues/
│   │   ├── bug.md
│   │   ├── feature.md
│   │   ├── task.md
│   │   ├── epic.md
│   │   ├── refactor.md
│   │   ├── dependencies.md
│   │   ├── documentation.md
│   │   ├── maintenance.md
│   │   ├── test.md
│   │   ├── performance.md
│   │   ├── security.md
│   │   ├── technical-debt.md
│   │   └── investigation.md
│   └── pull_requests/
│       └── default.md
├── tests/
├── go.mod
└── README.md
```

### Design Principles

- CLI引数の解析、テンプレート解決、本文編集、GitHub操作の責務を分離する
- 認証やGitHub APIとの通信については既存の公式機能を優先する
- 内蔵テンプレートはGoの `embed` を使ってバイナリに組み込む
- テンプレートは情報の構造を標準化し、具体的な実装方法を不必要に強制しない
- テンプレートの生成処理は、将来の非対話モードでも再利用できるようにする
- MVPでは外部サービスへの追加依存を避ける

## Roadmap

### v0.1.0: Portable Defaults

MVP。

- 内蔵Issue・PRテンプレート
- テンプレート解決
- CLIによる種類・取得元の選択
- 対話形式での編集
- Issue・PRの作成
- Dry Run
- GitHub CLI Extensionとしての配布

### v0.2.0: Personal Workflow

個人の開発規則をより柔軟に管理できるようにする。

- XDG Base Directoryに準拠した設定管理
- ユーザー独自テンプレートの登録
- テンプレートのカスタマイズ
- テンプレートの取得元と優先順位の設定
- テンプレート変数のサポート

### v0.3.0: Agent-Friendly Workflow

AI Agentや自動化ツールからの利用を改善する。

- 非対話モード
- 標準出力への本文出力
- JSON形式での構造化出力
- Agent Skillとの連携
- AI Agentによる本文生成ワークフローへの対応

なお、AIモデル自体をgh-scaffoldに内蔵することは必須としない。

## Related Projects

### GitHub CLI

[https://cli.github.com/](https://cli.github.com/)

GitHubが提供する公式CLI。Issue・Pull Requestの作成など、GitHubとの基本的な操作を担う。

### gh-prx

[https://github.com/ilaif/gh-prx](https://github.com/ilaif/gh-prx)

Pull Requestの作成ワークフローを支援するGitHub CLI Extension。PRテンプレートのフォールバックなど、類似した機能を提供する。

### gh-issue-bulk-create

[https://github.com/ntsk/gh-issue-bulk-create](https://github.com/ntsk/gh-issue-bulk-create)

テンプレートとCSVを利用したIssueの一括作成に対応するGitHub CLI Extension。

### GitHub Default Community Health Files

[https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)

GitHub公式の共通テンプレート管理機能。

gh-scaffoldはこれらを置き換えることではなく、**リポジトリの所有者や設定に依存しない、開発者個人のポータブルなワークフロー**を提供することを目指す。

## Project Status

**Planning / MVP Definition**

現在はMVPの要件定義段階であり、実装および一般利用向けの公開は今後の予定。

## License

TBD
