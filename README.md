# ToPu_Resample

[![Blender](https://img.shields.io/badge/Blender-4.2%2B-F5792A?logo=blender&logoColor=white)](https://www.blender.org/)
[![Version](https://img.shields.io/badge/version-1.30.0-blue)](CHANGELOG.md)
[![License](https://img.shields.io/badge/License-GPL--3.0--or--later-blue.svg)](#ライセンスとクレジット)

**選択した辺列・ループの頂点数を、形とUVを保ったまま増減するBlenderアドオンです。**
Change the vertex count of selected edge chains and loops while keeping their shape and UVs.

スムーズツール（**Smooth**）も同梱しています。

- [日本語](#日本語)
- [English](#english)

---

## 日本語

### 主な機能

- **リサンプル**：選択した辺列・閉じたループの頂点数を、ホイールで増減します。
  - なめらかな曲線に沿って頂点を並べ直します。形状キープ・曲率補正・均等配置も選べます。
  - 円柱のように面でつながった複数のループは、頂点の位置を揃えて処理するため、縦の辺がねじれません。
  - 円に近い閉じたループは、曲率補正で円形に整えられます。
  - UV、頂点グループ、頂点色、シーム、シャープ、クリースなどの属性を引き継ぎます。頂点が元の辺から外れて動いても、テクスチャが面の上でずれないようにUVを補正します。
- **スムーズ**：スムーズ量（±%）とカーブ量（±%）の2つの値だけで、辺列・ループをなめらかにします。マイナスのスムーズ量で曲がりを強調することもできます。
- 実行中は画面下部にHUDを表示します。ボタンのクリックや値のドラッグで直接操作できます。
- 3Dマウス、中ボタン、テンキーでの視点操作をそのまま使えます。
- 日本語 / English のUIに対応します。

### 対応環境

- Blender 4.2 以降（4.2 / 5.0 / 5.1 で動作確認）
- メッシュの編集モード

### インストール

1. [Releases](https://github.com/http4211/ToPu_Resample/releases) から ZIP をダウンロードします。
2. Blender の `編集 > プリファレンス > アドオン` で、`ディスクからインストール` を選び、ZIP を展開せずに指定します。
3. アドオン一覧で **ToPu_Resample** を有効にします。

**更新するとき**は、古いバージョンを無効にし、**Blender を再起動してから** 新しい ZIP を入れてください。再起動しないと、古いプログラムが読み込まれたまま残ることがあります。

従来形式（`bl_info`）のアドオンです。別のアドオン（Loop Resample など）を入れる必要はありません。

### 起動方法

メッシュの編集モードで辺を選択し、次のどれかで起動します。

| 方法 | リサンプル | スムーズ |
| --- | --- | --- |
| 右クリックメニュー | **ToPu Tools > Resample** | **ToPu Tools > Smooth** |
| ショートカット（初期設定） | **Ctrl＋Alt＋R** | **Alt＋C** |

ショートカットはアドオンプリファレンスで変更・オフにできます。
ToPu_PathExtrude や ToPu_ViewSmooth など、ほかの ToPu アドオンと一緒に使う場合も、右クリックメニューの「ToPu Tools」は1つにまとまります。

---

### リサンプル

1. 辺選択モードで、辺列または辺ループを選択します（複数可）。
2. リサンプルを起動し、ホイールで頂点数を増減します。
3. **左クリック / Enter / Space で確定**、**Esc / 右クリックでキャンセル**（元のメッシュに戻ります）。

#### 操作

| 入力 | 動作 |
| --- | --- |
| ホイール / Q・E | 頂点数を増減 |
| C / HUDの補間ボタン | 補間方式の切り替え・選択 |
| Ctrl＋ホイール / Ctrl＋Q・E | 形状維持（曲率補正）・張力（張力調整）の調整 |
| T | 均等配置のオン・オフ |
| S | サブディビジョンモード（辺の数を2倍・半分単位で増減） |
| R | 初期状態に戻す |
| HUDの値をドラッグ / 値の上でホイール | その値を変更（ドラッグ中のShiftで微調整） |
| Ctrl＋Z / Ctrl＋Shift＋Z | ツール内の元に戻す / やり直す |
| H | HUDの最小化・再表示 |
| 左クリック（HUDの外） / Enter / Space | 確定（Alt＋左クリックは視点操作） |
| Esc / 右クリック | キャンセル |
| 3Dマウス / 中ボタン / Ctrl＋中ボタンドラッグ / テンキー | 視点操作 |

実行中はホイールを頂点数の変更に使うため、ホイールでのズームはできません。ズームは Ctrl＋中ボタンドラッグ、テンキーの＋／−、3Dマウスで行えます。
実行中は選択の変更やほかのメッシュ編集を受け付けません。

#### 補間方式

| 補間方式 | 動き |
| --- | --- |
| なめらか補間 | 元の頂点を通るなめらかな曲線（求心性Catmull–Rom）に沿って配置 |
| 形状キープ | 元の辺（折れ線）の上に配置 |
| 曲率補正（初期設定） | 0%でなめらか補間と同じ曲線。プラスで折れ線に近づき、+100%で形状キープと同じ。マイナスで元の形をならして丸くする |
| 張力調整 | 0.15でなめらか補間、0.5で形状キープと同じ。間を調整 |

**円に近い閉じたループ**（各頂点が平均半径の±10%以内、ほぼ平面、中心を1周するもの）は、曲率補正0%以下で円形に整えます。円柱のように面でつながった複数のループは、すべてが円に近い場合だけまとめて円形にします。

#### 対象

- 開いた辺列、閉じたループ
- 四角形の面でつながった平行な辺列（円柱の輪など）
- 三角ファン、n-gon の蓋
- つながっていない辺列とループの同時処理

分岐・交差した選択や、周囲の面を安全に対応付けられない形は、メッシュを変更せずに処理を中止します。

#### UVと属性

- UV（複数のUVマップ）、頂点グループのウェイト、頂点色・コーナー色、シーム、シャープ、クリースなどの辺の属性、頂点の属性を引き継ぎます。
- UVの切れ目（アイランドの境界）が通る位置には頂点を固定し、テクスチャが切れ目をまたいで伸びないようにします。面でつながったループどうしでは、この固定位置を共通にします。
- 面のUVの大きさの1%未満しか離れていない「髪の毛ほどのすき間」は、切れ目として扱いません。
- 頂点が元の辺から外れて動いた場合も、元の面のUVの伸び方から位置に合わせてUVを補正します。
- 新しい形にUVの境界を載せられない場合は、確定時にお知らせします。

---

### スムーズ

ToPu_ViewSmooth と同じ考え方の、2つの値だけで調整するスムーズです。毎回、アドオン設定の初期値（スムーズ量65%・カーブ量100%）から始まります。

| 設定 | 初期値 | 効果 |
| --- | --- | --- |
| スムーズ量 | 65% | プラスでなめらかに、0%で元の形、マイナスで曲がりや凹凸を強調（−1000%〜1000%） |
| カーブ量 | 100% | 開いた辺列は0%で両端を結ぶ直線、閉じたループは0%で円。マイナスで膨らむ向きが反転（−1000%〜1000%） |
| ウェイト | 距離 | 距離：頂点の間隔を保つ／均等：強くかけるほど頂点の間隔が揃う |

計算は反復ではなく、なめらかな曲線を1回求める方式なので、値を大きくしても崩れません。開いた辺列の両端は動きません。閉じたループは「サイズ維持」で、縮んだ分を元の大きさに戻します。

#### 操作

| 入力 | 動作 |
| --- | --- |
| マウスを左右に動かす | スムーズ量（320ピクセル×UI倍率で100%。Shiftで微調整） |
| ホイール / Q・E | カーブ量（1段で10%。Shiftで1%） |
| T / HUDのウェイトボタン | 距離／均等の切り替え |
| V | サイズ維持（閉じたループ選択時） |
| B | 境界固定（面を選択したとき） |
| R | 初期値に戻す |
| Ctrl＋Z / Ctrl＋Shift＋Z | ツール内の元に戻す / やり直す |
| H | HUDの最小化・再表示 |
| 左クリック（HUDの外） / Enter / Space | 確定 |
| Esc / 右クリック | キャンセル |

確定後は F9 で値を調整し直せます。分岐した選択や面の範囲では、スムーズ量に応じた回数のラプラシアンスムーズで処理します（カーブ量は対象外）。

---

### アドオンプリファレンス

| 設定 | 内容 |
| --- | --- |
| 言語 | 自動（Blenderの表示言語に合わせる）／日本語／英語 |
| ショートカット | リサンプル・スムーズの起動キーの変更、オン・オフ |
| 実行後に法線をリセット | 確定時に法線をリセット（カスタム法線を使うメッシュでは注意） |
| スムーズの初期値 | スムーズ量・カーブ量の開始値 |

ショートカットの変更を残すには、プリファレンスを保存してください。

### 制限事項

- シェイプキーのあるメッシュ、スキンモディファイアの頂点データ、複数オブジェクトの同時編集には対応していません。開始前に止まり、メッシュは変更しません。
- 頂点数が変わるため、整数・真偽値などの属性や境界の位置は、常に完全に一致するわけではありません。
- 作り直した面のカスタム法線は、厳密には保持されません。
- すべてのメッシュ・モディファイアの組み合わせで検証済みというわけではありません。

### ライセンスとクレジット

- **ToPu_Resample**：ToPu4211 / ToPu contributors
- 元になったUIとスムーズ：**AV: ResampleMesh v1.1.9**（Anton Vashkevich, 2024）
- リサンプルの計算部分：**[Loop Resample v1.0.1](https://github.com/YLIANG021/Loop-Resample)**（YLIANG）を統合・修正
- HUDのデザイン：ToPu_PathExtrude をベース

ライセンスは **GPL-3.0-or-later** です。全文は [LICENSE](LICENSE) を参照してください。
元になった作品の作者による公式版・公認版ではありません。
変更履歴は [CHANGELOG.md](CHANGELOG.md)、ソースの構成は [SOURCE_COMPOSITION.md](SOURCE_COMPOSITION.md)、検証内容は [VALIDATION.md](VALIDATION.md) を参照してください。

---

## English

### Features

- **Resample**: change the vertex count of selected edge chains and closed loops with the mouse wheel.
  - Vertices follow a smooth curve; Keep Shape, Curvature Correction and Even Spacing are available.
  - Stacked loops joined by faces (e.g. cylinder rings) stay aligned, so the rungs never twist.
  - Curvature Correction rounds near-circular closed loops into circles.
  - UVs, vertex groups, colors, seams, sharp and crease are carried over. UVs are corrected when vertices move off the original edges, so textures stay in place.
- **Smooth**: two values only — Smoothness (±%) and Curve Amount (±%). Negative smoothness emphasizes bends.
- A HUD at the bottom of the view; click buttons or drag values directly.
- 3D mouse, middle mouse and numpad navigation work during the tools.
- Japanese / English UI.

### Requirements

Blender 4.2 or later (tested on 4.2, 5.0 and 5.1), Mesh Edit Mode.

### Installation

1. Download the ZIP from [Releases](https://github.com/http4211/ToPu_Resample/releases).
2. In `Edit > Preferences > Add-ons`, choose `Install from Disk` and select the ZIP (do not extract it).
3. Enable **ToPu_Resample**.

When updating, disable the old version and **restart Blender** before installing the new ZIP.

### Usage

Select edges in Edit Mode, then run:

| | Resample | Smooth |
| --- | --- | --- |
| Context menu | **ToPu Tools > Resample** | **ToPu Tools > Smooth** |
| Default shortcut | **Ctrl+Alt+R** | **Alt+C** |

**Resample**: Wheel / Q,E = vertex count · C = interpolation · Ctrl+Wheel = shape preserve / tension · T = even spacing · S = subdivision mode · R = reset · H = hide HUD · Ctrl+Z / Ctrl+Shift+Z = in-tool undo / redo · Left click / Enter / Space = confirm · Esc / Right click = cancel.

**Smooth**: Mouse left/right = smoothness · Wheel / Q,E = curve amount · T = weights · V = preserve size · B = lock boundary · R = reset · Left click / Enter / Space = confirm · Esc / Right click = cancel. Re-adjust with F9 after confirming.

Shortcuts, language, normal reset and Smooth's initial values are set in the add-on preferences.

### Limitations

Meshes with shape keys, Skin vertex data and multi-object editing are not supported (the tool stops before changing anything). Custom normals on rebuilt faces are not preserved exactly.

### License and credits

GPL-3.0-or-later. Based on **AV: ResampleMesh v1.1.9** (Anton Vashkevich) and **[Loop Resample v1.0.1](https://github.com/YLIANG021/Loop-Resample)** (YLIANG); HUD design based on ToPu_PathExtrude. Not an official or endorsed version by the original authors. See [CHANGELOG.md](CHANGELOG.md) for changes.
