---
name: design-check-skill
description: 'Audit-only design completeness checker for Apple-platform apps (iPhone, iPhone Duo foldable, iPad, Mac, Apple Watch). Checks an existing UI design, screen spec, demo mock or development prompt against ID-numbered per-platform checklists, reports gaps (抜け) with evidence, fixes them (or writes fix instructions for visual files it cannot edit), and repeats audit passes, each from a viewpoint not yet used in the loop, until three consecutive passes find no new gaps. Does NOT create designs, layouts, mockups or assets. Covers HIG compliance, size-class layout and resizing, orientation policy, Dark Mode, Dynamic Type, VoiceOver, contrast, Reduce Motion/Transparency, Safe Area, keyboard occlusion, iPhone Duo outer/inner displays and Split View, iPad windowing/pointer/keyboard, Mac menus/shortcuts/windows, watchOS glanceability/complications, app icon specs and appearances, fictional mock data, PNG previews. Use when the user asks for a design check, 抜けチェック, or a mock/prompt review for an Apple-platform app.'
---

# design-check-skill

Appleプラットフォーム向けデザインの「抜け」を監査するためのチェックリストと監査ループです。**監査専用であり、デザインそのもの（レイアウト、モック、素材）を新しく作ることはしません。** 対象は既存のUI設計・画面仕様・デモモック・開発プロンプト。**チェックして終わりにせず、抜けを直して再チェックし、「新規の抜けゼロ」が3パス連続するまで回す**のがこのスキルの仕事です。

## 用語

- **抜け**： 必要なのに書かれていない／作られていない項目、または「実装・制作する側が迷わず再現できる具体性」がない曖昧な記述。好みの問題は抜けに含めない
- **新規の抜け**： そのパスで初めて見つかった抜け。**前のパスの修正で新たに生じた不整合（デグレ）も新規に数える**。前のパスで記録済みで、ユーザー判断待ちのため保留しているものは数えない（保留一覧に残す）
- **パス**： 適用対象の全チェック項目を1回判定し終えること

## 監査ループ（必須手順）

1. **対象と範囲を決める** — 監査する成果物と、そのアプリの対応プラットフォーム（iPhone / iPhone Duo / iPad / Mac / Watch）を特定する。不明ならプロンプトや仕様の記述から判断し、判断した根拠を報告に書く。対象外プラットフォームのリストは適用しない
2. **成果物の種類で修正方法を決める**
   - **テキスト成果物**（設計書・画面仕様・開発プロンプト）： 成果物を直接書き換える
   - **ソースを編集できる視覚成果物**（SVG / HTML / Figma 等でツールから編集可能なもの）： ソースを直接修正し、PNGプレビューも再書き出しする
   - **編集できない視覚成果物**（PNG / JPEG だけ、手元にソースがないもの）： 自分では描き直さず、**修正指示書**（どの画面のどこを、どの基準で、どう直すか）を出す。ループは「修正待ちで一時停止」として報告し、修正版が渡されたら同じパス番号の続きから再開する
3. **1パス判定する** — 適用対象の全項目を「OK / 抜け / 対象外」で判定し、抜けには**項目ID・根拠（どの画面・どの記述か）・修正案**を付ける。**パスごとに、そのループでまだ使っていない観点を下表から1つ選び、その観点で全項目を見直す**（同じ観点は1つのループで2回使わない。同じ見方を繰り返しても見落としは見つからないため）。表の順に使うのを基本とし、対象に合わない観点（例： 1プラットフォームだけのアプリで V-05）は飛ばして次を使う。観点から見つかった、チェックリストにない種類の抜けも記録する（IDは最も近い項目か「追加」）

   | ID | 観点 | 見方 |
   |---|---|---|
   | V-01 | チェックリスト順 | 共通→各プラットフォーム→アイコン・成果物の順に、全項目を上から判定する |
   | V-02 | ユーザーフロー順 | 初回起動→主要フロー→権限拒否→データ空→無料版の上限到達→購入・復元→エラー の順に画面を辿り、各画面で関係する項目を判定する |
   | V-03 | 状態マトリクス | ライト／ダーク × 標準／最大文字サイズ × 最小／最大ウィンドウ（iPadとMac）× 外側／内側／内側の Split View（iPhone Duo）× 視差効果を減らす・透明度を下げる の組み合わせで、各画面を想定する |
   | V-04 | 支援技術の利用者 | VoiceOver の読み上げ順に画面を辿る、画面を見ずに操作する、色の見分けがつきにくい、指先の細かい操作が難しい、という利用者になったつもりで判定する |
   | V-05 | プラットフォーム横断の一貫性 | 同じ機能が各端末で同じ名前・同じアイコン・同じ状態表示になっているか、端末ごとの違いが HIG に沿った意図的なものかを、機能単位で横に並べて比べる |
   | V-06 | 極端なデータと文言の長さ | 0件・1件・大量の件数、とても長い名前や本文、桁の多い数字、英語など文字数が増える言語に訳したとき、の各条件で崩れ・省略・はみ出しを探す |
   | V-07 | 文言と用語 | ボタン名・見出し・空状態・エラー・確認ダイアログの文言を全部読み、具体的で、端末間で用語がそろい、誤解や誇張がないかを判定する |
   | V-08 | 作る人の目 | 実装・制作する人がこの資料だけで迷わず作れるか（寸法・色・文字の指定、画面遷移、例外時の挙動、素材の書き出し形式に指定漏れがないか）を判定する |

