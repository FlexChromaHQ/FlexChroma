## FlexChroma™
  Beyond standard filament color.
  > Print Colors You Imagine.

[🏠 FlexChroma HUB](https://flexchroma.carrd.co/)

---

# 混色フィラメント研究ノート（Multicolor Blend Filament Research）

本プロジェクトは、FDM 3Dプリンタで使用される「フィラメントカラー」の特性を体系的に記録・分析するための研究ノートです。

既に研究が進んでいる Web カラー（RGB）やプロセスカラー（CMYK）とは異なり、  
**フィラメントカラーは材料特性・透過率・粒子分布・押出条件など多くの要因が絡み合うため、色の再現性や混色挙動が未解明の部分が多い**という特徴があります。

本リポジトリでは、これらの未知の領域を実験と測定によって明らかにし、  
「3Dプリントにおける色の理解と再現性向上」を目指します。
・・・というところを目指したいけれど、今は個人メモ

## 🎯 目的（Purpose）

- Web カラーやプロセスカラーとは異なる **フィラメント固有の色挙動**を明らかにする
- 色材の混合比・ノズル径・流量・層高・モデル形状などが色に与える影響を体系化
- 縦長で多数の混色比を一括で印刷できるテストモデル「**Blend Color Checker**」を設計
- 混色フィラメントの再現性・予測性を高めるための基礎データの蓄積
- 既存の色空間（RGB/CMYK/Lab）とフィラメントカラーの関係性を探る

## 📦 研究内容（Contents）

# 1. 研究対象：フィラメントの混色における色の挙動

- Blend color checkerを用いた色ムラの比較
  
  　　**以降はいったんペンディング**
- 0.2 / 0.4 / 0.6 / 0.8 mm（標準）：いったんペンディング
- 0.4 / 0.6 / 0.8 mm（高流量）：いったんペンディング
- ImageJ による明度・色差の標準偏差測定

# 2. 色の印刷方法について
   
   最も小さく測定に向くモデルはS4シリーズであるが、それでも1つ印刷するためには1時間かかり、更にそれを測定できる形状に印刷するために30分程度かかる。
   Blend Filamentは中間色が33色、比較用に原色が2色と考えると、全部の色の印刷を行うためには2.2日かかることになる。
   当然1つの組み合わせだけではなく、様々な色の組み合わせについて確認するべきであるため、従来の方法での色の測定は実現性に乏しい。

   そのためこの35色を一度に印刷する事を目的に**Blend Color Checker**を開発した。これにより従来最速で2.2日かかっていた印刷が4回の印刷での合計5時間まで減少した。
   当初は一度に印刷しようと考えていたが、A1 mini環境でも印刷できるようにするためStandard向けCheck FilamentとDetail向けCheck Filmentに分けた。

　**Blend Color Checker**による任意の色同士の混合比ごとの色変化
  Blend color Checkerの仕様：Check FilamnentとCheck towerの2モデル構成。
  
  Check Filamentは21段のBlend Filamentをつなげてフィラメントにしたもので、StandardとDetail向けがある。
  - Standard:0～100％まで5%刻み21段のフィラメント
  - Detail:0,0.5,1,1,5,2,3,4,5,7.5,10,50,90,92.5,95,96,97,98,98.5,99,99.5,100%の21段のフィラメント。
  
  Check Towerは17×17×180ｍｍの縦長に印刷される四角柱Check Filamentの段の入れ替わりによって異なる混色比を観察できる。
  - 目印としてノッチを付けており、スムーズに印刷できると混色比の切り替わり位置とノッチが一致する。
  - 外部スプール活用で無駄なフィラメント消費を抑えるが、スタートパージのバラつきなどによって色位置がずれることがある。
  - 4面にA,B,C,Dと識別文字を付けており、面ごとの違いを確認できるようにしている。




# 3. 測定方法

- 測定機器：NIX mini3 個人でも購入しやすいカラーセンサーとして採用
- 撮影条件の統一：NIX mini3とBlend Color CheckerをつなぐものとしてSensor holderを開発した。
  NIX mini3を固定し、Blend Color Checkerをスライドさせることで測定したい場所に移動、固定されるため測定が容易。


## 📊 評価指標（Metrics）

● カラーセンサー L*a*b* 値

  カラーセンサーで測定したHEX値の記録。資料には直接出さない。
  
● HEX値

  カラーセンサーで測定したHEX値の記録。
  数値をまとめて実測値のHEX値(4面の平均値)を算出する。
  - そのままだと暗く、鈍い色として測定されるため０％と１００％の測定結果とフィラメントのカタログ値(Bambu LabはHEXの記載あり)の差分を測定。
    その差分を用いて混色比をカタログ相当値として補正した物を測定結果とする。

● 色差（ΔE）

  4面の色のバラつきは各面同士のΔE00の数値を計算し、その最大値をバラつきとして提示する。
  - Ver.1においてはDetailのΔEは1程度であるが、Standardは平均して高く悪い場合は10近いことがある。
    これはDetailモデルは混ぜる側のフィラメントが表に出ない構造になっていることが良い結果を生んだと考える。
　これは各混色モデルのスペックでもある。


```
## 📁 ディレクトリ構成（Structure）

blend-filament-research/
├── README.md
├── nozzle-tests/
│ ├── 0.2mm/
│ ├── 0.4mm/
│ ├── 0.6mm/
│ └── 0.8mm/
├── color-measurements/
│ ├── imagej-results.csv
│ ├── roi-settings.md
│ └── analysis-notes.md
├── swatch-design/
│ ├── stl/
│ └── design-notes.md
└── photos/
```


## 🧪 使用ツール（Tools）

- **NIX mini3**：L*a*b* 値の取得
- **Bambu Lab P2S 系列**：造形
- **Blend Color Checker**：比較用モデル
- **Fiji / ImageJ**：明度・色差の標準偏差測定 将来的に活用の可能性あり。


## 📝 License

This project is licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0).

🙌 作者（Author）

- ひつじぐも（Sheepycloud）
- 3Dプリント × フィラメントカラー研究

  ---

## Explore FlexChroma

- [🏠FlexChroma HUB](https://flexchroma.carrd.co/)
- [📁 FlexChroma Data Archive]()

Print Colors You Imagine.
