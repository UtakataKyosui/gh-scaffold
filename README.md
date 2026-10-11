# gh-scaffold

**Your workflow, any repository.**

リポジトリやOrganizationを横断して、一貫したIssue・Pull Request作成ワークフローを提供するGitHub CLI Extensionです。CLIに内蔵したMarkdownテンプレートを使い、リポジトリごとのテンプレート設定なしで利用できることを目指します。

## 開発状況

現在は実装前の計画段階です。実行可能な拡張機能・配布バイナリはまだありません。

Go製の単一バイナリとして実装し、GitHubの認証・Issue・PR作成には既存のGitHub CLIを利用します。独自のバックエンド、外部LLM、追加APIキーは必要としない設計です。

## v0.1.0で提供する機能

- 13種のIssueテンプレートと汎用PRテンプレートを内蔵。
- リポジトリのMarkdownテンプレートを優先し、ない場合は内蔵へフォールバック。取得元の明示指定にも対応。
- タイトル・本文の対話編集、投稿前の確認・取消、投稿しないdry-run。

GitHub Issue FormsのYAML形式はMVPの対象外です。

以下は実装予定のコマンド例です。現在は実行できません。

```sh
gh scaffold issue --type feature
gh scaffold issue --source builtin --type bug --dry-run
gh scaffold pr
```

## 実装計画

詳細仕様・依存関係・受け入れ条件は[v0.1.0 Epic (#1)](https://github.com/UtakataKyosui/gh-scaffold/issues/1)と各Issueで管理します。

- [CLI基盤・コマンド契約 (#2)](https://github.com/UtakataKyosui/gh-scaffold/issues/2)
- [内蔵Issue・PRテンプレート (#3)](https://github.com/UtakataKyosui/gh-scaffold/issues/3)
- [リポジトリテンプレート検出・取得元の優先順位 (#4)](https://github.com/UtakataKyosui/gh-scaffold/issues/4)
- [対話編集・確認・取消・dry-run (#5)](https://github.com/UtakataKyosui/gh-scaffold/issues/5)
- [Issue作成 (#6)](https://github.com/UtakataKyosui/gh-scaffold/issues/6)
- [Pull Request作成 (#7)](https://github.com/UtakataKyosui/gh-scaffold/issues/7)
- [自動検証・CI・配布 (#8)](https://github.com/UtakataKyosui/gh-scaffold/issues/8)

後続バージョンの計画:

- v0.2.0: [個人設定・ユーザー定義テンプレート・変数展開 (#9)](https://github.com/UtakataKyosui/gh-scaffold/issues/9)
- v0.3.0: [非対話モード・本文出力・JSON出力 (#10)](https://github.com/UtakataKyosui/gh-scaffold/issues/10)
- v0.3.0: [Agent Skill・外部AIの本文生成ワークフロー (#11)](https://github.com/UtakataKyosui/gh-scaffold/issues/11)

## License

未定。配布前に決定します。