4. **抜けを直す** — 手順2の方法で修正する。修正が設計判断（機能範囲、課金、プラットフォーム追加、自社ルールの変更など）を変える場合は勝手に決めず、選択肢と推奨案を提示して**保留**にする。保留した項目は以後のパスで新規に数えない
5. **再判定する** — 次のパス番号で手順3に戻る
6. **報告する** — 「報告フォーマット」に従う

**終了条件（厳格）**

- 「新規の抜けゼロ」のパスが **3回連続** したら終了する。1パスでも新規の抜けが出たら連続カウントは0に戻る
- パス上限は **8**（観点が8つなので、1ループで全観点を1回ずつ使い切ったところが上限）。上限に達しても3連続ゼロにならない場合は、そこで止めて残件と理由を報告する
- 上限に達した時点で見つけた抜けをすべて修正済みなら、修正後の成果物で**新しいループ**を始める（観点は V-01 から使い直す）。新しいループでも上限に達したら、そこで止めて報告する（ループは最大2回）
- 成果物の範囲が変わった（プラットフォームの追加など）ときも、新しいループとして数え直す
- 「1回ゼロだった」だけでは終了しない。保留がある場合は「3連続ゼロ・保留N件」として終了し、保留一覧を必ず添える

## チェックリスト

各項目の先頭はIDです。報告ではIDで参照します。[n] は末尾の出典番号です。

### 共通（全プラットフォーム）

- **C-01** Apple HIGへの準拠が明示され、HIGと実装側の判断が衝突したらHIG優先・報告すると決まっている [1]
- **C-02** ダークモード： 色はアセットカタログまたはシステムカラーで定義され、ライト／ダーク両方で確認する手順がある [2]
- **C-03** Dynamic Type： 最大文字サイズ（アクセシビリティサイズを含む）でも文字の切れ・重なりがなく、操作要素が画面外に押し出されない
- **C-04** VoiceOver： 主要操作にラベルがあり、読み上げ順序が画面の論理順と一致している。画像だけのボタンにもラベルがある
- **C-05** コントラスト： テキストと背景のコントラスト比が、17pt以下は4.5:1以上、18pt以上は3:1以上、太字は3:1以上（HIGがAccessibility Inspectorの基準として示すWCAG AA相当の値）。ライト・ダーク両方で確認する [2]
- **C-06** 色だけで情報を区別していない（状態や種類を形・アイコン・文字でも示す）[2][11]
- **C-07** 視差効果を減らす（Reduce Motion）がオンのとき、自動・反復アニメーションや拡大縮小の動きを抑える方針がある [2]
- **C-08** 透明度を下げる／コントラストを上げる（Reduce Transparency / Increase Contrast）がオンのとき、Liquid Glassや半透明素材の上でも文字が読める。独自の半透明UIを作る場合はこの2設定での見え方が規定されている [2][3]
- **C-09** コントロールのサイズと間隔： 各プラットフォーム節の数値を満たし、隣接するコントロールの間に十分な余白がある（目安： ベゼルありは周囲約12pt、ベゼルなしは約24pt）[2]
- **C-10** 文字サイズ： カスタム書体でも各プラットフォームの最小サイズを下回らない（iOS / iPadOS 最小11pt・標準17pt、macOS 最小10pt・標準13pt、watchOS 最小12pt・標準16pt）[2]
- **C-11** アイコン類はSF Symbolsを基本にし、独自アイコンは必要なものだけに限っている
- **C-12** レイアウトはサイズクラス（実際に使える幅と高さ）で決めており、端末の種類や向きで決め打ちしていない [4]
- **C-13** 空状態（データゼロ）の画面がある
- **C-14** エラー時・権限拒否時の画面と、設定アプリ等への代替導線がある
- **C-15** 無料版の制限（件数・機能）は、ユーザーが制限にぶつかる前に画面上で明示されている（残量表示、Pro限定項目の事前表示など）。制限到達時の挙動が説明され、データを無言で削除しない
- **C-16** 入力時にソフトウェアキーボードで入力中の項目や確定ボタンが隠れない
- **C-17** 起動画面（Launch Screen）がInfo.plistで構成されていて（iOS 27 SDK以降は提出の必須条件）、広告・ロゴアニメ・待機演出を入れていない [5]
- **C-18** UI文字列は String Catalogs で管理され、ハードコードされていない
- **C-19** モック・スクリーンショット・サンプルデータは**明らかに架空**の人名・住所・電話番号・会社名・画像を使っている（実在の人物・住所・企業・作品を使わない。宛名・連絡先系のアプリは特に注意）

