# Technodelic Visual Lab

既存の音響生成スタジオ [`neural_mantra_studio_v6.html`](../neural_mantra/neural_mantra_studio_v6.html) および音マントラ音響工学（[`SRC_252_neural_mantra_acoustic_reconstruction.md`](../research_corpus/SRC_252_neural_mantra_acoustic_reconstruction.md)）の設計思想をベースに、**Three.js による連続的ビジュアル変調**を統合した実験的 Web アプリケーション設計プロジェクトです。

## コンセプト
一般的な音楽ビジュアライザーのように「音量に合わせて激しく明滅させる」のではなく、`Neural Mantra` シリーズで制作された楽曲（言語野バイパス・3オクターブピラミッド周波数設計・真空断）と連動して、知覚の時間・空間・身体感覚（収束と拡散、低ゲイン状態における変性意識的変調）がどう変化するかを安全に探索・記録します。

1. **Neural Mantra 音響工学との直結**：`35〜85Hz`（体幹・仙骨サブベース）、`200〜500Hz`（脱濁バンド）、`6kHz超`（成層圏・空間開放）の帯域構造をそのまま Web Audio API の FFT 解析バンドとして利用する。
2. **反応系（A）と知覚構成系（B）の分離**：情動的な興奮やストロボ刺激に頼らず、音と色彩・空間構造の関係性によって知覚構成がどう変調するかを観察する。
3. **結合度と位相差の探索**：`Audio Coupling`（0.0〜1.0）や `Phase Offset`（同期／固定遅延／緩やかな位相ドリフト／完全独立）を制御し、予測処理の精度重み付け（Precision Weighting）への影響を体験的に比較する。
4. **Safety by Design**：高速ストロボや急激な輝度変化を一切排除し、`Neural Mantra` 特有の「-100dB 完全真空断（無音ドロップ）」に対しても、衝撃的なフラッシュではなく滑らかな収束・静止で応答する。

## ディレクトリ構成
* [`DESIGN.md`](./DESIGN.md) — 詳細設計書（`Neural Mantra Studio v6` との統合アーキテクチャ、ピラミッド周波数連動仕様、Three.js 6モード、パラメータ仕様、安全設計、実験ログスキーマ）

## 初期プロトタイプ（Phase 1）対象モード
1. **MODE 1: `RAINBOW FLOW`**（虹色流体 × `NIRVĀṆA & VELVET` / アンビエント：色彩と形状の独立制御）
2. **MODE 2: `FRACTAL SPACE`**（幾何学空間・無限フラクタル × `GENESIS CONTINUUM` / 反復電子音：低速・位相差探索）
3. **MODE 3: `CONVERGENCE / DIFFUSION`**（GPU粒子群による収束と拡散の同時成立 × サブベースドローン）
