# 羽球模擬器 · 羽球戰術工作室

Static, dependency-free application served at `/AI/Badminton-Sandbox/` on GitHub Pages. No backend: HTML, CSS and ES modules only, with the session kept in the browser's localStorage. `package.json` and `tests/` are only for local Node tests and are not needed at runtime.

## Preview

From the repository root:

```powershell
python -m http.server 8765 --bind 127.0.0.1
```

Open `http://127.0.0.1:8765/AI/Badminton-Sandbox/`. Serve over HTTP rather than opening `index.html` directly, because the app uses JavaScript modules.

## Experience

- Mobile play keeps the court in sight, recommends three shots by playing style, offers all shots on demand, and provides a fixed skill/play/pause toolbar. During animation, the court expands and the tactical controls are hidden until the shot finishes.
- Court Settings offers Arcade (default) and Tactical pacing. Arcade lowers errors, rewards power shots and skills, and lets successful attacks force a weak return that puts the receiver under pressure for the next shot.
- Ability budgets cap at 40 points, so high-level players retain distinct strengths instead of automatically maxing every stat. Old all-10 allocations migrate to the player's style preset.
- Two optional specialties (heavy smash, chasing, deception, rescue, steadiness) each exchange one stat point for another. Their changes feed the radar, shot speed and match model; they also travel with shared player cards.
- The skill workshop combines an existing signature move with speed, deception or control, a 100-momentum charge or a risky 70-momentum trigger, and four effect colors. A small trial canvas previews the player's shot speed and color without changing the match. Passive Wall supports a name and color; active bonuses and risky costs are restricted to active skills. Designs survive saved sessions and player-card sharing.

