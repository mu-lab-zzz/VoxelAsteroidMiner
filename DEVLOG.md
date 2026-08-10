# Voxel Asteroid Miner — 開発ログ

## セッション概要（2026-08-10）

---

## 1. UI修正：地平線リセットボタン

- バーチャルジョイスティックと被っていたリセットボタンを**右上**に移動
- サイズを小さく調整し、操作の干渉を解消

---

## 2. メーカーシステムの設計・実装

### 追加メーカー

| メーカー | コンセプト |
|----------|-----------|
| **Spark Space** | 入門向け。鉄と銅だけで製造可能な低コスト品 |
| **Spicy X** | 高性能・高コスト。各スペックの尖った特化型 |
| **Blue Orange** | バランス重視。コスパ良好な中堅ブランド |
| **Rocket Love** | 機動性特化。旋回補助（turnBonus）を持つユニーク設計 |

### 新規ステータス

| ステータス | 説明 |
|-----------|------|
| `turnBonus` | 旋回速度補正（ベース回転率 2.0 に加算） |
| `maxHp` | 船体耐久値（ベース100、船体ブロックで加算） |

### バグ修正

- `boost` スタットがブロックから集計されていなかった問題を修正
  （`_computeShipStats()` のループに `s.boost += b.stats.boost` を追加）

---

## 3. 標準パーツの廃止とメーカー代替

### 削除したパーツ

**Tier 1（Spark Spaceが代替）**
`laser_0` / `engine_1` / `hull_1` / `cargo_s` / `reactor_s` / `solar_s` / `thruster_s`

**Tier 2（メーカー品が代替）**
`engine_2` / `laser_1` / `cargo_m` / `reactor_m` / `thruster_m`

**Tier 4**
`warp_core`（メーカー製ワープが代替）

---

## 4. メーカー別パーツ一覧

### コクピット（各社 Tier 1 / 2 / 3）

| Tier | Spark Space 🔵 | Spicy X 🔴 | Blue Orange 🟠 | Rocket Love 💜 |
|------|--------------|-----------|--------------|--------------|
| 1 | SS-Cp1 | SX-Pilot | Open | Cozy |
| 2 | SS-Cp2 | SX-Suite | View | Slim |
| 3 | SS-Cp3 | SX-Core | Panorama | Micro |

- 性能は旧標準コクピット（Mk1/2/3）と同一。見た目・ブランドのみ差別化

### エンジン

| Tier | Spark Space | Spicy X | Blue Orange | Rocket Love |
|------|-------------|---------|-------------|-------------|
| 1 | SS-1（推力8 加速+0.1） | — | — | — |
| 2 | SS-2（推力14 加速+0.2） | SX-7（加速+0.6 高消費） | Horizon（バランス） | Nimble（旋回+1.0） |
| 3 | — | SX-14（加速+1.2 最大消費） | Horizon X（加速+0.8） | Nimble+（旋回+2.5） |

### レーザー

| Tier | Spark Space | Spicy X | Blue Orange | Rocket Love |
|------|-------------|---------|-------------|-------------|
| 1 | SS-L1（採掘0.3） | — | — | — |
| 2 | SS-L2（採掘0.5） | SX-Drill（1.0） | Cutter（0.8） | Precise（0.5） |
| 3 | — | SX-Drill+（1.8） | Cutter+（1.4） | Precise+（0.9） |

### 船体（耐久値 maxHp）

| Tier | Spark Space | Spicy X | Blue Orange | Rocket Love |
|------|-------------|---------|-------------|-------------|
| 1 | SS-H1（+30） | — | — | — |
| 2 | SS-H2（+70） | SX-Armor（+200） | Solid（+100） | Lite（+50） |
| 3 | — | SX-FortArmor（+400） | Solid X（+250） | Lite+（+120） |

### 貨物

| Tier | Spark Space | Spicy X | Blue Orange | Rocket Love |
|------|-------------|---------|-------------|-------------|
| 1 | SS-C1（70） | — | — | — |
| 2 | SS-C2（120） | SX-C5（150） | Vault（100） | Compact（75） |
| 3 | — | SX-C10（300） | Vault X（200） | Compact+（150） |

### リアクター

| Tier | Spark Space | Spicy X | Blue Orange | Rocket Love |
|------|-------------|---------|-------------|-------------|
| 1 | SS-R1（100） | — | — | — |
| 2 | SS-R2（200） | SX-R9（300） | PowerCell（200） | LightCell（120） |
| 3 | — | SX-R18（600） | PowerCell X（400） | LightCell+（250） |

### ソーラーパネル

| Tier | Spark Space | Spicy X | Blue Orange | Rocket Love |
|------|-------------|---------|-------------|-------------|
| 1 | SS-S1（+15） | — | — | — |
| 2 | SS-S2（+30） | SX-Sol（+50） | SunDisk（+30） | Feather（+20） |
| 3 | — | SX-Sol+（+100） | SunDisk+（+70） | Feather+（+40） |

### スラスター

| Tier | Spark Space | Spicy X | Blue Orange | Rocket Love |
|------|-------------|---------|-------------|-------------|
| 1 | SS-T1（8） | — | — | — |
| 2 | SS-T2（14） | SX-T3（16） | Glider（12） | Lateral（8 旋回+0.8） |
| 3 | — | SX-T6（30） | Glider+（24） | Lateral+（16 旋回+1.5） |

### ワープコア（既存、段階済み）

| Tier | Spark Space | Rocket Love | Blue Orange | Spicy X |
|------|-------------|-------------|-------------|---------|
| 2 | SS-W1（1000） | — | — | — |
| 3 | — | Jumper（1800） | — | — |
| 4 | — | — | Horizon W（2500） | — |
| 5 | — | — | — | SX-Warp（5000） |

---

## 5. ワークショップ UI 改修

- ブロック製造一覧をメーカー別タブに変更
- タブ：**⚡ Spark Space** / **🔥 Spicy X** / **🔷 Blue Orange** / **💙 Rocket Love** / **🔧 一般**
- `getMaker(bid)` 関数でIDプレフィックスから自動判定
- タブ選択状態はモーダルを開いている間持続

---

## 6. 初期配置

新規ゲーム開始時の船：

```
[空] [空] [空      ] [空] [空]
[空] [空] [SS-Cp1 🔵] [空] [空]
[空] [空] [SS-L1  ⚡] [空] [空]
[空] [空] [SS-1   🔥] [空] [空]
[空] [空] [空      ] [空] [空]
```

Spark Space Tier 1 のコクピット・レーザー・エンジンを各1個装備した状態でスタート。

---

## 技術メモ

- **対象ファイル**：`/workspace/voxelasteroidminer/index.html`（Three.js r128 単一ファイル構成）
- **ブランチ**：`main-update → main`
- `SHIP_BLOCKS` オブジェクトにすべてのパーツを定義
- `_computeShipStats()` でブロックを集計してシップステータスを算出
- `warpSpeed` のみ合計ではなく最大値（`Math.max`）を採用
