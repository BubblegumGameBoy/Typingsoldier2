# Typingsoldier2

日本語タイピング × リアルタイム攻城ゲーム **「ソルジャータイピング」** をベースに、
お城・ボス・エフェクトをクトゥルフ神話テイストへ刷新していくプロジェクト。

## 現状（素材取り込み済み）

このリポジトリには2つのソースから素材を取り込んでいる。

### ① ゲーム本体（[Bubble-Wars](https://github.com/BubblegumGameBoy/Bubble-Wars) から）

| パス | 役割 |
|------|------|
| `index.html` | ゲーム本体（HTML/CSS/JSすべて内包、約2700行）。これ1つで動く。 |
| `typing-engine.js` | ローマ字入力エンジン（かな→ローマ字変換テーブル等）。 |
| `images/` | 現行のボス/ユニット画像（`*_boss.png`, `soldier_*`, `goblin_*`, `hero_*`, `lancer_*` など）。 |
| `audio/` | Wave別BGM・ボス戦BGM。 |
| `ranking.html` | スコアランキング画面。 |
| `robots.txt` | クローラ設定。 |

**ゲーム構造の要点（`index.html` 内）:**

- 単語をローマ字入力 → 味方ユニット召喚 → 自動で敵城へ進軍して戦う
- 全5 Wave、各Waveにボス。城壁到達で敵城HPを削り、ボス撃破 or 敵城破壊でWaveクリア
- ボス定義: `const BOSS_DEFS`（アメーバ→魔猫→大蝙蝠→魔王→神）
- 味方/敵ユニット: `makeUnit()` / スプライトは `SPRITE_*`（ドット絵配列）と `IMAGE_DEFS`（PNG画像）の2系統
- お城: `drawPlayerCastle()` / `drawEnemyCastle()`（canvasのfillRectで手描き）
- エフェクト: `gs.effects`（`explosion / shockwave / ember / blob / slash / spawn / msg` など）

### ② クトゥルフ素材（[spacebar-clicker](https://github.com/BubblegumGameBoy/spacebar-clicker) から）

刷新に使う原素材を `cthulhu-assets/` にまとめている。

| 種別 | ファイル | 用途（予定） |
|------|----------|--------------|
| ボス（webp 43体） | `boss_cthulhu` / `boss_nyarlathotep` / `boss_hastur` / `boss_tsathoggua` / `boss_shantak2` / `boss_migo` / `boss_deepone` / `boss_ghoul` / `boss_ghast` / `boss_gug` / `boss_nightgaunt` / `boss_moonbeast` / `boss_tindalos` / `boss_blackgoat` / `boss_glaaki` / `boss_chaugnar` / `boss_cthugha` … + 番号付き `boss01_rat`〜`boss11_*` | 各Waveのボスをクトゥルフ神格へ差し替え |
| 味方ユニット（png 17種） | `unit_knight` / `paladin` / `mage` / `warlock` / `angel` / `dark` / `elf` / `dwarfb·w` / `dragonr·g·p` / `farmer` / `villager` / `heavy` / `sword` / `arch` | 召喚ユニットを画像ユニットへ差し替え |
| 背景 | `bg.png` | お城・背景の刷新 |
| SE | `slash.mp3` | 斬撃音 |

## 刷新方針（次ステップ）

- 味方ユニットを `unit_*` 画像に差し替え
- 各Waveのボスをクトゥルフ神格（webp）に差し替え
- お城・背景を `bg.png` 等で刷新
- 撃破演出・必殺技などエフェクトを強化

## 動作確認（ローカル）

```sh
python3 -m http.server 8000   # http://localhost:8000
```
