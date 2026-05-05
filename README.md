# CNC CAM — DXF to G-code Converter

**ブラウザで動くDXF→Gコード変換CAMツール。インストール不要。**

🔗 **[ツールを使う](https://freezingdragon.github.io/CNC_web-CAM/cnc-cam.html)**

---

## 概要

Genmitsu 3018Pro（GRBL）向けに最適化された軽量CAMツールです。一般的な3018系の中華CNCをはじめ、GRBLで動くCNCに対応しています。
DXFファイルをブラウザにドロップするだけで切削用Gコードを生成します。
サーバー不要、インストール不要、HTMLファイル1枚で完結します。

## 特長

- **DXF直接読み込み** — VectorWorks・Illustrator・Affinity対応
- **SPLINE対応** — De Boorアルゴリズムによる高精度なNURBS直線近似
- **チェーン最適化** — 連続した線分を自動結合してZプランジを最小化（加工時間を最大70%短縮）
- **G2円弧出力** — CIRCLE エンティティを円弧Gコードに変換
- **ワーク配置オフセット** — 図形全体を任意の位置にシフト
- **バウンディングボックストレース** — 加工前の範囲確認用Gコードを別途生成します
- **2.5D対応** — 彫り込み深さの異なる複数のレイヤーを設定可能。いわゆる2.5D（2D多段切削）に対応します
- **タブ設定** — 任意のレイヤーにタブを設定できます。タブ付き外形切り抜き加工が可能
- **可動域バリデーション** — 設定した機械サイズを超える場合にエラー表示

## 対応DXFエンティティ

| エンティティ | 対応 | 備考 |
|---|---|---|
| LINE | ✅ | |
| LWPOLYLINE | ✅ | 頂点列を自動分解 |
| SPLINE（degree-3 NURBS） | ✅ | De Boorアルゴリズムで近似 |
| CIRCLE | ✅ | G2円弧として出力 |

## 対応DXFソフト

| ソフト | 備考 |
|---|---|
| VectorWorks | 直接書き出し可（Illustrator経由不要） |
| Illustrator | $INSUNITSインチ問題を自動補正（スケール25.4） |
| Affinity Designer / Publisher | CR単独行末・UTF-8に対応 |

## 使い方

1. **[ツールを開く](https://freezingdragon.github.io/CNC_web-CAM/cnc-cam.html)**
2. DXFファイルをドロップゾーンにドラッグ＆ドロップ
3. レイヤー名・切削条件を設定
4. **◈ 解析** ボタンでDXFを読み込み
5. **⬜ トレース** で加工範囲確認用Gコードを生成
6. **⚙ Gコード生成** で切削用Gコードを生成
7. `.nc`ファイルをダウンロードしてCNCjsやCandleで実行

## DXFレイヤー設定

| レイヤー | 用途 | 図形 |
|---|---|---|
| 加工レイヤー（デフォルト：`L1`） | 切削パス | LINE / LWPOLYLINE / SPLINE / CIRCLE |
| 原点レイヤー（デフォルト：`ORIGEN`） | ワーク原点定義 | CIRCLE（1個のみ）またはSPLINE |

## 生成されるGコード

```gcode
G21        ; mmモード
G90        ; 絶対座標
M3 S10000  ; スピンドル起動
G4 P2.0    ; ウォームアップ
G0 Z5.0    ; 安全高さへ
; --- 深度反復ループ ---
G0 Z5.0
G0 X-10.0 Y-20.0
G1 Z-0.5 F150
G1 X-30.0 Y-20.0 F500
; --- 終了 ---
M5         ; スピンドル停止（最優先）
G0 Z30.0   ; 終了退避
G0 X0 Y0
M2
```

## パラメータ

| パラメータ | デフォルト | 説明 |
|---|---|---|
| 総深さ | 2.0mm | 最終切削深さ |
| 切込み/パス | 0.5mm | 1パスの切り込み量 |
| 水平送り | 500mm/min | XY方向の送り速度 |
| Z進入送り | 150mm/min | プランジ速度 |
| 移動退避高さ | 5.0mm | 線間の退避高さ |
| 終了退避高さ | 30.0mm | 加工完了時の退避高さ |
| スピンドルS値 | 10000 | 回転数（$30基準） |
| SPLINE近似誤差 | 0.05mm | 曲線の直線近似精度 |
| X/Yオフセット | 0mm | ワーク配置オフセット |
| スケール補正 | 1.0 | Illustratorインチ問題に1/25.4を自動検出 |

## 動作環境

- Chrome / Edge（推奨）
- Firefox / Safari（動作しますが一部機能に制限あり）
- サーバー不要、オフライン動作可

## 対象機械

- Genmitsu 3018Pro / 3040
- GRBLコントローラを搭載したCNCルーター全般
- 機械原点：右奥（X+/Y+ リミットスイッチ）

## 関連記事

[CNCをブラウザで動かす：ncvc＆Candleからの完全脱却記]（[note記事URL](https://note.com/freezing_dragon/n/n458ccc960970?app_launch=false)）

## ライセンス

MIT License

---

*Made by [@FreezingDragon](https://github.com/FreezingDragon)*
