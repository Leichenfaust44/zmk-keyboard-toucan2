# Toucan Keyboard — Gallium ZMK Config

[![Keymap](https://github.com/Leichenfaust44/zmk-keyboard-toucan2/blob/win_mode/keymap-drawer/beekeeb.jpg)](https://github.com/Leichenfaust44/zmk-keyboard-toucan2/blob/win_mode/keymap-drawer/beekeeb.jpg)

Custom ZMK firmware for the [beekeeb Toucan 2 Keyboard]([https://beekeeb.com/toucan-keyboard2/]) using the **Gallium v2** Columnar Staggered layout, optimised for Swiss German (DE-CH) input.

This config uses the **5-col layout**, making key positions identical to the [Piantor Pro BT](https://github.com/Leichenfaust44/zmk-config) config for easy cross-board maintenance.

[![Keymap](https://github.com/Leichenfaust44/zmk-keyboard-toucan/raw/main/keymap-drawer/toucan.svg)](https://github.com/Leichenfaust44/zmk-keyboard-toucan/blob/main/keymap-drawer/toucan.svg)

---

## Display

The left half features a **nice!view gem** display (Sharp Memory LCD). It shows the following widgets from top to bottom:

[![Display](https://github.com/Leichenfaust44/zmk-keyboard-toucan/raw/main/keymap-drawer/toucan_display.svg)](https://github.com/Leichenfaust44/zmk-keyboard-toucan/blob/main/keymap-drawer/toucan_display.svg)

| Widget | Description |
|-|-|
| **Batteries** | Charge bars for local (left) and peripheral (right) halves |
| **WPM chart** | Bar chart of last 10 WPM values, fixed range 0–100 |
| **Active layer** | Current layer name displayed large in the middle |
| **BLE profile** | 5 profile dots, active profile filled |

Display configuration via [`nice_view_gem`](https://github.com/M165437/nice-view-gem).

---

## Layout Overview

### Base (Default Layer)

[Gallium](https://github.com/GalileoBlues/Gallium) is an alternative layout designed to minimise same-finger bigrams and lateral stretches. This config uses the columnar-staggered version with X and Q swapped.

```
B  L  D  C  V  |  J  F  O  U  ?/!
N  R  T  S  G  |  Y  H  A  E  I
X  Q  M  W  Z  |  K  P  ,/; ./: -/_
```

**Hold-tap keys on the base layer:**

* `,` → `;`
* `.` → `:`
* `-` → `_`
* `?` → `!`

**Home row mods** (cross-hand activation only, balanced flavor):

Two separate behaviors are used — Shift activates faster, Ctrl/Alt/Super require a deliberate hold.

* Left: `N`=Super(slow) · `R`=Alt(slow) · `T`=Ctrl(slow) · `S`=Shift(fast)
* Right: `H`=Shift(fast) · `A`=Ctrl(slow) · `E`=Alt(slow) · `I`=Super(slow)

| Behavior | Keys | `tapping-term-ms` | `require-prior-idle-ms` |
|-|-|-|-|
| `hml_shift` / `hmr_shift` | S, H | 200 ms | 20 ms |
| `hml_slow` / `hmr_slow` | N, R, T, A, E, I | 400 ms | 400 ms |

**Thumb cluster:**

```
ESC (hold: FN) | MagicKey (hold: Nav) | Backspace/Del | SmartShift | Space | Sym
```

---

## Thumb Key Behaviours

| Key | Tap | Hold |
|-|-|-|
| **ESC** | Escape | FN layer |
| **MagicKey** | Adaptive key (see below) | Nav layer |
| **Backspace** | Backspace | — |
| **Backspace** (with Shift) | Delete | — |
| **SmartShift** | Sticky Shift (capitalises next key only) | Regular Shift |
| **SmartShift** (double-tap) | Caps Word (capitalises until word boundary) | — |
| **Space** | Plain space | — |
| **Sym** | `num_word` (auto-exit number layer) | Momentary Sym layer |

### Magic Key — Adaptive Substitutions

The Magic Key applies smart substitutions for SFB bigrams on the Gallium layout. For any other key, nothing happens (no key repeat).

| After typing | Magic Key outputs | Rationale |
|-|-|-|
| `E` | `U` | «eu» SFB Ringfinger rechts — heute/neu/Euro |
| `P` | `F` | «pf» SFB Zeigefinger rechts — Pferd/Kopf |
| `-` | `>` | Produces `->` |
| `a` / `A` | `ä` / `Ä` | Umlaut substitution (1 s window) |
| `o` / `O` | `ö` / `Ö` | Umlaut substitution (1 s window) |
| `u` / `U` | `ü` / `Ü` | Umlaut substitution (1 s window) |

Umlaut substitutions have a 1-second timeout window; all others have a 5-second window.

> **Hinweis:** Zusätzlich zur adaptiven Ersetzung oben gibt es einen zweiten, proaktiven Weg für Umlaute: `MagicKey` + `A`/`O`/`U` gleichzeitig gedrückt (Combo) liefert direkt `ä`/`ö`/`ü` bzw. `Ä`/`Ö`/`Ü` bei gehaltenem Shift, ohne auf das Substitutions-Zeitfenster angewiesen zu sein. Siehe [Combos](#combos).

---

## Sym (Layer 1) — Numbers and Symbols

Numbers on the left, symbols on the right. Home row mods mirror the base layer split (Shift fast, Ctrl/Alt/Super slow). Activated by the Sym thumb (momentary hold) or `num_word` (tap — auto-exits when a non-number/symbol key is pressed).

```
 —   7   8   9   —  |  [   ]   <   >   |
 0   4   5   6   —  |  {   }   ^   `   ~
 .   1   2   3   —  |  `   ´   §   °   —
```

**Thumb cluster:** `to(0) | — | Backspace* | to(0) | Space* | num_word`

`*` = transparent, fällt durch auf die Base-Layer-Funktion der jeweiligen Taste.

---

## FN (Layer 2) — Function Keys

Function keys on the left (F1–F12 arranged like a numpad), raw modifier keys on the right home row for `Shift/Ctrl/Alt/Super + Fx` combinations. Activated by holding ESC.

```
F10  F7  F8  F9  —  |   —   —    —    —    —
F11  F4  F5  F6  —  |   —  RSft RCtl RAlt RGui
F12  F1  F2  F3  —  |   —   —    —    —    —
```

**Thumb cluster:** `— | — | — | — | — | —`

---

## Nav (Layer 3) — Navigation and System Controls

Navigation on the right, system/media controls on the left. Activated by holding MagicKey. **Trackpad acts as scroll wheel on this layer.**

```
BtClrAll**  Next  VolUp  RGB_OFF  StudioUnlk  |  PgUp  Home   Up    End    BtClr**
—           Prev  VolDn  —        —           |  PgDn  Left   Down  Right  Reset**
Boot**      Play  Mute   MicMute  —           |  CapsL  —    PrtSc   —     Boot**
```

**Thumb cluster:** `BT_PRV | BT(0) | BT_NXT | Desktop W | — | Desktop P`

`Desktop W/P` = `Ctrl+Gui+Left/Right` (macOS Spaces: vorherige/nächste Fläche).

`**` = tap-dance safety: single tap does nothing, double-tap triggers the action (BtClr, BtClrAll, Boot).  
`MicMute` = `LGui+RAlt+K` shortcut.  
`CapsL` = Caps Lock (manual toggle).

---

## Trackpad Behaviour

The Toucan's Cirque Pinnacle trackpad (right half) is always active as a mouse cursor. Behaviour changes by layer:

| Layer | Trackpad behaviour |
|-|-|
| **Base (0)** | Cursor movement + tap-to-click (left click) |
| **Sym (1)** | Cursor movement + tap-to-click |
| **FN (2)** | Cursor movement + tap-to-click |
| **Nav (3)** | Scroll wheel (horizontal scroll inverted) |

**Mouse clicks:**

| Action | Result |
|-|-|
| Tap touchpad | Left click (tap-to-click) |
| `M+W` combo (Base layer) | Left click |
| `Q+M` combo (Base layer) | Right click |

Pointer speed: 3.5× (`zip_xy_scaler 350 100`). Scroll speed: 1/5× (`zip_scroll_scaler 1 5`).

Power management: trackpad enters idle after 30 seconds of no keypresses, waking in ~300ms on touch. Deep sleep after 60 minutes.

---

## Combos

Most combos are active on Base and Sym layers (Tab and Return also work on FN); Copy/Paste/Cut, the mouse-click combos, and the umlaut combos are Base-layer only.  
All combos use `timeout-ms = 50ms`. `require-prior-idle-ms` varies: 400ms (symbols), 300ms (Tab/Return), 800ms (mouse clicks), none set (umlaut combos — see below).

**Whitespace (horizontal home row):**

* `R+T` → Tab
* `T+S` → Return

**Symbols — left hand vertical (top+home / home+bottom):**

```
B+N → "    L+R → @    D+T → #    C+S → $
N+X → '    R+Q → %    T+M → \    S+W → =
```

**Symbols — right hand vertical (top+home / home+bottom):**

```
Y+H → +    O+A → *    U+E → €    ?+I → Copy
H+P → -    A+, → /    E+. → &    I+- → Paste
?+- → Cut  (top+bottom pinky stretch)
```

**Brackets — right hand horizontal (bottom row):**

* `P+,` → `(`
* `,+.` → `)`

**Umlaute — Magic Key + Vokal (Base layer only, Shift-sensitiv):**

* `MagicKey+A` → `ä` / `Ä` (bei gehaltenem Shift)
* `MagicKey+O` → `ö` / `Ö`
* `MagicKey+U` → `ü` / `Ü`

Kein `require-prior-idle-ms` gesetzt, da Magic Key ohnehin schnell nach Vokalen erreicht werden soll. Beeinträchtigt die einzelne Magic-Key-Funktion (adaptive Bigramme, Nav-Layer) nicht — Combos werden vor der Tap/Hold-Auflösung der beteiligten Tasten ausgewertet.

**Mouse clicks — left hand bottom row (Base layer only, 800ms prior-idle):**

* `M+W` → Left click
* `Q+M` → Right click

---

## Flashing Firmware

### Download

1. Go to the **Actions** tab → latest successful build
2. Download the `firmware` artifact and extract the `.zip`:

   * `toucan_left_rgbled_adapter_nice_view_gem-seeeduino_xiao_ble-zmk.uf2`
   * `toucan_right_rgbled_adapter-seeeduino_xiao_ble-zmk.uf2`

The keyboard reboots automatically after copying.

---

## Bluetooth

### Pairing

Turn on both halves and search for "Toucan" on your device. The keyboard is in pairing mode automatically.

### Switching / Managing Profiles

Access the Nav layer (hold MagicKey) and use the thumb cluster:

* `BT_PRV` / `BT_NXT` — cycle profiles
* `BT(0)` — select profile 0

Destructive BT operations (BT\_CLR, BT\_CLR\_ALL) require a double-tap on Nav layer (hold MagicKey).

---

## Battery & Power

* Each half has an ON/OFF switch (OFF = fully disconnected, ON = sleep when idle)
* Deep sleep after 60 minutes of inactivity
* Trackpad enters idle after 30 seconds of no keypresses to save power (~1.7 mA idle vs 2.9 mA active)

---

## Modifying the Config

1. Edit `config/toucan.keymap`, `config/toucan_behaviors.dtsi`, or `config/toucan_combos.dtsi`
2. Push → GitHub Actions builds automatically
3. Download the firmware artifact from the Actions tab

The keymap, behaviors, and combos use **identical key positions and logic** to the [Piantor Pro BT config](https://github.com/Leichenfaust44/zmk-config) — changes can be ported between boards with minimal adaptation.

**Tools:**

* [ZMK Studio](https://zmk.studio/download) — GUI editor (use Studio Unlock on Nav layer)
* [keymap-editor](https://nickcoutsos.github.io/keymap-editor) — browser-based editor

---

## License

Based on [ZMK Firmware](https://zmk.dev), MIT License.

The included shield `nice_view_gem` is modified from [M165437/nice-view-gem](https://github.com/M165437/nice-view-gem), MIT License.

The embedded font QuinqueFive is designed by GGBotNet, licensed under the SIL Open Font License, Version 1.1.
