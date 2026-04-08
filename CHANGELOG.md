# Keymap Changelog

Personal customizations on top of the stock MoErgo Go60 keymap. Newest first.

## RH_T3: hold = Hyper, tap = sticky RShift

Plain `&mt` couldn't capitalize via tap (mod-tap shift releases before the next
keypress, so holding-to-capitalize would fire Hyper instead). Added a custom
hold-tap `hyper_sk` that wraps `&kp` on hold and `&sk` on tap, so the right
pinky thumb is now `&hyper_sk LG(LA(LS(LCTRL))) RSHIFT`:

- Hold = Hyper (`Cmd+Ctrl+Alt+Shift`)
- Tap  = sticky right shift (next key is capitalized)

## Symbols layer rebuild + code combos

Optimized the keymap for fullstack dev (Rust, Go, TS, Bash, Elixir) in Neovim.

### Symbols layer (`layer_symbols`) — fully rewritten

Activation moved to **hold Space** (RH_T1). Left hand types symbols, right hand
becomes a nav/edit cluster. Layout grouped by code-frequency with mirrored
bracket pairs on home row:

```
┌──────┬──────┬──────┬──────┬──────┬──────┐    ┌──────┬──────┬──────┬──────┬──────┬──────┐
│      │  !   │  @   │  #   │  $   │  %   │    │  ^   │  &   │  *   │      │      │      │
├──────┼──────┼──────┼──────┼──────┼──────┤    ├──────┼──────┼──────┼──────┼──────┼──────┤
│      │  \   │  {   │  }   │  |   │  `   │    │ Home │ PgDn │ PgUp │ End  │      │      │
├──────┼──────┼──────┼──────┼──────┼──────┤    ├──────┼──────┼──────┼──────┼──────┼──────┤
│ Esc  │  =   │  (   │  )   │  -   │  +   │    │ Left │ Down │  Up  │ Right│  ;   │  '   │
├──────┼──────┼──────┼──────┼──────┼──────┤    ├──────┼──────┼──────┼──────┼──────┼──────┤
│      │  ~   │  [   │  ]   │  _   │  /   │    │      │      │  <   │  >   │  ?   │      │
└──────┴──────┴──────┴──────┴──────┴──────┘    └──────┴──────┴──────┴──────┴──────┴──────┘
```

Why:
- `()` `=` `-` `+` on home row → `=>` and `->` are one rolling motion on the strongest fingers.
- `{}` on row 2 index/middle → second-most-used brackets in TS/Rust/Go, no shift chord.
- `|` on home-row index → Rust closures `|x|`, TS unions `string | number`, bash pipes.
- `[]` on row 4 → least-used brackets in this stack (mostly array access).
- `mt LCTRL ESC` mirrored on the symbols layer so Vim escape works while symbol-typing.

### Thumb cluster (base layer)

| Thumb | Before | After |
|---|---|---|
| RH_T3 (pinky) | `&lt LAYER_symbols RSHIFT` | `&mt LG(LA(LS(LCTRL))) RSHIFT` (hold = Hyper, tap = RShift) |
| RH_T2 (ring)  | `&mt LG(LA(LS(LCTRL))) RET` | `&lt LAYER_nav RET` |
| RH_T1 (index) | `&lt LAYER_nav SPACE` | `&lt LAYER_symbols SPACE` |

Symbols moved from RH pinky to RH thumb-index (hit way more often than nav for
a coder). Hyper preserved on RH_T3 alongside the held right-shift. Left thumbs
unchanged: `&sk LSHFT` for one-shot caps remains on LH_T3.

### Code digraph combos (base layer only)

Vertical combos, 30ms timeout, fire macros that emit common digraphs:

| Combo | Keys | Output |
|---|---|---|
| `combo_fat_arrow`  | `J + M` | `=>` |
| `combo_thin_arrow` | `K + ,` | `->` |
| `combo_dbl_colon`  | `D + C` | `::` |
| `combo_neq`        | `S + X` | `!=` |
| `combo_eqeq`       | `A + Z` | `==` |
| `combo_and_and`    | `F + V` | `&&` |
| `combo_or_or`      | `G + B` | `\|\|` |

Vertical pairs are very unlikely to misfire during normal typing. Bump
`timeout-ms` to 40-50 if any of them feel finicky.

### Numpad layer (`layer_nums`)

Moved `0` from the buried bottom thumb row up to the index-inner column
(left of `4 5 6`) so the numpad is reachable as a self-contained cluster.
The slot it replaced previously held a redundant `LS(SEMI)` (`:`), already
covered by the `colon_semi` mod-morph on the base layer.
