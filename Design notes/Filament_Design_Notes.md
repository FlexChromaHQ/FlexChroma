# Notes on Filament-like Models
### Fundamental Technologies for Printing Filament-Based Models with FlexChroma
---
### Revision History
- 2026-06 Initial document.
---
### Link
[Readme](https://github.com/FlexChromaHQ/FlexChroma/blob/main/README.md), 
[Blend Filament Design Notes](https://github.com/FlexChromaHQ/FlexChroma/blob/main/Design%20notes/Blend_Filament_Design_Notes.md), 
[Gradient Filament Design Notes](https://github.com/FlexChromaHQ/FlexChroma/blob/main/Design%20notes/Gradient_Filament_Design_Notes.md)

## Scope
This document describes technologies shared by all FlexChroma filament-like models.
Line-specific algorithms are documented separately.

## Equipment Used
- プリンタ：Bambu Lab製 FDMプリンタP2S
- ホットエンド：0.4mm焼き入れスチールホットエンド　又は、0.4mm大流量ホットエンド
- マルチマテリアルシステム：Bambu Lab製 AMS2 Pro
- カラーセンサー：NIX製 mini3
- モデリング機材：Windows11 PC(CPU:Corei7-12700, RAM:48GB, RTX3060Ti)

## Flexchroma Overview
本書では、すべてのFlexChromaフィラメントモデルに共通する技術について解説します。

### FlexChroma Hierarchy
FlexChromaは独自のフィラメントを作成するためのシステムであり、既に投稿済みのBlend Filamentと今後投稿予定のGradient Filament、TriBlend Filament、******** Filament、 ******** Filament、******** Filament、******** Filament、******** Filamentからなる。
上記の順番が開発優先順。

FlexChromaと言うBrandの下にBlend Filament等のSeriesが複数あり、Seriesの下にS FamilyやM Familyと言ったプリンタに合わせたFamilyをおいている。各FamilyにはS4 modelと言うように大きさの違うmodelがあり、各modelの中には異なったProfileが収められている。

Profileごとの違いはSeriesごとにその内容が異なる。各Profileには複数のPlateが設けられており、ProfileとPlateの組み合わせでSeriesごとの特色の調整が可能である。例えばBlend Filamentでは混色比が特徴となっており、ユーザーは要望に合ったPlateを選択することとなる。

## Basic Shapes of FlexChroma
モデルの形状はアルキメデスらせん状のモデルである。例えばM1シリーズの場合は、最も内側はR30mm程度であり、最も外側はR127mm程度となる。
螺旋のピッチは2mm程度であり、らせんの断面はΦ1.75mmの円形状である。Blend Filamentにおいてはこの円形状を51分割し、適切に2つのフィラメントを割り振ることで想定の混合比率を作り出している。

モデルの設定で定着性に重要なのは1層目線幅と1層目速度。きれいなスライスのために通常線幅の設定と薄い壁面検出を有効にしている。印刷したものはフィラメントとして使用するため、インフィル密度100％も重要である。

フィラメントとして使用する際に邪魔になるためブリムはOFFの設定としているが、らせんの両端にモデルによるブリムを設けている。ちなみにこのブリムにはサイズと混色比などと言った出来上がったフィラメントがどのような物かを識別でき情報が記載されており、すぐに使わない場合の内容確認に便利である。

### Common Model Settings and Profiles for Filament Creation
フィラメント作成の成否を握るのはProfileであり。Profileにて設定された層高さに合わせた断面形状、そしてその前提となるホットエンドの種類である。その内容を下記に記す。
- ホットエンド：0.4mm　定着性で1層目0.24mmが必要であるため、0.2mmノズルでは対応不可能。均質な混合のために2層目以降を0.08mmとしたいため、0.6mm、0.8mmノズルが対応不可能であり、0.4mm専用となる。
- 1層目層高さ：0.24mm　定着性に最も影響する項目。失敗を防ぐために十分に厚くしている。
- 2層目以降層高さ：0.08mm　色の混合に影響する項目。均質な混合のため可能な限り薄くしている。
- 1層目線幅：0.55mm　定着性に影響する項目
- 外壁線幅：0.55mm　フィラメント幅に影響する項目
- 薄い壁面検出：ON　できるだけ小さい断面のスライスを可能とするための設定。Archeよりもこちらの方がよい。
- インフィル密度：100%　フィラメントとして使うために内部を充填する。
- インフィル形状：Linear　インフィル密度100％を設定すると自動的に反映される項目。
- 1層目速度：50mm/s　定着性に影響する項目。
- 外壁速度：250mm/s　速度、バリの発生に影響する項目
- インフィル速度300mm/s　速度に影響する項目
- 一層目AUX冷却ファン：20%　定着性に影響する項目

### FlexChroma Sizes
各SeriesはサイズごとにFamily分けしており、Blend Filamentを例にとると最も小さく3Dbenchyがちょうど作れるS4 modelからS3, S2, S1, M4, M3, M2と順に大きくなってゆきM1モデルが最大サイズとなる。Blend filamentを例にとるとそれぞれの重量はPLAで印刷した場合で（S4:12g, S3:19g, S2:26g, S1:33g, M4:43g, M3:53g, M2:63g, M1,73g）

S FamilyはA1mini(180×180mmプリントベッド)で印刷可能なFamily、M Familyは256×256mmプリントベッドで印刷可能なFamilyとして展開している。
S1, M1はそれぞれプリンタサイズの限界の大きさであり、S2, M2はS1, M1で失敗する場合の代替サイズとしている。

H2S等の320×320mmサイズ向けにはL Familyとして想定しているが、現状対応するプリンタを保有していないため投稿の予定が立っていない。


## Known Issues
### Adhesion
らせんのピッチも定着性に影響し、1.98mm程度だと、隙間を開けているはずの部分で部分的にくっついてしまう事が何か所にもわたって発生する。
しかしこのくっつく部分が定着には好影響を及ぼし、上手く印刷できることが多い。

一方でピッチが2.06mm程度だと全くくっつかなくなる代わりに、途中での剥がれが発生した時にその部分がフィラメントとして使えない状況が発生する。
そのため、一部モデルではピッチが小さい通常モデルの他にピッチの大きいワイドクリアランスモデルを設けた。

一部の定着不良がツールヘッドに引っかかり、連続的に印刷物をはがしてしまう恐れがあり、定着不良による微小な失敗が完全な失敗となりやすい。
一方で中央から端までが接地面となるが、モデル自体の剛性は低いため、ABSに良く見られる反りによる剥がれは起きにくい。
定着面となる一層目はブリムの外周も含めて必ずフィラメントAによって一筆書きで造形される。

### Material compatibility
定着性に影響する条件としてはフィラメントの材質があげられる。実際の印象はテクスチャードPEIプレートの場合、PLA Basic＞ABS＞PETG＞PLA Matteの順に定着性が悪化する。

PLA BasicとテクスチャードPETプレートを中心にモデル設計してきたが、他の材質だと上手く行かないことが散見される。PLA Matteはサイズの大小に関係なく失敗することが多い。最大サイズだとPETGやABSが上手く行かないが、小さくすれば印刷可能。また常温プレートSupertackなどは密着性が良く困難なサイズ・材質でも成功率が高い。

この辺りを表にしてまとめたものをモデル画像として掲載中のため、最新の情報はそちらを確認されたい。

### Burrs
バリの発生が再現性(場所などの条件)なく発生することがある。ただし、発生するのは印刷時に瞬間的な停止をしている場所であり、エクストルーダーでの吐出が間に合っていないために一時停止するが、吐出はすぐに止まらずバリと言う形になってしまうのではないかと考えている。

エクストルーダー性能に見合った速度に落とすことで対策できる可能性あり（未検証）。現状では印刷に支障がないため、優先順位を下げている。

### User reports
P1SユーザーからBLD-Fの印刷でフィラメント同士がくっついてしまうという不具合報告があった。線間クリアランスを0.26mmから0.32mmに拡大した物を試してもらったが、改善せず。一方で他のP1Sユーザーからはとても良いモデル(恐らく成功している)と言うコメントが届いている。

私の使用プリンタはP2Sだが、P1Sの最悪条件を模して部品冷却ファンを止め、扉と天板を閉じ、Zオフセットを元のセッティングから0.06mm下げた数値にして試しの印刷おをこなったが、非常に良いプリント結果であった。

結局原因究明には至らず、対処療法として線間クリアランスを0.5mmに拡大したモデルを代替モデルとして追加することとした。その後の不具合報告はない。
