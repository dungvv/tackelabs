# Design

<!-- Written at finish of the first build, from the shipped world. -->

## World: Amber Phosphor Terminal

The site IS the product's thesis: a "company operating system" shown as an
actual OS — booting, mounting modules, taking commands. Nothing describes
the product; everything demonstrates it. The coming-soon state is dramatized
as a system in final assembly, not a countdown or a placeholder.

## Palette

| Token | Value | Use |
|---|---|---|
| `--screen` | `#0c0a06` | page ground, warm near-black |
| `--screen-2` | `#100d08` | terminal window field |
| `--bezel` | `#1c1810` | title/status bars |
| `--line` | `#2a2415` | borders only |
| `--phosphor` | `#ffb340` | primary text, amber phosphor |
| `--phosphor-hi` | `#ffd08a` | emphasis, links |
| `--dim` | `#98855a` | secondary text, descriptions |
| `--faint` | `#4d422c` | timestamps, chrome texture |
| `--green` | `#8fd977` | `[ OK ]`, GRANTED, prompt user, LEDs |
| `--red` | `#ff6f5e` | errors only |

Green is reserved for success states; amber carries all identity. Never
introduce a third hue.

## Type

- **Martian Mono** for everything — the world's only face. Hierarchy is
  weight (300–800), color, and caps — never size. Sizes: 15px session text,
  11px chrome text, 11px session under 700px.
- ASCII/box-drawing glyphs are lettering, not icons. No pictographic icons
  exist in this world.

## Composition

- One terminal window: slim title bar (LEDs, `tackelabs-os`, session meta),
  `.session` scrollback (the only scroll region), tmux-style status bar
  (`tackelabs` tag, build, `tackelabs.com`).
- All content is terminal output: kernel-log lines `[ t.tttttt]`, dotted
  leaders, `man`-page blocks (`NAME`/`SYNOPSIS`/`DESCRIPTION`/indented
  5-space body), aligned columns. No cards, no sections, no hero shell.
- The prompt `guest@tackelabs:~$` + block caret is the only interactive
  element; every action is a typed command (`waitlist`, `help`, `modules`…).

## Motion

- One authored sequence: CRT power-on (scaleY flash) → boot log types
  itself line by line with variable cadence → `man tackelabs` runs → live
  prompt. Any key skips the boot.
- Caret blinks on a 1.06s step. No other ambient motion, no hover effects
  except link invert (phosphor ground, screen text).
- `prefers-reduced-motion`: everything prints instantly, no blink, no
  power-on.

## States

- `[ OK ]` green / `[ WAIT ]` amber — always `&nbsp;`-joined so brackets
  never split across a wrap.
- Errors print in `--red` with a named recovery (`error: … recovery: …`).
- Browser surfaces are themed: amber selection (phosphor on screen),
  dashed amber focus ring, thin phosphor scrollbar, caret-color.

## Responsive

- ≤700px: compact `┌ TACKELABS OS ┐` box banner replaces the 73-col ASCII
  wordmark; session font 11px; module descriptions hidden; dot leaders
  shorten (19 cols vs 30).
- ≤640px: bezel goes edge-to-edge (no radius, no side borders); titlebar
  meta and statusbar mid/right items hidden.
- Long lines wrap at word boundaries (`break-word`), like a real narrow tty.

## Rules for future surfaces

- Stay in grammar: timestamps, command echoes, aligned columns, bracketed
  states. New content is new commands or new log output, never new chrome.
- Anything interactive must be reachable as a typed command.
- `WAITLIST_ENDPOINT` (top of the inline script) is the integration point
  for a real form backend; empty = localStorage queue.
