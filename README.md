# Typingsoldier2 — タイピング神話討伐（召喚レイド）

**ことばを打つと兵士が召喚され、自動で戦う。** 軍勢を増やして「今日の神格」を討て。
討伐したら今日はクリア——また明日、新たな神格が現れる。

🔗 **公開URL**: https://play.bubblegumgameboy.com/Typingsoldier2/

## 遊び方

```sh
python3 -m http.server 8000   # ローカルは http://localhost:8000
```

- なまえを入れて「挑戦する」（Enterでも開始）
- ひらがなをローマ字で打ちきると**兵士が1体召喚**される（IME不要・変換中でも打てる）
- **長いことばほど強い兵士**（2文字=村人 → 5文字=竜・天使）。兵士は自動で攻撃し続ける
- ボスHPを削りきったら **討伐！** スコア＝**打鍵速度×正確率×ボス強度**（どのボスの日でも公平）
- 討伐した日はロック。翌日（JST 0時）に新しいボスと新しいお題が来る
- `?debug` を付けると低HP・記録なしの練習モード／Escで中断

設計の詳細（バランスノブ・データ構造・意図）は **[DESIGN.md](DESIGN.md)** を参照。

## ファイル構成

| パス | 役割 |
|------|------|
| `index.html` | **新ゲーム本体**（タイピング神話討伐。1ファイル完結） |
| `typing-engine.js` | ローマ字入力エンジン（かな→ローマ字判定・IME回避。旧作から流用） |
| `cthulhu-assets/` | クトゥルフ素材（ボス43体webp・ユニットpng・背景`bg.png`・`slash.mp3`） |
| `DESIGN.md` | ゲーム設計書（コアループ／バランス／Firestore構造） |
| `soldier-typing.html` | 旧作「ソルジャータイピング」（参考用にそのまま残置） |
| `images/` `audio/` | 旧作の画像・BGM（旧作用） |
| `ranking.html` | 旧作のランキング画面 |

## 素材の出どころ

### ① ゲーム基盤（[Bubble-Wars](https://github.com/BubblegumGameBoy/Bubble-Wars) から）

| パス | 役割 |
|------|------|
| `soldier-typing.html` | 旧ゲーム本体（HTML/CSS/JSすべて内包、約2700行）。 |
| `typing-engine.js` | ローマ字入力エンジン（新ゲームでも使用）。 |
| `images/` | 旧作のボス/ユニット画像。 |
| `audio/` | Wave別BGM・ボス戦BGM。 |
| `ranking.html` | 旧作スコアランキング画面。 |
| `robots.txt` | クローラ設定。 |

**旧ゲーム構造の要点（`soldier-typing.html` 内）:**

- 単語をローマ字入力 → 味方ユニット召喚 → 自動で敵城へ進軍して戦う
- 全5 Wave、各Waveにボス。城壁到達で敵城HPを削り、ボス撃破 or 敵城破壊でWaveクリア
- ボス定義: `const BOSS_DEFS`（アメーバ→魔猫→大蝙蝠→魔王→神）
- 味方/敵ユニット: `makeUnit()` / スプライトは `SPRITE_*`（ドット絵配列）と `IMAGE_DEFS`（PNG画像）の2系統
- お城: `drawPlayerCastle()` / `drawEnemyCastle()`（canvasのfillRectで手描き）
- エフェクト: `gs.effects`（`explosion / shockwave / ember / blob / slash / spawn / msg` など）

### ② クトゥルフ素材（[spacebar-clicker](https://github.com/BubblegumGameBoy/spacebar-clicker) から）

`cthulhu-assets/` に格納。新ゲームで使用中。

| 種別 | ファイル | 用途 |
|------|----------|------|
| ボス（webp 43体） | `boss_cthulhu` / `boss_nyarlathotep` / `boss_hastur` ほか + 番号付き `boss01_rat`〜`boss11_*` | **日替わりボス**（43日でロスター1周） |
| 味方ユニット（png 17種） | `unit_knight` / `paladin` / `mage` ほか | 未使用（将来の物量モード用に温存） |
| 背景 | `bg.png` | 戦場の背景として使用中 |
| SE | `slash.mp3` | 単語完成の斬撃音として使用中 |

## 今後の候補

- 物量モード（レイド形式を検討・DESIGN.md参照）
- Firebase匿名認証によるロック強化
- モバイル入力対応
