# 🎯 PLINKO DUNGEON

> **IMT01306618 Games Development | Odd Semester 2026/2027**  
> **Author:** Sean Tandjaja  
> **Engine:** GDevelop 5 (Physics 2.0 Box2D)  
> **Project Files:** `game.json` (Standard GDevelop) & `Plinko Dungeon.json`

![Plinko Dungeon Mockup](screen_mockup_1280x720.png)

---

## 📜 Logline
A 3-minute single-player physics-pachinko dungeon duel where you drop kinetic orbs through randomized multiplier pegs to slay a boss before it counter-attacks your health to zero.

---

## 🎮 Core Game Loop
1. **Aim**: Position the dropper horizontally across the top rail (`[A]` / `[D]` or Arrow Keys).
2. **Wager**: Choose your ante bet before dropping:
   - `[1]` **Standard Ante ($10)**: Safe, steady play.
   - `[2]` **High Roller ($25)**: High risk, exponential jackpot rewards!
3. **Drop**: Release your kinetic orb into the pegboard (`[SPACE]` or Left Click).
4. **Cascade & Visual Juice**: The orb bounces off pegs with Box2D elastic physics:
   - ⚪ **Standard Plain Peg**: `+1 DMG`, radiant cyan hit flash pulse, bursts **8 cyan pixel sparks** (`Fx_Pixel_Explosion`).
   - 🟡 **Multiplier Gold Peg**: `+0.5 Multiplier`, golden hit flash pulse, bursts **12 glittering gold stars**.
   - 🔴 **Bomb Red Peg**: `+6 AOE DMG`, fiery hit flash pulse, explodes **16 fiery crimson blast pixels** with shockwave.
5. **Score & Combat Resolution**:
   - The orb lands in one of the 5 bottom multiplier buckets: `[0.5x, 1.5x, 5.0x, 1.5x, 0.5x]`.
   - Landing in the center bucket triggers the **5.0x JACKPOT!**
   - Total Damage = `(Accumulated Peg DMG) × Multiplier × Bucket Multiplier`.
   - Chip Payout = `Wager × Bucket Multiplier`.
   - The boss takes total damage. If still alive, the boss counter-attacks player HP!
6. **3-Card Interactive Orb Drafting (Between Rounds)**:
   - When Boss 1 or 2 is defeated, a **3-Card Drafting Modal** pops up on screen:
     - ⚙️ **Standard Iron** (`Card 1` / `[1]`): Balanced weight and restitution (`0.65`), `+1 Base DMG`.
     - 🟢 **Bouncy Slime** (`Card 2` / `[2]`): Hyper-elastic (`0.95` restitution), `+0 Base DMG`, triggers multi-peg cascades.
     - 🪨 **Heavy Boulder** (`Card 3` / `[3]`): Heavy density (`2.5` mass, `0.35` restitution), crushing straight down `+5 Base DMG`.
   - Players can click any card or press `1`/`2`/`3` to equip and enter the next round.
   - Defeating a boss also restores **`+30 HP`** to the player.

---

## 🏆 Win & Lose Conditions
- **Win Condition**: Defeat all 3 dungeon bosses (deplete Dragon Landlord HP to 0 across 3 consecutive rounds).
- **Lose Condition**: 
  - Player HP reaches 0 from cumulative boss counter-attacks, OR
  - Player runs out of chips (`<$10`) and cannot place the minimum ante bet (starts with `$60`).

---

## 👹 Boss Escalation
| Round | Boss | Max HP | Counter-Strike DMG |
|---|---|---|---|
| **Round 1** | **Goblin Guard** | `90 HP` | `8 DMG` |
| **Round 2** | **Slime Brute** | `180 HP` | `12 DMG` |
| **Round 3** | **Dragon Landlord** | `260 HP` | `14 DMG` |

---

