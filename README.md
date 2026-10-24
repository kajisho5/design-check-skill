# design-check-skill

Appleプラットフォーム（iPhone / iPad / iPhone Duo / Mac）向けのUI設計・画面仕様・デモモック・開発プロンプトを、プラットフォーム別のチェックリストで監査し、「抜け」（必要なのに無い・曖昧な項目）を見つけて直すためのスキルです。

- チェックリストは共通／iPhone／iPad／iPhone Duo／Mac／アイコン・成果物形式の6区分
- 監査は1回で終わらせず、抜けを直して再チェックする「監査ループ」を回します
- 終了条件は厳格です：「新規の抜けゼロ」のパスが3回連続するまで回します

## できること／できないこと

できること：
- 既存の設計・仕様・モック・プロンプトに対する、チェックリスト全項目の判定（OK／抜け／対象外）と抜けの根拠提示
- 抜けの修正（成果物自体の書き換え）と、修正後の再チェック
- パスごとの記録（何パス目で何件の新規の抜けが出たか）と最終報告

できないこと：
- デザインの制作（レイアウト、モック、アイコン、素材は作りません。監査専用です）
- デザインの「好み」の判定（抜けは「必要なのに無い・曖昧」だけを対象にし、好みは提案として分けて述べます）
- 対象外プラットフォームの強要（例： Mac版のないアプリにMacモックを求めることはしません）

## 使い方

1. スキル対応エージェント（Claude等）に、このリポジトリをスキルとして読み込ませる
2. 監査対象（設計書、画面仕様、モック、プロンプトなど）と、そのアプリの対応プラットフォームを渡す
3. 依頼する。例：
   - 「このモックをチェックして」
   - 「プロンプトのデザイン抜けを見て」
   - 「iPad向けの抜けがないか監査して」
4. 生成物： パスごとの判定結果と、修正済みの成果物、そして最終報告（総パス数、各パスの新規の抜け件数、修正内容、最終判定）

## 前提条件

- 監査対象になるデザイン成果物（UI設計、画面仕様、デモモック、開発プロンプトのいずれか）と、そのアプリの対応プラットフォームが分かっていること
- チェックリストの数値基準は Apple の Human Interface Guidelines に基づきます。OSのメジャーアップデート時は数値の再確認を推奨します

## ファイル構成

- `SKILL.md` — 監査ループの手順（終了条件を含む）と、共通／iPhone／iPad／iPhone Duo／Mac／アイコン・形式のチェックリスト、出典一覧
- `README.md` — このファイル
- `LICENSE` — MIT License

## 自分の環境に合わせる箇所

- アイコン作成ツール（チェックリストではレイヤードアイコン作成手段全般を指します。例： kajisho5/fresnel）
- 自社ルールとして上乗せしている項目（例： アイコンへの文字入れの全面禁止は、HIGの「原則避ける」より厳しい運用ルールです。自組織の方針に合わせて調整してください）
- 視覚成果物の提出形式（ここではラスター画像プレビューの添付を必須にしています。確認環境に合わせて変更可）

## よくある質問

**Q. デザインを作ってくれるスキルですか？**
A. いいえ。監査専用です。既存の設計・モック・プロンプトの「抜け」を見つけて直すことが仕事で、デザインそのものは制作しません。

**Q. なぜ「3パス連続ゼロ」まで回すのですか？**
A. 1回ゼロになっただけでは、見落としと本当のゼロを区別できないためです。修正のたびに観点の抜けや新たな不整合が生まれうるので、ゼロが3回連続して初めて「安定して抜けがない」と判断します。各パスの結果は記録され、報告に含まれます。

**Q. チェックリストの数値（44×44ptなど）の根拠は？**
A. Apple の Human Interface Guidelines（Accessibility）に基づきます。SKILL.md 末尾の「出典・参考」に公式ページへのリンクがあります。

**Q. 自分のアプリはiPhoneだけなのに、iPadやMacの項目もチェックされますか？**
A. いいえ。対象アプリの対応プラットフォームに該当するチェックリストだけを適用します。

**Q. app-factory-skill との関係は？**
A. app-factory-skill（アプリ量産のプロンプト生成）がプロンプトを作る側、このスキルがそのプロンプトのデザイン面を監査する側です。単独でも使えます。

## 出典・参考リンク

チェックリストの数値・仕様の根拠です（詳細は SKILL.md 末尾の「出典・参考」）。

- Apple Human Interface Guidelines — Accessibility: https://developer.apple.com/design/human-interface-guidelines/accessibility
- Apple Human Interface Guidelines — App Icons: https://developer.apple.com/design/human-interface-guidelines/app-icons
- Apple Human Interface Guidelines — Materials（Liquid Glass）: https://developer.apple.com/design/human-interface-guidelines/materials
- Apple Human Interface Guidelines（トップ）: https://developer.apple.com/design/human-interface-guidelines

## 関連プロジェクト

- app-factory-skill（例： kajisho5/app-factory-skill）— アプリ量産のプロンプト生成（この design-check-skill と併用）
- fresnel（例： kajisho5/fresnel）— Liquid Glass アイコン作成ツール

## ライセンス

MIT License（`LICENSE` を参照）
