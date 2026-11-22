# Technodelic Visual Lab — システム詳細設計書（Neural Mantra 統合版）

## 1. プロジェクト目的と既存資産（Neural Mantra）との統合方針

**Technodelic Visual Lab** は、リポジトリ内の [`neural_mantra_studio_v6.html`](../neural_mantra/neural_mantra_studio_v6.html) および音マントラ音響工学（[`SRC_252_neural_mantra_acoustic_reconstruction.md`](../research_corpus/SRC_252_neural_mantra_acoustic_reconstruction.md)）を音響側のベースとして設計された、ブラウザベースの知覚・意識実験ラボである。

### Neural Mantra 設計思想からの継承ポイント
1. **3オクターブ・ピラミッド周波数設計への対応**：
   * **Sub-Bass / Body Core（35Hz–85Hz）**：体幹・仙骨に同期する低域 → 粒子の**収束（Convergence）**や場の重力・呼吸的膨張へマッピング。
   * **Mud-Removal Band（200Hz–500Hz）**：言語思考ノイズを避けるために意図的に空けられた帯域 → ここでのエネルギー低下（静けさ）を背景ノイズの低減指標として扱う。
   * **Voice / ASMR Pocket（1kHz–3kHz）**：左右交差音素（Phonetic Crossfire）や囁き帯域 → 空間の左右対称性や微細な位相揺らぎへ接続。
   * **Stratosphere / Crown（6kHz–16kHz / D6）**：ノンビブラート持続音・528Hzボウル倍音 → **虹色スペクトル（Color Flow）**・薄膜の透明感・**拡散（Diffusion）**へマッピング。
2. **-100dB 完全真空断（Dead-Room Vacuum Cut）との連動**：
   * 楽曲中で急激な無音区間が発生した際、画面をフラッシュさせるのではなく、**0.5秒の滑らかな慣性減衰を経て、静かな静止・低ゲイン状態へ移行する**ことで、聴覚的予測符号化のリセットと視覚的静寂を同期させる。
3. **単一HTML＋ダークモードUIの継承**：
   * `neural_mantra_studio_v6.html` と共通のカラーパレット（`--bg: #0d0f12`, `--panel: #161a22`, `--panel-border: #262c36`, `--accent: #00e5ff`, `--accent-secondary: #ff3366`）と操作思想を踏襲し、まずはビルド不要の単一HTML（またはES Modules）で即座に動作・実験できる構成を優先する。

---

## 2. システムアーキテクチャ

```text
TECHNODELIC VISUAL LAB (Integrated with Neural Mantra Audio Architecture)
├── Audio Engine (Web Audio API)
│   ├── Track Loader (Suno生成楽曲 / ローカル音声ファイル / 4大核モデル対応プリセット)
│   ├── FFT Analyzer (AnalyserNode: fftSize = 4096)
│   ├── Pyramid Band Extractor (Neural Mantra 準拠の4帯域分離)
│   │   ├── SubCore (35–85 Hz)
│   │   ├── MudBand (200–500 Hz)
│   │   ├── CrossfirePocket (1–3 kHz, L/R Stereo Balance)
│   │   └── Stratosphere (6–16 kHz)
│   ├── Spectral Features (Spectral Centroid / Spectral Flux / RMS / Silence Detector)
│   └── Envelope Smoother (Attack >= 150ms, Release >= 400ms でフラッシュ防止)
│
├── Modulation Engine (本アプリの中核)
│   ├── Audio Coupling (0.0: 完全独立 〜 1.0: 強連動)
│   ├── Phase Offset & Drift (sync / fixed_offset / slow_drift / independent)
│   ├── Decoupled Channels (Audio -> Color / Audio -> Geometry / Audio -> Motion)
│   └── Autonomous LFO (音と無関係に動く独立周期ジェネレーター)
│
├── Visual Engine (Three.js / WebGL2)
│   ├── [Mode 1] Rainbow Fluid (ShaderMaterial + Curl/Simplex Noise + Hue Field)
│   ├── [Mode 2] Fractal Space (Raymarching SDF + Slow Camera/Torsion Control)
│   ├── [Mode 3] Convergence / Diffusion Particles (GPU Particles / Torus + Micro-Vortices)
│   ├── [Mode 4] Cymatics (波動干渉シェーダー + 帯域別固有モード合成)
│   ├── [Mode 5] Torus Mandala (InstancedMesh + 対称性シェーダー)
│   └── [Mode 6] Iridescent Membrane (Fresnel + Thin-Film Interference Shader)
│
├── Post Processing (Three.js EffectComposer)
│   ├── RenderPass
│   ├── UnrealBloomPass (強度上限を制限し過刺激を防止)
│   ├── ColorGrading / Subtle Chromatic Aberration (初期値 0)
│   └── LuminanceLimiterPass (最大輝度・フレーム間変化率クランプ)
│
├── Experiment Controller (Neural Mantra レシピ連携)
│   ├── Recipe-Linked Presets (NIRVĀṆA & VELVET / GENESIS CONTINUUM / PRIMAL ORIGIN / BĪJA OVERDRIVE)
│   ├── Condition Sequencer (同一楽曲でのA/B/C/D条件比較タイマー)
│   └── Experience Log (セッション後の主観評価記録 & JSON/Markdown出力)
│
└── Safety & UI (Dark Mode: #0d0f12 / #00e5ff)
    ├── Mode Selector / Audio Loader / Parameter Sliders
    ├── Fullscreen Toggle & Auto-Hide HUD
    └── Safety Controls (Calm-Stop / Max Speed Cap / Max Contrast Cap / Photosensitivity Notice)
```