- A skippable first-visit shuttle entrance and a three-step introduction. Help can replay it. System reduced-motion settings are respected; Court Settings also offers a simplified mode.
- Sixteen original character presets in the existing ROSTER. Team Setup offers four team slots, a mode-filtered character grid, desktop detail drawer and mobile bottom sheet. Tap to assign or swap, drag characters or slots, and remove characters before filling the lineup. New assignments and gender replacements apply the complete preset; moving a character keeps its customized profile. Player Studio remains the single editor.
- Four editable cartoon players: name, level 1–18 (default 6) tiered after the Taiwan Badminton Promotion Association scale; level drives per-shot error, shuttle speed (km/h and animation flight time) and outright-winner odds, personality, playing style, face shape, hairstyle, skin tone and accessory.
- Men's / women's / mixed doubles; 21-point or 11-point games; single game or best-of-three.
- Ability radar: five stats (power, speed, net, defense, control) plus a gold sixth "skill" axis. Level sets the point budget; style presets fill it and can be hand-tuned. Stats are read relative to the player's own average, so they shape strengths and weaknesses while level stays the overall strength.
- Rackets: ten well-known models plus a standard practice racket. Each racket's strengths and weaknesses come from published specs (balance, stiffness, shaft), become radar modifiers that sum to about zero, and its best-known colourway is drawn on the player's racket. The radar shows the base shape dashed under the racket-adjusted one.
- When the lineup, format or scoring changes mid-match, the setup and court pages offer to keep the score or restart with the new lineup and arrange the first server and receiver.
- Signature skills (雷霆重殺, 網前魔術, 平抽風暴, 鷹眼, 鐵壁) with optional custom names, charged by a per-player momentum meter. Active skills are triggered in manual mode or chosen by the player's personality in automatic play; 鐵壁 fires on its own. Each opens with a manga cut-in.
- Thirteen rally shots, including 跳殺, 點殺, 劈殺, 撲球, 切吊 and 勾對角, each with its own risk, speed, trajectory and effects. Defenders can scramble back would-be winners (救球) with a diving pose.
- Tension: rally counter, GAME POINT / MATCH POINT banner, focus lines and a heartbeat in long rallies, and slow motion on the shot that decides a game.
- Match report: opens when a match ends (and from the result panel) with game scores, an MVP (direct winners ×3, saves ×2, skill points ×2, errors −1.5, +4 for the winning side) and each player's scoring rate, error rate, initiative / passive split and skill rate. Phones get one card per player.
- Post-match analysis: three replayable highlights, point sources, per-player breakdowns, and a matchup test against five style archetypes. Player cards can be shared as links (#player=…) and imported into any slot.
- Manual shots, automatic rally, automatic match; pause/resume and playback speed.
- Manual shots are resolved by the same level/personality/style model as automatic play (net, out, outright winner), with pre-shot odds shown in Courtside Notes. Court Settings can switch back to route-only mode with a manual point winner.
- Camera views: the tactical diagonal view, plus a perspective camera behind the blue baseline with presets for broadcast and low courtside angles. In the perspective camera, drag vertically on empty court (or use the slider) to raise or lower the camera continuously; a tap still picks a landing spot.
- Per-shot flight time (smash fastest, clears slowest) and three synthesised hit sounds (smash, clear, other) with a sound toggle.
- Draggable court positions and keyboard-accessible player/shot/target buttons. Space plays/pauses when focus is outside an input or button.
- Shot timeline, replay, undo and branching from the state before a selected shot; score and records roll back with the branch.
- Browser-local session persistence. Editing the roster preserves progress; restart explicitly clears the match.
- Inspectable JSON export, file download and clipboard alternatives.

## Implementation

- `model.js`: pure match rules, service slots, shot selection, seeded simulation and trajectories.
- `court.js`: responsive Canvas renderer, tactical (diagonal desktop / frontal mobile) and broadcast perspective projections, inverse touch mapping and image cache.
- `audio.js`: Web Audio synthesis for the three hit sounds (smash adds a soft-clipped crack, sub boom and a limiter); no audio assets.
- `abilities.js`: stats, budgets and presets, signature skills and the momentum meter.
- `radar.js`: the ability radar as inline SVG.
- `analysis.js`: match summary, highlights, matchup simulation and shareable player cards.
- `effects.js`: procedural manga-style impact effects for smashes (hit-stop, flash, focus lines, jagged burst, speed wedges, floor splash, shout label), drawn in court metres so they follow every camera.
- `characters.js`: original SVG portraits and expressions, shared between roster and court.
- `app.js`: UI orchestration, animation/replay state, persistence and onboarding.
- `styles.css`: responsive editorial sports interface, keyboard focus and reduced-motion/transparency/contrast variants.

There are no runtime packages. Web fonts use `font-display: swap` and system fallbacks. An internet connection is only needed for the optional font styles; court graphics and characters are local.

## Verification

From this folder:

```powershell
npm.cmd test
```

Forty Node tests cover service rules, scoring/deuce/caps, 100 seeded complete series, real net/out destinations, successful flight clearance, hidden-canvas regression, tactical and broadcast projection/inverse mapping, reduced-motion rendering, shot timing/sound classes and level-driven odds. Actual browser checks and outstanding design work are recorded in `DESIGN-QUALITY.md`.

The automatic model is a simplified tactical simulation, not a real-world win probability estimator. Route-only manual mode lets the user declare the rally winner. Service height, double hits and complete collision physics are outside the model.

### 人物池（2026-10-04）

男生：Jay 10、Curt 8、Leo 6、KK 7、Brain 7、Ethan 9、Owen 5、Noah 6。
女生：Rena 4、Mia 6、Ivy 6、Nora 6、Luna 7、Zoe 8、Ella 5、Aria 9。
Brain 使用淺膚色、平頭與綁結頭巾。選角卡顯示預設級數；選取角色與模式換人會套用該角色預設級數，保留可自訂的球風、性格與裝備。既有儲存人物在載入時保留自訂級數。人物卡支援新外觀，42 項自動測試通過。

### 能力雷達修正

五軸呈現力量、速度、網前、防守、穩定；刻度固定 0–13，容納球拍與專長加成。實線為有效能力，虛線為原始分配。移除僅由級數生成的絕技展示軸；絕技保留獨立發動機制。角色卡雷達改為正常排版，避免覆蓋文字；編輯器顯示各能力作用與使用實際 shotSpeed 計算的殺球速度。模型以相對自身平均的 edge 計算特長，級數維持整體實力；不把雷達數值或面積解讀為固定勝率。44 項測試通過，涵蓋同級力量／防守／穩定對實際球路機率的影響。

### Character Roster (2026-10-05)

`ROSTER` remains the single, immutable template source. `characterPreset()` deep-copies a template into a player with a stable `characterId`; `profiles` retain the existing four-slot shape. Assignment prevents duplicate characters and uses `slotGender()` for every destination. Empty positions block entering or restarting the court until all four positions are filled.

Each character has an original codename, tactical description, strengths, weaknesses, specialties, signature skill, appearance colors and bounded shot preferences. `choosePlan()` applies preferences after court, style and personality weights and before skill selection. Five-axis abilities and `shotOdds()` / `oddsFor()` remain responsible for outcomes. Preferences change selection frequency, not shot success probabilities.

Version-2 sessions remain supported. Known legacy names gain identity metadata while retaining saved customizations; unrecognized custom names stay custom profiles. Player-card links now preserve identity, hair color, accent and tendencies; old links still decode. Hair and accessory colors follow the character through swaps, while jerseys follow the team. CourtRenderer's face cache includes appearance colors.

`npm test` runs 53 tests, including the original regression coverage and nine character-roster tests. Racket-only and specialty-only fixtures explicitly clear preset specialties to continue testing those modifiers independently.