### iPhone

- **P-01** 画面の向きの方針が明記されている（例： 縦向き固定。回転対応する例外はアプリごとに明記）
- **P-02** 向きを固定していても、ウィンドウサイズの変化（Mac上のiPhoneミラーリング、iOS 27以降のシーンのリサイズ等）でレイアウトが破綻しない [4][6]
- **P-03** Safe Area（Dynamic Island／ホームインジケータ等）と重なる位置に操作要素がない
- **P-04** 片手操作を想定し、主要アクションが画面下部〜中部に届く配置になっている
- **P-05** シート（ハーフモーダル）の使い分けと、閉じる操作（スワイプ／ボタン）が全画面で一貫している
- **P-06** タップ対象は44×44pt以上（HIGの標準値。28×28ptはアクセシビリティ上の最小で、これを下回らない）[2]
- **P-07** 主要画面のモックがiPhoneフレームで用意されている

### iPad

- **I-01** 縦・横どちらの向きでもレイアウトが成立する。`UIRequiresFullScreen` に頼っていない（iPadOS 26で非推奨。iOS / iPadOS 27 SDKでビルドすると、全画面固定にはならずリサイズされる）[6]
- **I-02** ウィンドウ表示で自由にリサイズされても崩れない。最小ウィンドウでも主要機能が使え、細長い・正方形に近い・横長のどの形でも成立する [4][7]
- **I-03** 画面の広さを使い切るレイアウト（サイドバー＋詳細など）で、iPhone画面の引き伸ばしにしていない
- **I-04** ポインタ（ホバー効果）とハードウェアキーボード（主要操作のショートカット）に対応している。主要コマンドはiPadのメニューバーにも出し、メニューバーにしかない機能を作らない（メニューバーは通常隠れているため）[8]
- **I-05** 主要画面のモックがiPadフレームで用意されている

### iPhone Duo（折りたたみ iPhone。iPhoneアプリはすべて対象）

iPhone Duo は2026年9月発表の折りたたみ iPhone（外側5.4インチ、内側7.6インチ）。iPhone アプリはそのまま iPhone Duo でも動くので、iPhone 対応アプリでは原則この節も適用する。

- **D-01** 外側（閉じた状態）と内側（開いた状態）の両方の主要画面が設計され、モックがある [14]
- **D-02** レイアウトを向きではなくサイズクラスで決めている。外側は通常の iPhone と同じ（縦向き＝横 compact・縦 regular）、内側は横・縦とも regular。内側はアプリの対応する向きの指定に従わないことを前提にしている [15]
- **D-03** 内側で一覧と詳細を並べるなど、iPhone 画面の引き伸ばしにしていない。内側の Split View（画面の左右どちらか半分）でも主要機能が使える [15][16]
- **D-04** 開閉したときに、表示中の画面・選択・入力中の内容が引き継がれる [14]
- **D-05** 外側と内側の横向きでは、ふつう上下にあるものが後端に縦一列で並ぶ（外側は上から Dynamic Island → ステータスバー → ツールバー → タブバー。タブバーも横側）ことを前提にしている。内側の縦向きは横向きのバー。ツールバーは 戻る／閉じる → 強調操作 → 残り の順で、記号のボタンに題名もある。Safe Area は非対称として扱う [16]
- **D-06** 横向きにも対応している（テントのように立てて置かれるため推奨）[15]
- **D-07** スクリーンショット用の画像が iPhone Duo の寸法（外側 1398 × 2034 px、内側 2007 × 2853 px）で書き出せる。現時点は任意だが、2027年4月以降は iOS 27.1 SDK 以降でビルドしたアプリで必須 [13]
- **D-08** 半分折ったときの中央の折り目（内側の縦向きでは横一文字、横向きでは縦一文字）に、見出し・ボタン・画像・数値など重要な要素がかかっていない（連続スクロールの本文やリストは可）。左右に分ける配置は中央で均等、グリッドは偶数列 [16]