---

## 3. Neural Mantra 4大核モデル連動プリセット設計

既存の `Neural Mantra` 4大核モデル（`SRC_252`）をそのまま実験プリセットとして組み込む。

| プリセット名（連携楽曲） | 推奨ビジュアルモード | 推奨パラメータ設定 | 探索・検証の目的 |
| :--- | :--- | :--- | :--- |
| **① NIRVĀṆA & VELVET**<br>(52 BPM / 42Hz Sine + 528Hz Bowl) | `RAINBOW FLOW` | `audioCoupling: 0.45`<br>`phaseMode: slow_drift`<br>`spatialFlowSpeed: 0.15` | ドラムレスの鎮静音響に対し、虹色色相のみを緩やかに位相ドリフトさせた際の時間・空間感覚の変調を観察。 |
| **② GENESIS CONTINUUM**<br>(112–120 BPM / 20秒持続音 + 真空断) | `FRACTAL SPACE` or<br>`CONVERGENCE / DIFFUSION` | `audioCoupling: 0.60`<br>`phaseMode: fixed_offset`<br>`convergence: 0.65, diffusion: 0.65` | 20秒ゼロブレス持続音と真空消滅における「収束と拡散の同時成立」および無音時の静止移行を検証。 |
| **③ PRIMAL ORIGIN**<br>(138 BPM / 50Hz Sub + 左右音素交差) | `CONVERGENCE / DIFFUSION` or<br>`CYMATICS` | `audioCoupling: 0.50`<br>`symmetry: 8`<br>`spatialFlowSpeed: 0.25` | 左右チャンネルの音素交差（Crossfire）とサブベースによる身体中心軸のまとまり感を視覚と同期させる。 |
| **④ BĪJA OVERDRIVE**<br>(142 BPM / 38Hz Taiko + 55Hz Drone) | `TORUS MANDALA` | `audioCoupling: 0.55`<br>`luminanceCap: 0.75`（厳格制限） | 高密度リズム下でも視覚側が過剰な明滅・高ゲインにならず、幾何学的対称性を維持できるかを確認。 |

---

## 4. パラメータ仕様（Modulation & Visual）

| パラメータ名 | 範囲 | 役割・知覚検証上の意味 |
| :--- | :--- | :--- |
| `audioCoupling` | `0.00` – `1.00` | 音の特徴量が映像変調へ与える影響度の全体係数。`0`で完全独立、`0.5`で緩やかな連動、`1.0`で密接連動。 |
| `phaseMode` | `sync` / `fixed_offset` / `slow_drift` / `independent` | 音と映像の時間関係のモード選択。`slow_drift` は類似周期のまま時間経過で位相がゆっくりズレ続ける実験用モード。 |
| `phaseDriftRate` | `-0.10` – `+0.10` rad/s | `slow_drift` 時の位相ズレ速度。 |
| `colorFlowSpeed` | `0.00` – `1.00` | 虹色・色相スペクトルの変化速度（空間の動きとは独立して `0` に固定可能）。 |
| `spatialFlowSpeed` | `0.00` – `1.00` | 流体・トンネル・粒子の物理的移動／変形速度（色変化とは独立して `0` に固定可能）。 |
| `geometryComplexity` | `0.10` – `1.00` | ノイズのオクターブ数、フラクタル反復回数、干渉波数の制御。 |
| `symmetry` | `1` – `16` | マンダラ・サイマティクス・空間分割の対称次数。 |
| `depthPerception` | `0.00` – `1.00` | 平面的な流体知覚から奥行きのある空間知覚までのバランス。 |
| `convergence` | `0.00` – `1.00` | 粒子・場を中心や局所渦へ引き寄せる力（収束ベクトル：SubCore帯域と連動可）。 |
| `diffusion` | `0.00` – `1.00` | 粒子・場を空間全体へ広げ均一化する力（拡散ベクトル：Stratosphere帯域と連動可）。 |
| `randomness` | `0.00` – `1.00` | 規則的周期に対する揺らぎ（予測可能性の調整）。 |

---

## 5. プロトタイプ対象3モードの詳細仕様

### MODE 1: `RAINBOW FLOW`（虹色流体 × アンビエント）
* **目的**：虹色という色相変化そのものと、流体形状の空間変化のどちらが知覚変調に寄与しているかを切り分ける。
* **実装**：フルスクリーンクワッド上のフラグメントシェーダー（`THREE.ShaderMaterial`）。多層Simplex/Curl Noiseによる速度場と、Oklab/HSV空間での滑らかな連続色相補間を分離実装する。
* **実験条件例**：
  * `colorFlowSpeed > 0, spatialFlowSpeed = 0`（形状静止・色のみ流動）
  * `colorFlowSpeed = 0, spatialFlowSpeed > 0`（色固定・形状のみ流動）

