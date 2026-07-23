# Notes on Gradient Filament
### Fundamental Technologies for Printing Filament-Based Models with FlexChroma
---
### Revision History
- 2026-06 Initial document.
---
### Link
[Readme](https://github.com/FlexChromaHQ/FlexChroma/blob/main/README.md), 
[Filament Design Notes](https://github.com/FlexChromaHQ/FlexChroma/blob/main/Design%20notes/Filament_Design_Notes.md), 
[Blend Filament Design Notes](https://github.com/FlexChromaHQ/FlexChroma/blob/main/Design%20notes/Blend_Filament_Design_Notes.md)

## Scope
This document describes technologies shared by all FlexChroma filament-like models.
Line-specific algorithms are documented separately.

## Equipment Used

Filament_Design_Notes.mdと同一

## Gradient Filament Overview

本書では、FlexChroma Gradient Filamentモデルに共通する技術について記録します。

## About Gradient Filament
Gradient FilamentはフィラメントAからフィラメントBへ段階的に変化することでグラデーションを実現するモデルです。
A→Bの変化は35段階を想定しており、各段階の長さは折り返し付近を少し長くして他は同程度の長さを想定しています。
各段階の混合比は人間の目から見て色が均等(色覚的均等)に変化するような混合比を目指します。
色覚的均等を実現することは困難ですが、今回は代替手段としてΔE00による計算と代表色カーブによる補正を用います。

## The Development History of Gradient Filament

### Early Gradient Filament
最初期のGradient FilamentはBlend Filamentを検討してすぐに作りました。データとしては残っていませんが、最初期の５％～９５％のBlend Filamentを用いて作ったGradient Filamentです。
noteでは記事として投稿するものの、その後はBlend Filamentの完成を第一目標とするため、いったん棚上げされていました。その後Blend Filamentの投稿がひと段落して、JRRF2026の前にMakerworldへ投稿すべく開発を開始しました。

開発中にBlend Color Checkerのコメント欄にBlend Color Checkerの両端の識別色をなくしてGradient Filamentの様なものが作れないか？と言う意見をもらいました。
この意見は想定されたもので、作ること自体は大した工数が必要ではなかったためNew Series PreviewとしてGradient Filamentのテザーモデルとして公開しました。

### Applying the Color Curve
開発の中で課題になったのは白と黒のBlend Color Checkerからのフィードバックでした。等間隔に並んでいて等割合で変化するBlend Color Checkerの白と黒のフィラメントの印刷物は多くの部分が黒色で黒の少ない部分がかろうじてグレーに見えるというかなり偏った物でした。
実は他の印刷結果も何らかの偏りがあり、リニアな変化は色相や明度が近い組合せに限られました。
この結果から分かる事は単純に等割合で等間隔なBlend Filamentを並べるだけではだめだという事です。

幸い色と混色比の情報はColor Chartに記載したものがありました。それらのデータから色の傾向を3つに分け、それぞれの色の傾向をカラーカーブと名付けました。
更にその内の明度差や透過率の差が大きい組合せに現れやすいカーブのうち黒を含むものを分離し、4つのカラーカーブにまとめました。

- Vivid Curve:色相差が大きく、明度差はそれほどではない組合せのカーブ
- Neutral Curve:色相差も明度差も大きく違わない組合せのカーブ
- Contrast Curve:明度差や透過率の差が大きい組合せのカーブ
- Deep Curve:黒を含む組み合わせのカーブ

このカーブに従い、等間隔に並べたフィラメントの混色比を各カーブ事に適切な値にすることで、人間の目で見た時に素直なグラデーションとなるように設定することとしました。
ちなみに黒だけを別カーブとしたのは黒が人間にとって非常に特殊な色であるためです。センサーでは濃いグレーとなっていても肉眼では黒に見えてしまいます。そのためDeep Curveでは60%グレーくらいの領域をきわめて広く取った極端なカーブとしました。

### About the Cycle Count
Gradient Filamentはモデル自体のサイズとそのモデルに置いて何回の色の入れ替わりがあるかで周期が変わります。
この入れ替わり周期は一般的な言葉の定義がなく、以下のように定めました。

1. グラディエント長さ(Gradient length):色の入れ替わり長さ(A→B)を示す。
1. サイクル長さ(Cycle length):繰り返しパターン(A→B→・・・→A)の長さを示す。
1. サイクル回数(Cycle count):モデル内での繰り返しパターン(A→B→・・・→A)の回数を示す。

１の意味として使われる用語はColor change lengthが多いですが、Makerworldへの投稿であることと、Bambu labの影響力を考慮しグラディエント長さを採用しました。
一般的に使われているのは１ですが、複数色にまたがる場合は不適切です。そのためFlexChromaでは２と３を通常利用し、パラメータとしては３を使用することとします。

これらCurveとCycle Countを反映したモデルを投稿しようと思いましたが、JRRF2026には間に合わないことが想定されたためGradient Filament S4としてはVivid CurveとContrast Curveのみを適用したモデルとしてCycle Countは0.5～2までを投稿し、その他は後日反映することとしました。

### The Failure of the S4 beta Model
JRRF2026に何とかVivid CurveのCycle Count4のモデルが間に合いました。これをさっそく印刷してみましたが、どうも上手く行きません。色の切り替わりのタイミングで必ずバリが発生しており、特に切り替わりの多いCycle Countが4のモデルでは大量に発生してしまっています。
このバリが印刷時にかなりのフリクションとなっており、使用が困難な状況となっておりました。
改めて確認するとCycle Count数が少ないほどフリクションが少ないですが、引っ掛かることがあるという点は変わらないという事が分かりました。

この不具合に対応するため対策の検討を行います。
対策案と結果は以下の通りでした。

1. 外壁の印刷速度を抑える：若干の効果あり。
1. 使用フィラメントブランド統一：ブランド差による影響なし。
1. 混色比切り替え断面を斜めにする：バリの発生が斜めになるが発生してしまう。
1. 外壁色固定：バリはほぼ発生しないが、外壁切り替えのタイミングでフィラメントA100％の筋が発生する。　(0%から50％付近まで外壁をAで固定し、50％位置で外壁A内側B→外壁B内側Aへ一気に切り替える)

この様に解決案が見つからないという状況となります。
そのため一旦「Gradient Filament S4」は「Gradient Filament S4 Beta」へ変更し、後処理(バリ除去)が必要なモデルと明記するという対応を取りました。

### The Problem of Excessive Man-Hours
Gradient Filament S4 Beta作成時に感じていたことですが、Blend Filamentに比べてはるかに多いモデリング工数がかかるという課題が分かってきました。
そもそもバリ除去の解決案を検討するにも時間がかかってしまうため、モデリング工数の根本的な解決を図ることにしました。

人間の手作業でのモデリングではなくスクリプトでの自動作成ができるようにModelCoreプロジェクトを始めることにしました。
工数問題の解決とバリ問題の解決加速のため**現在ModelCoreの開発中。**

---

## The following are some notes on Gradient Filament.

フィラメントの色の組み合わせにより各段階の混色比の最適案は異なってきます。
全ての組み合わせを網羅することは作成者と使用者の両方に負荷が大きすぎるため、代表モデルで代替します。
代表モデルはモデル名と代表的な組み合わせ、段階ごとの混色比で構成されるデータです。
代表モデルを決めるためにBlend Filamentで測定済みのカラーチャートを用います。
この代替モデルをカラーカーブと呼ぶ。カラーカーブの種類は以下の通り。

🌈 Vivid Curve(デフォルト)：色相差の大きい組合せに使用、
◑ Harf Curve：穏やかな色変化を実現する。Vividが0-100%で変化するのに対して0-50％に(色入替えで50-100%にも)対応する。
これは色入替えとつなぎ合わせることで通常の0.25サイクル相当の周期を実現する。
バリエーションとして25-75%に対応するモデルを検討する。
🩶 Neutral Curve：近い色同士の組合せに使用、
☀ Contrast Curve：明度差や透過率差の大きい組合せに使用、
⚫ Deep Curve：黒を含む組み合わせ専用

Gradient Filamentはモデル自体のサイズとそのモデルに置いて何回の色の入れ替わりがあるかで周期が変わる。
この入れ替わり周期は一般的な言葉の定義がなく、以下のように定める。
１．グラディエント長さ(Gradient length):色の入れ替わり長さ(A→B)を示す。
２．サイクル長さ(Cycle length):繰り返しパターン(A→B→・・・→A)の長さを示す。
３．サイクル回数(Cycle count):モデル内での繰り返しパターン(A→B→・・・→A)の回数を示す。
１の意味として使われる用語はColor change lengthが多いが、Makerworldへの投稿であることと、Bambu labの影響力を考慮しグラディエント長さを採用する。
また、一般的に使われているのは１だが、複数色にまたがる場合は不適切である。そのためFlexChromaでは２を通常利用し、表などには１を追記することがあるといった使い方にする。

モデル自体のサイズはBelnd filament同様の８種類だが、サイクル長さを考え微調整されている。サイクル回数は通常0.5、１、２、４買いが設定され、
重複に近いサイクル長さを除くとLine全体で３０種類の長さを使うことが出来るようになる予定である。

Gradient Filamentにおける課題：ContrastやDeep Curveでは0.5%以下の混色比が求められる。しかしBlend Filamentでの実績から0.4mmノズルでは0.5%までの分解能が限界である。
この限界を突破するために0.5%混色比断面と0%混色比断面を長さ方向で頻繁に切り替える事で対応する。切り替え周期は10mm以下を目標とする。
モデリング成功し、投稿準備中。