### Mac（Mac版・Mac対応があるアプリのみ）

- **M-01** Macでの提供形態が明記されている（SwiftUIのマルチプラットフォーム等のMacネイティブ／「Designed for iPad」で動作）。Designed for iPadの場合はMac専用UIを作らない方針も明記
- **M-02** Macネイティブの場合、標準のメニューバー項目がある（アプリメニューの情報・設定・終了、編集、表示、ウィンドウ、ヘルプ。書類を扱うアプリはファイルも）[8]
- **M-03** 主要操作にキーボードショートカットが割り当てられ、標準ショートカット（⌘C / ⌘V / ⌘W / ⌘Q / ⌘, など）と衝突しない [8]
- **M-04** ウィンドウのリサイズに追従し、最小サイズでも主要機能が使える
- **M-05** ポインタ前提の密度とホバー表現になっている（iPad画面の単純な拡大にしていない）
- **M-06** コントロールの標準サイズは28×28pt（最小20×20pt）を目安にしている（iOSの44×44ptをそのまま持ち込んでいない）[2]
- **M-07** 主要画面のモックがMacのウィンドウ表示で用意されている

### Apple Watch（Watchアプリがあるアプリのみ）

- **W-01** 一目で分かる（glanceable）1画面完結の操作が中心で、1回の操作が短時間で終わる設計になっている [9]
- **W-02** 画面階層が浅く、縦方向の移動はDigital Crownでできる [9]
- **W-03** コンプリケーション（ウォッチフェイス上の表示）と通知で何を見せるかが決まっている [9]
- **W-04** 常時表示（Always On）で手首を下げたときの表示内容が決まっている（個人情報や細かい数値を出し続けない等）[9]
- **W-05** iPhoneが近くにないときの動作（単独で動く範囲と、同期待ちの表示）が決まっている [9]
- **W-06** タップ対象は44×44pt（最小28×28pt）、文字は標準16pt・最小12ptを目安にしている [2]
- **W-07** 主要画面のモックがApple Watchフレームで用意されている

### アイコンと成果物の形式

- **A-01** アイコンはレイヤードアイコン（.icon）として作られている。作成ツールは Icon Composer（Xcode同梱のApple公式ツール）または同等のツール（例： kajisho5/fresnel [12]）[10]
- **A-02** レイヤーは正方形でマスク前のもの（自分で角丸・円形マスクを付けていない）。背景レイヤーを読み込む場合は全面・不透明。前景はベクター（SVG / PDF）を優先し、文字はアウトライン化している [10]
- **A-03** ハイライト・影・ぼかし・光彩などの効果を自分で描き込まず、システムの効果に任せている [10]
- **A-04** iOS / iPadOS / macOS は、デフォルト／ダーク／クリア（ライト・ダーク）／ティント（ライト・ダーク）の外観を確認済み。HIG上は用意しない外観をシステムが自動生成するが、**自社ルールとして全外観を用意して確認する** [10]
- **A-05** キャンバスサイズが正しい（iOS / iPadOS / macOS は1024×1024px、watchOSは1088×1088pxで円形にマスクされる）。watchOSのアイコンは背景を黒にしていない [10]
- **A-06** アイコンに文字を入れていない（HIG上は「体験やブランドに不可欠な場合だけ」。**自社ルールとして全面禁止**）。グリフが小サイズでも判別でき、写真やUI部品の複製を使っていない [10]
- **A-07** 視覚成果物はベクター原版に加えてPNGのプレビューが添えられている（スマートフォン等、SVGを表示できない環境で確認するため）
- **A-08** App Store掲載用スクリーンショットに転用するモックは、App Store Connectの必須サイズで書き出せる（透過なしのPNG / JPEG。サイズは提出時に最新の仕様を確認）[13]
- **A-09** 2台並びなどの掲載用画像を作る場合、2画面の状態（時刻・データ・選択・ライト／ダーク）が一致し、役割分担が分かり、小さく表示しても文字が読める

