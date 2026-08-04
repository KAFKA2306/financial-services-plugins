# financial-services-plugins — Anthropic金融サービスpluginの旧snapshot

このリポジトリは、Anthropicが公開した金融サービス向けClaude plugin群を、2026年2月24日時点の履歴で保持しているsnapshotです。

**KAFKA2306独自の金融分析製品ではありません。** コード、skill、command、connectorの上流正準と最新情報は、Anthropicの現行リポジトリを確認してください。

- 現行上流: https://github.com/anthropics/financial-services
- 旧上流名: https://github.com/anthropics/financial-services-plugins
- このsnapshotの最終commit: `c5947c8b0686da6fc9c2e4bd4dbdcb7fd4073ff6`
- このsnapshotの最終commit日時: 2026年2月24日
- ライセンス: Apache License 2.0

> **状態:** upstream snapshot / 更新停止  
> **利用前提:** 現行上流との差分、plugin schema、connector、利用条件を再確認  
> **KAFKA固有差分:** 2026年8月4日のcommit監査では確認できず

---

## 何を含むsnapshotか

取得時点では、Claudeを金融業務へ適用するためのfile-based pluginが含まれています。

主な領域:

- financial analysis
- investment banking
- equity research
- private equity
- wealth management
- partner-built plugin

pluginは主に次の要素で構成されます。

```text
plugin-name/
├── .claude-plugin/plugin.json   manifest
├── .mcp.json                    MCP connector設定
├── commands/                    明示的に実行するcommand
└── skills/                      domain知識とworkflow
```

これらはMarkdownとJSONを中心とする指示・設定資産です。KAFKA2306の`investor`や`CrewTrade`の実装、data、model、検証結果とは別物です。

---

## 現行上流との関係

Anthropicの現行公開リポジトリは`anthropics/financial-services`です。現行上流では、pluginだけでなく、agent、managed-agent cookbook、Microsoft 365向け導入tool、検証scriptなどを含む構造へ拡張されています。

```text
anthropics/financial-services
  現行の上流正準
        │
        └─ 継続更新、schema変更、install経路変更

KAFKA2306/financial-services-plugins
  2026-02-24までの旧snapshot
  KAFKA固有機能の正準ではない
```

このsnapshotのREADMEやinstall commandを、現行上流の最新手順として使わないでください。

---

## KAFKA2306内での位置づけ

KAFKA2306の金融・投資研究で正準となる主なリポジトリは次です。

- [`KAFKA2306/investor`](https://github.com/KAFKA2306/investor) — 投資研究、企業知識DB/API、金利・為替、公開画面
- [`KAFKA2306/CrewTrade`](https://github.com/KAFKA2306/CrewTrade) — 定量研究catalogと証拠監査
- [`KAFKA2306/semiconductor-earnings-model`](https://github.com/KAFKA2306/semiconductor-earnings-model) — 半導体企業の決算model
- [`KAFKA2306/WealthAudit`](https://github.com/KAFKA2306/WealthAudit) — 個人資産の監査と予測

このsnapshotのskillを参考にする場合も、分析data、計算、証拠、成果物の正準は各domain repositoryへ保持します。

---

## 含まれるworkflowの例

取得時点の上流READMEでは、次のようなworkflowが説明されていました。

- comparable company analysis
- DCF
- LBO
- 3-statement model
- earnings update
- initiating coverage
- investment committee memo
- deal sourcing
- client review
- portfolio KPI monitoring

これらはpluginが支援するworkflowの説明です。分析結果の正確性、契約dataへのaccess、ExcelやPowerPointの完成品質、投資判断の妥当性を保証するものではありません。

---

## MCP connector

snapshotには、金融data providerへ接続するMCP設定例が含まれます。

例:

- Daloopa
- Morningstar
- S&P Global
- FactSet
- Moody's
- LSEG
- PitchBook
- Aiera
- Chronograph
- Egnyte

providerごとに契約、API key、認証、利用範囲、再配布条件、rate limitが異なります。

MCP URLがfileに書かれていることは、次を意味しません。

- 利用契約がある
- 認証済みである
- endpointが現在も有効である
- dataを公開成果物へ転載できる
- KAFKA2306がproviderを推奨している

---

## 利用する場合

### 現行版を使う

新規導入では、原則としてAnthropicの現行上流を確認します。

```text
https://github.com/anthropics/financial-services
```

install command、marketplace名、plugin schemaは変更され得ます。現行上流READMEとClaude製品の公式資料を確認してください。

### このsnapshotを調査する

特定時点の構造比較、skill設計研究、差分監査など、snapshotそのものを調べる場合に使用できます。

```bash
git clone https://github.com/KAFKA2306/financial-services-plugins.git
cd financial-services-plugins
git log --oneline --decorate
```

pluginを実行する前に、少なくとも次を確認します。

- `.claude-plugin/plugin.json`
- `.mcp.json`
- `commands/`
- `skills/`
- connectorの認証要求
- shellや外部toolを実行する指示
- 現行Claude Code / Cowork schemaとの互換性

---

## 更新・同期方針

このリポジトリでは、2026年8月4日時点で自動同期workflowを確認していません。

今後の選択肢:

1. snapshotとしてarchiveする
2. 現行上流へ追随するmirrorとして再構築する
3. 必要なskillだけを`agent-resources`へ出典付きで移植する
4. KAFKA固有pluginを別directoryまたは別repositoryで管理する

上流追随とKAFKA固有変更を同じbranchへ無秩序に混ぜません。継続する場合は、upstream remote、基準commit、差分一覧、同期手順をREADMEへ追加します。

---

## セキュリティ

金融pluginは、機密dataや外部providerへ接続する可能性があります。

実行前に確認するもの:

- pluginの全文
- MCP endpoint
- 認証方法
- providerの利用契約
- file読取・書込範囲
- spreadsheet、document、presentationへの出力内容
- 外部通信
- prompt injectionへの耐性
- 個人情報、顧客情報、未公開情報の取扱い

保存しないもの:

- API key、token、cookie
- 顧客data
- 未公開案件情報
- private equityの非公開資料
- 証券口座情報
- 契約上再配布できないprovider data

---

## 既知の制約

- 現行上流より古いsnapshotです。
- 現行のinstall path、marketplace名、plugin schemaと一致しない可能性があります。
- KAFKA固有の変更、検証、CIを確認できません。
- connectorの現在稼働や利用権を保証しません。
- AI生成の金融分析は専門家レビューと一次資料照合が必要です。
- このrepositoryは投資助言、売買推奨、法務・税務助言を提供しません。

---

## ライセンス

Apache License 2.0。詳細は[`LICENSE`](LICENSE)を確認してください。

上流plugin、partner-built plugin、外部provider data、生成成果物には、別の利用条件が適用される場合があります。

**README実体監査:** 2026年8月4日