## 🕹️ Controls
| Action | Key / Input |
|---|---|
| **Move Dropper Left** | `[A]` or `[Left Arrow]` |
| **Move Dropper Right** | `[D]` or `[Right Arrow]` |
| **Drop Orb** | `[SPACE]` |
| **Select Standard Ante ($10)** | `[1]` (Aiming state) |
| **Select High Roller ($25)** | `[2]` (Aiming state) |
| **Draft Reward Orb** | **Click Card** or press `[1]`, `[2]`, `[3]` (Draft state) |

---

## 🎴 Post-Round Orb Drafting
Upon defeating **Goblin Guard** or **Slime Brute**, an interactive 3-card drafting modal pops up:
- **Standard Iron**: Balanced mass, +1 DMG, 0.65 restitution, highly predictable flight path.
- **Bouncy Slime**: Hyper-elastic (0.95 restitution), +0 base DMG, massive 20+ peg multi-hit combos.
- **Heavy Boulder**: High density, 0.35 restitution, +5 massive base DMG, plows straight down into the 5x Jackpot pit.
Cards are rendered with bold, high-contrast arcade typography (`Verdana Bold`) with distinct archetype badges, stat breakdowns, and hotkey indicators.

---

## 📁 Repository Structure
```
Plinko Dungeon/
├── game.json                    ← GDevelop 5 canonical project definition (opens directly across platforms)
├── Plinko Dungeon.json          ← GDevelop 5 alternate project file (synchronized 1:1)
├── screen_mockup_1280x720.png   ← 1280x720 art & layout reference
├── asset_preview.png            ← Asset preview sheet
├── .gitattributes               ← Preserves binary integrity for PNG/WAV across OSes
├── README.md                    ← Game design & repository documentation
└── assets/                      ← 32x32 pixel sprites & 16-bit audio assets (57 total)
    ├── card_draft_iron.png / card_draft_slime.png / card_draft_boulder.png
    ├── ui_draft_header.png / ui_banner_frame.png
    ├── ui_btn_ante.png / ui_btn_highroller.png / ui_aim_dots.png
    ├── ui_hp_frame.png / ui_hp_fill.png / ui_neon_horiz.png / ui_neon_vert.png
    ├── ui_chip.png / ui_heart.png
    ├── boss1_goblin_guard.png / boss2_slime_brute.png / boss3_dragon_landlord.png
    ├── bucket_0_5x_a.png / bucket_1_5x_a.png / bucket_5_0x.png / bucket_1_5x_b.png / bucket_0_5x_b.png
    ├── dropper.png
    ├── orb_standard_iron.png / orb_bouncy_slime.png / orb_heavy_boulder.png
    ├── peg_standard_white.png / peg_standard_white_hit.png
    ├── peg_multiplier_gold.png / peg_multiplier_gold_hit.png
    ├── peg_bomb_red.png / peg_bomb_red_hit.png
    ├── fx_spark_cyan_f1..f4.png / fx_spark_gold_f1..f4.png / fx_spark_bomb_f1..f5.png
    ├── torch_f1.png / torch_f2.png
    ├── wall_brick.png / wall_mossy.png
    ├── bgm_chiptune.wav         ← 130 BPM 16-bit arcade battle theme
    └── 7 Synthesized SFX (.wav)
```

---

## 🚀 How to Run in GDevelop 5
1. Open **GDevelop 5**.
2. Click **Open a project** (or browse to the folder).
3. Select `game.json` (or `Plinko Dungeon.json`).
4. Click the **Preview** button (play icon) in the top toolbar.

### ⚠️ Collaborator Git Pull Guide (If Assets Don't Load)
If collaborators pulled previously and encounter missing assets or red placeholders, it is usually caused by an outdated local working copy or Git blocking a pull due to local autosaves:
```bash
# 1. Reset any local autosaves or uncommitted scene changes
git reset --hard origin/main

# 2. Pull the latest assets and project files
git pull origin main

# 3. Open game.json or Plinko Dungeon.json directly in GDevelop
```
