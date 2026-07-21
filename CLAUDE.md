# Voxel Asteroid Miner

このリポジトリは **Voxel Asteroid Miner（WebGL2/Three.js 宇宙採掘ゲーム）** です。

✅ 宇宙船・小惑星・採掘レーザー関連の変更はここに行ってください。  
⚠️ `mu-lab-zzz/Atomic-Factory`（`/home/user/Atomic-Factory/`）とは **別リポジトリ・別ゲーム** です。混同しないでください。

## ファイル構成
- `index.html` — ゲーム本体（Three.js r128、単一ファイル）

## 技術メモ
- Three.js r128 / WebGL2
- 小惑星: `SphereGeometry.toNonIndexed()` + ノイズ変位 + 頂点カラー（鉱石種別）
- 採掘: `mine()` で頂点を内側にへこませてクレーター形成
- 衝突: 船と小惑星の球同士コリジョン
- 操作: yaw/pitch を独立した浮動小数点角度で管理（ロールドリフト防止）
- 設定: `pitchInvert: true` がデフォルト
