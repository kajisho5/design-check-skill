# design-check-skill

iPhone / iPad / iPhone Duo / Mac 向けデザインの「抜け」を監査するためのチェック＆監査ループのスキルです（監査専用で、デザイン制作は行いません）。

## 使い方

Claude（やスキル対応エージェント）に読み込ませ、「このモックをチェックして」「プロンプトのデザイン抜けを見て」のように頼みます。手順とチェックリストは `SKILL.md` にあります。チェック → 修正 → 再チェックを、抜けがゼロになるまで回します。

## 構成

- `SKILL.md` — 監査ループの手順と、共通／iPhone／iPad／iPhone Duo／Mac／アイコン・形式のチェックリスト

## 関連

- kajisho5/app-factory-skill — アプリ量産のプロンプト生成（この design-check-skill と併用）
- kajisho5/fresnel — Liquid Glass アイコン作成ツール