## 報告フォーマット

```
対象： （成果物名） ／ 対応プラットフォーム： （判断根拠）
監査ループ結果： 総Nパスで終了（3連続ゼロ ／ 上限到達 ／ 修正待ちで一時停止）

| パス | 観点 | 新規の抜け | 連続ゼロ |
|---|---|---|---|
| 1 | V-01 チェックリスト順 | 5 | 0 |
| 2 | V-02 ユーザーフロー順 | 1 | 0 |
| 3 | V-03 状態マトリクス | 0 | 1 |
| … | | | |

見つけた抜け： ID ／ 根拠 ／ 修正内容（プラットフォーム別）
保留（ユーザー判断待ち）： ID ／ 選択肢 ／ 推奨案
提案（抜けではない好みの指摘）： 箇条書き
最終判定： 抜けゼロ（3パス連続）／ 残存する抜けとその理由
```

## やらないこと

- チェックだけで終わらせて修正しない（ループを回し切るのが仕事）
- 対象外プラットフォームの項目を抜けと判定する（例： Mac版のないアプリにMacモックを求めない）
- デザインの好みを抜けとして扱う（好みは「提案」として分けて書く）
- 編集できない画像を、自分で描き直した別物に差し替える（修正指示書を出す）
- 設計判断を勝手に変える（保留にして選択肢を出す）

## 出典・参考

チェックリストの数値・仕様は次の公式情報に基づきます（2026年10月時点で確認）。OSのメジャーアップデート時は数値を再確認してください。

1. Apple Human Interface Guidelines（トップ）: https://developer.apple.com/design/human-interface-guidelines
2. HIG — Accessibility（コントロールのサイズと間隔、文字サイズ、コントラスト比、視差効果を減らす）: https://developer.apple.com/design/human-interface-guidelines/accessibility
3. HIG — Materials（Liquid Glass。透明度を下げる・コントラストを上げる設定での見え方）: https://developer.apple.com/design/human-interface-guidelines/materials
4. HIG — Layout（サイズクラス、向きを固定してもリサイズに対応する）: https://developer.apple.com/design/human-interface-guidelines/layout
5. TN3208: Preparing your app’s launch screen to meet App Store requirements（iOS 27 SDK以降は起動画面が必須）: https://developer.apple.com/documentation/technotes/tn3208-preparing-your-apps-launch-screen-to-meet-app-store-requirements
6. TN3192: Migrating your app from the deprecated UIRequiresFullScreen key: https://developer.apple.com/documentation/technotes/tn3192-migrating-your-app-from-the-deprecated-uirequiresfullscreen-key
7. HIG — Multitasking（iPadのフルスクリーン／ウィンドウ表示）: https://developer.apple.com/design/human-interface-guidelines/multitasking
8. HIG — The menu bar: https://developer.apple.com/design/human-interface-guidelines/the-menu-bar
9. HIG — Designing for watchOS: https://developer.apple.com/design/human-interface-guidelines/designing-for-watchos
10. HIG — App icons（レイヤー、外観、仕様表）: https://developer.apple.com/design/human-interface-guidelines/app-icons
11. App Store Connect Help — Accessibility Nutrition Labels（「色だけに頼らない区別」等の申告項目）: https://developer.apple.com/help/app-store-connect/manage-app-accessibility/overview-of-accessibility-nutrition-labels
12. fresnel（レイヤードアイコン作成ツール）: https://github.com/kajisho5/fresnel
13. App Store Connect Help — Screenshot specifications: https://developer.apple.com/help/app-store-connect/reference/app-information/screenshot-specifications
14. Apple Newsroom — Apple unveils iPhone Duo（2026-09-10）: https://www.apple.com/my/newsroom/2026/09/apple-unveils-iphone-duo/
15. Apple Developer Tech Talk — Prepare your app for iPhone Duo: https://developer.apple.com/videos/play/tech-talks/111461/
16. HIG — Designing for iPhone Duo: https://developer.apple.com/design/human-interface-guidelines/designing-for-iphone-duo