### MODE 2: `FRACTAL SPACE`（幾何学空間 × 反復音・持続音）
* **目的**：視覚と聴覚の時間的一致／不一致が、奥行き感・時間感覚の変調にどう影響するかを検証する。
* **実装**：レイマーチング（Signed Distance Field）による自己相似空間・ねじれ空間。
* **安全配慮**：前進速度はデフォルトで極低速（または停止状態での空間変形のみ）とし、前庭感覚との不一致による乗り物酔いを防ぐ。

### MODE 3: `CONVERGENCE / DIFFUSION`（粒子の収束と拡散の同時成立）
* **目的**：身体感覚モデルにおける「収束と拡散の同時成立」を視覚的・空間的に表現し、低音ドローン等との共鳴を探索する。
* **実装**：`THREE.Points` + カスタムシェーダー（または `GPUComputationRenderer`）による1万〜10万粒子シミュレーション。
* **構造**：全体としては大きなトーラス構造を描きながら空間に広がり（**拡散**）、同時にトーラス断面の局所領域では無数の小さな渦や密度核（**収束**）が生成・維持される。

---

## 6. 安全設計仕様（Safety by Design）

視覚刺激による体調不良や光過敏性リスクを最小化するため、以下の制約をエンジンレベルで強制する。

1. **ストロボ・明滅の禁止**：
   * 急激な全画面輝度変化（2Hz以上の高コントラスト明滅）を生むシェーダーロジックを禁止。
   * 音響エンベロープには必ずローパス平滑化（Attack 150ms以上 / Release 400ms以上）を適用し、トランジェントや真空断の前後で画面が瞬時にフラッシュしないようにする。
2. **輝度・コントラストリミッター**：
   * ポストプロセッシング最終段で最大輝度（リニア空間 `0.85` 上限）とフレーム間の最大輝度変化率をクランプする。
3. **Calm-Stop（即時穏やか停止）**：
   * `Space` または `Esc` キー、および画面隅の固定ボタンで、暗転ではなく**0.5秒かけて滑らかに動きと音を減衰させ、穏やかな静止画・無音状態へ移行**する。
4. **起動時確認**：
   * 初回起動時に光過敏性・めまい・頭痛等の既往がある場合は使用を控える旨の簡潔な安全案内を表示する。

---

## 7. 実験プロトコルと体験ログスキーマ

`neural_mantra_studio_v6.html` と同様の自己観察・比較を前提とし、セッション終了後に以下の構造化データを記録・エクスポート（JSON / Markdown）できるようにする。

```json
{
  "sessionId": "2026-11-20T18-30-00",
  "audioTrack": "nirvana_velvet_52bpm.mp3",
  "neuralMantraModel": "NIRVANA_AND_VELVET",
  "durationSec": 300,
  "visualMode": "RAINBOW_FLOW",
  "parameters": {
    "audioCoupling": 0.45,
    "phaseMode": "slow_drift",
    "phaseDriftRate": 0.02,
    "colorFlowSpeed": 0.35,
    "spatialFlowSpeed": 0.15
  },
  "subjectiveRatings": {
    "alteredStateShift": 4,
    "pleasantness": 5,
    "wakefulnessClarity": 5,
    "spatialDistortion": 4,
    "bodyBoundarySoftening": 3,
    "physicalDiscomfortOrDizziness": 0
  },
  "notes": "形状はほぼ静止させ、色相のみゆっくり位相ドリフトさせた条件で時間感覚の変化が最も明瞭だった。"
}
```

---

## 8. 開発ロードマップ

* **Phase 1（MVP・単一HTMLファイル `technodelic_visual_lab.html` として実装）**
  * `neural_mantra_studio_v6.html` と統一されたダークモードUI（`#0d0f12` / `#00e5ff`）
  * Web Audio API FFT解析パイプライン（3オクターブ・ピラミッド4帯域抽出 + 平滑化エンベロープ）
  * `MODE 1: RAINBOW FLOW`、`MODE 2: FRACTAL SPACE`、`MODE 3: CONVERGENCE / DIFFUSION` の実装
  * `Audio Coupling` / `Phase Mode (Slow Drift)` / `Color vs Spatial Flow` のリアルタイム制御UI
  * 安全リミッター（Luminance Limiter + Calm-Stop）と体験ログJSONエクスポート
* **Phase 2（拡張モード・Neural Mantra 連携強化）**
  * `MODE 4: Cymatics`、`MODE 5: Torus Mandala`、`MODE 6: Iridescent Membrane` の追加
  * 5分間A/B比較用のタイムライン・プリセット・オートメーション機能
* **Phase 3（マルチデバイス・WebXR対応）**
  * モバイルブラウザ最適化およびWebXRでの没入検証（フレームレート維持・低速制御を徹底）
