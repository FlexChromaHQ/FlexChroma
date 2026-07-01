# Windows開発環境整理の経緯（ドラフト）

## 目的

FlexChromaの開発環境を整備するため、

- Cドライブ容量不足の解消
- OneDriveの整理
- GitHub Desktop導入
- 将来のModelCore・ColorCore開発基盤構築

を目的として環境整理を開始した。

---

# 1. 特殊フォルダーをDドライブへ移動

Windowsの

- デスクトップ
- ドキュメント
- ピクチャ
- ビデオ
- ミュージック

などの特殊フォルダーをDドライブへ移動することを決定。

しかしOneDriveとの関係が複雑で、思うように移動できなかった。

**添付画像**

- 特殊フォルダー設定画面
- OneDriveバックアップ画面

---

# 2. OneDriveとの競合

OneDriveがオンライン専用ファイルを保持していたため、

特殊フォルダーを解除できなかった。

そのため

> 「このデバイス上に常に保持する」

を実行し、ローカルへダウンロードを開始した。

**添付画像**

- 「このデバイス上に常に保持する」の操作画面
- OneDrive同期画面

---

# 3. 約15万件の同期

ここが最も時間の掛かった工程だった。

OneDriveが約15万件のファイルを同期。

途中で

- 約6万件まで減少
- 仕事へ行く時間になったため放置

最終的に同期は完了した。

**添付画像**

- 約15万件同期中
- 約6万件まで減少
- 同期完了画面

---

# 4. OneDrive解除後の違和感

同期終了後、

Windowsの「ピクチャ」

と

OneDrive内の「Pictures」

の内容が一致しないことに気付いた。

ここから原因調査を開始した。

**添付画像**

- Windows側ピクチャ
- OneDrive Pictures
- プロパティ画面

---

# 5. ChatGPTとの調査

ChatGPTと一緒に

- プロパティ
- cmd
- dir
- shell

などを利用して調査。

しかし

Windows特殊フォルダーの挙動が通常と一致せず、

原因を特定できなかった。

この時点で

「餅は餅屋」

という考えに至った。

---

# 6. Copilotへ相談

Windows・OneDriveはMicrosoft製品であるため、

Microsoft Copilotへ相談することを決定。

それまでの状況を詳細に整理し、

ChatGPTからCopilotへ引き継ぎを行った。

**添付画像**

- Copilotへの相談内容

---

# 7. Copilotによる解析

Copilotは

- Windows特殊フォルダー
- OneDriveバックアップ
- Windowsの内部仕様

を前提に解析。

その結果、

特殊フォルダーをDドライブへ移動し、

バックアップしたいフォルダーだけをOneDrive配下へ配置する構成を提案した。

ChatGPTは

「同期完了まで待った方が安全」

という助言だったが、

Copilotは

「OneDriveのスキャンはバックグラウンド処理なので通常作業を続けても問題ない」

というMicrosoft製品特有の知識を示した。

**添付画像**

- Copilotの回答

---

# 8. 最終構成

最終的に

Windows特殊フォルダー

↓

Dドライブ

OneDrive

↓

バックアップ対象のみ

3DP

↓

OneDrive管理

Git

↓

OneDrive外

という構成になった。

**添付画像**

- Dドライブ構成
- OneDrive構成

---

# 9. GitHub Desktop導入

その後、

GitHub Desktopを導入。

Public / Privateリポジトリを分離し、

ローカルへCloneした。

```
D:\Git
├── FlexLab
└── FlexChroma
```

これにより

GitHubを中心とした開発環境が完成した。

**添付画像**

- GitHub Desktop
- Clone画面
- Clone完了画面

---

# 10. 完成した開発環境

最終的に

```
D:\
├── Git
│   ├── FlexLab
│   └── FlexChroma
│
├── OneDrive
│   └── 3DP
│
├── デスクトップ
├── ドキュメント
├── ダウンロード
├── ピクチャ
├── ビデオ
└── ミュージック
```

という構成になった。

これにより

- OneDrive
- GitHub
- GitHub Desktop
- Windows特殊フォルダー

の役割を整理でき、

今後ModelCore・ColorCoreの開発を進める土台が完成した。

---

# ChatGPTが間違えたポイント

## ① Picturesを同一フォルダーだと勘違いした

途中で

- Windows特殊フォルダー Pictures

と

- OneDrive\Pictures

を同じフォルダーだと思い込んでしまった。

しかし実際には全く別物だった。

ユーザーから

「全然一致していません」

と指摘され、

認識を修正した。

**添付画像**

- Windows Pictures
- OneDrive Pictures
- プロパティ比較画像

---

## ② OneDrive同期について

ChatGPTは

> 「同期完了まで待った方が安全」

と助言した。

一方Copilotは

> 「OneDriveのスキャンはバックグラウンド処理なので通常作業を続けても問題ない」

と説明した。

これはMicrosoft製品に関する専門知識の差だった。

**添付画像**

- Copilot回答

---

# この出来事で感じたこと

今回最も印象的だったのは、

AIにも得意分野があることだった。

ChatGPTは

- 開発環境設計
- GitHub構成
- 開発方針
- 長期的なアーキテクチャ

には非常に強かった。

一方、

Copilotは

- Windows
- OneDrive
- 特殊フォルダー
- Microsoft製品

について、より正確で実践的な助言を行ってくれた。

今回の経験から、

「餅は餅屋」

という考え方はAIにも当てはまると感じた。

それぞれの得意分野を理解し、適切に使い分けることが、最も効率よく問題を解決する方法だと実感した。
