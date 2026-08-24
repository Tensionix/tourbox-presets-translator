# Audion TourBox Presets Translator

<!-- audion:release -->
[![Windows](https://img.shields.io/badge/Windows-10%20%7C%2011-0b6db8?style=flat-square&logo=windows&logoColor=white)](https://audion.dev/downloads/tourbox-presets-translator) [![Release](https://img.shields.io/github/v/release/Tensionix/tourbox-presets-translator?style=flat-square&label=release&color=e08a63)](https://github.com/Tensionix/tourbox-presets-translator/releases/latest) [![Downloads](https://img.shields.io/github/downloads/Tensionix/tourbox-presets-translator/total?style=flat-square&label=downloads&color=5fd08a)](https://github.com/Tensionix/tourbox-presets-translator/releases) [![License](https://img.shields.io/github/license/Tensionix/tourbox-presets-translator?style=flat-square&color=5fd08a&logo=apache&logoColor=white&cacheSeconds=3600)](https://github.com/Tensionix/tourbox-presets-translator/blob/main/LICENSE)

**Version 1.0.0** · 2026-08-25 · 3.0 MB

- [Direct download](https://audion.dev/get/tourbox-presets-translator/1.0.0/Audion_TourBox_Presets_Translator_v1.0.0_Full.zip) — unmetered, no rate limits
- [Project page](https://audion.dev/downloads/tourbox-presets-translator) — every version and how to install

`SHA-256: 5576c1c44557376369e47c4df75a26d802aacdf7354f524a7653a9611d005e68`

---

An **Audion** tool, published by [Tensionix](https://github.com/Tensionix).
<!-- /audion:release -->

Write TourBox Elite presets from a readable text file instead of clicking through
Console — plus a complete, evidence-backed description of the `.tb` format.

## The fastest way in: describe it, don't build it

Hand this whole project to any capable AI assistant — the folder, or the archive,
whichever is easier — and tell it, in as much detail as you like, what you want
your TourBox to do. In which application. Which knob for what. It writes the
manifest; you run one file; the preset is ready to import.

That works because everything the assistant needs travels with the project:

- [`FORMAT.md`](FORMAT.md) — the container, the control table, the action
  encoding, the modifier bits, and all 47 control slots, each marked *confirmed*,
  *inferred* or *unresolved*;
- [`TRANSLATOR.md`](TRANSLATOR.md) — what the translator promises and what it
  refuses to do;
- thirteen worked manifests in [`../manifests`](../manifests), each opening with a
  `_purpose` line explaining the intent behind the mapping.

And you can check the result without trusting anyone: `Verify Presets.cmd` proves
every built file reads back byte for byte, and `Show Preset Contents.cmd` lists
what each control actually sends. An assistant that invented a control name gets
an error rather than a quiet mistake — the tool never picks a slot by guessing.

## Or take one that already works

Thirteen presets are built and ready in [`../presets`](../presets). Import one
into TourBox Console and you are done — nothing to build, nothing to install.

| Preset | What it is built around |
|---|---|
| `00-windows-global` | conservative fallback — real wheel, arrows, undo/redo, Find |
| `10-browser-general` | Dial cycles tabs, Knob walks history, Side+Scroll zooms the page |
| `20-office-word` | Knob pages, Dial jumps by section, Tour opens the context menu and Scroll walks it |
| `21-office-excel` | Knob pages, Dial pans a screen sideways, Side+D-pad jumps by data block |
| `22-office-powerpoint-show` | presenter commands, Side layer holds laser / pen / arrow / erase |
| `30-dev-vscode` | Dial cycles editors, Side+Knob steps through problems, Tall opens the palette |
| `32-files-total-commander` | Knob walks the directory tree, C1/C2 are history back/forward |
| `40-media-potplayer` | frame → seconds → coarse jump, on Knob → Dial → Side+Knob |
| `41-media-foobar2000` | rotations run the playlist, seek and volume; buttons do transport |
| `50-audition-waveform` | Knob stretches the selection, Dial scrubs, Scroll zooms, C1/C2 walk the markers |
| `60-premiere-keyframes` | all three rotations act on the keyframe — find it, move it, change its value; the Prime Four become held modifiers for dragging clips |
| `61-premiere-lumetri` | point at any control and turn: the same wheel action at three speeds, ultra-fine on Dial through coarse on Scroll |
| `70-lightroom-develop` | Knob picks the slider, Dial turns it in steps, Scroll wheels whatever the pointer is over |

Two of them need a one-time setup in the host application, both documented:
[Premiere](PREMIERE_SETUP.md) and — for the three plugin keys — Audition.

### The one to look at first: Total Commander without a keyboard

[`32-files-total-commander`](../manifests/32-files-total-commander.json) runs the
whole file manager off the device. Not "most of it" — the function-key workflow,
moved off the keyboard entirely.

The argument behind it is in the manifest itself, and it is the kind of thing a
mapping is usually missing:

- **F5 and F6 stop blurring.** Copy and move sit next to each other on a keyboard
  and feel identical under the fingers. Here they are two different physical
  buttons, and you stop checking which one you pressed.
- **No combinations at all.** Every command is a single press, a double press or
  a turn. Trading a keyboard chord for a device chord would have gained nothing,
  so it was not done.
- **The Knob is the whole navigation.** Turn it to walk the list, press it to go
  in — `Ctrl+PageDown` enters a folder and an archive alike, so the same press
  works everywhere. Coming back out is the Short button: `Backspace`, which is
  what the hand already reaches for.
- **Two rotations that never fight.** Knob moves the selection file by file;
  Scroll sends the real mouse wheel and scrolls the view. Different jobs.
- **Almost layout-proof.** F2–F8, Alt+F1/F2, Alt+F7, Ctrl+PageUp/PageDown, Enter,
  Tab, Backspace and Insert carry no letter, so the preset survives a Cyrillic
  keyboard layout. Only `Ctrl+T` needs Latin.
- **Two commands are deliberately absent.** `Ctrl+R` and `Ctrl+D` are rare next to
  view and edit, and no slot was left that cost a single movement — so they stay
  on the keyboard, and the manifest says so rather than quietly dropping them.

Every manifest carries that reasoning in its `_purpose` and, where the mapping
had to depart from the intent, in a `_deviation` field. This is what keeping a
layout as text buys: the decisions travel with the file, and the next person —
or the next assistant — can argue with them.

These reflect one person's workflow. Fork the manifest, change a line, rebuild —
that is the point of keeping them as text.

## Or write it yourself

A manifest is small enough to read at a glance:

```json
{
  "name": "Browser - General",
  "bind": {
    "dial":      { "a": "ctrl+shift+tab", "b": "ctrl+tab" },
    "knob":      { "a": "alt+left",       "b": "alt+right" },
    "knobPress": "ctrl+l",
    "side+dpadLeft": "ctrl+home",
    "c1":        "ctrl+shift+t"
  }
}
```

Double-click **Build Presets.cmd**, import the result into TourBox Console. That
is the whole loop.

| Wrapper | What it does |
|---|---|
| `Build Presets.cmd` | every manifest → a `.tb` in `presets\` |
| `Verify Presets.cmd` | proves each built file reads back byte for byte |
| `Show Preset Contents.cmd` | lists what every preset actually sends, control by control |

No arguments, no flags — one file per action. The scripts find Python themselves
and work from wherever the folder sits.

A binding is either a shortcut string, or an `{a, b}` pair for the two rotation
directions:

```json
"scroll":     { "a": "up",  "b": "down" },
"top":        "ctrl+f",
"tallx2":     "escape",
"id:26":      "p1:33"
```

A shortcut is modifiers plus exactly one key. Modifiers are `ctrl`, `alt`,
`shift`. A key is a single character (`a`, `7`, `[`, `=`), a named special key
(`left`, `pagedown`, `f12`, `numpad7`), a spelled-out control (`space`, `enter`,
`escape`, `tab`, `backspace`, `delete`), or `p1:N` to address a raw code.

Control names are in [`FORMAT.md` §8](FORMAT.md). `id:N` addresses a raw slot. An
unknown name is an error — the tool never picks a slot by guessing.

The full contract — what the translator promises, what it refuses to do, and how
to check a preset once it is imported — is in [`TRANSLATOR.md`](TRANSLATOR.md).

## Why this exists

TourBox Console is the only way to author presets, and it is a lot of clicking for
a keyboard layout you already know. Presets are also awkward to diff, review or
keep in version control. This project makes the mapping a text file and the `.tb`
a build artifact.

Along the way the format had to be decoded properly. [`FORMAT.md`](FORMAT.md) is
the result — the container, the control table, the action encoding, the modifier
bits, the special-key table, and the full 47-slot control map, each entry marked
*confirmed*, *inferred* or *unresolved*.

## How the format was actually worked out

Nothing here came from documentation, because there is none. TourBox publishes no
format description, and Console does not explain itself. Every line in
[`FORMAT.md`](FORMAT.md) was earned the same way: build a file, feed it to
Console, read what Console says it means, export it back, compare the bytes.

That loop ran a lot of times. What survives of it is in the repository:

- **Seven probes**, `probe0` through `probe6` in [`../probes`](../probes). Each
  one asks a narrow question — which slot is which, what a modifier does to the
  payload, how a combination is recorded, what the mouse wheel looks like — by
  setting a controlled pattern and nothing else.
- **Three re-exports by Console itself**, in
  [`../probes/roundtrip`](../probes/roundtrip), named for the batches they cover:
  keys 1–20, 21–40, 41–60. Sixty key codes had to be walked in groups, imported,
  read off the screen label by label, and exported again — because the only
  authority on what a code means is what Console displays for it.
- **The vendor's own library.** `tools/tbcorpus.py` mines the several hundred
  presets shipped inside Console and the ones it keeps under
  `%APPDATA%\TourBox Console\import`, which is how you find out which codes are
  used in practice rather than which ones are merely possible.

The result is [`calibration/calibration.json`](../calibration/calibration.json):
the container, the control table, the record layout, the action block, the
modifier bits, the special-key table, the Qt key values, the corpus findings and
the write path — every one of them carrying a status rather than an assertion.
As it stands: **35 facts confirmed, 3 inferred, 9 still open**, and the open ones
are named in the file instead of being quietly rounded up.

That accounting is the point. A format decoded by observation is only as good as
its worst unmarked guess, so nothing here is presented as known unless a probe
demonstrated it — and where the evidence was indirect, the entry says *inferred*
and explains from what.

## One open question this project closes

The upstream project [`YongHee-Kim/tourbox-preset`](https://github.com/YongHee-Kim/tourbox-preset)
marks the modifier bits as **TBD**, with a single early observation recorded:
"a pure `Alt` binding sets payload byte 2 to `0x02`; full bitmask & byte
unconfirmed."

On an Elite with Console 5.11.3 the bits are Shift `0x02`, Alt `0x04`, Ctrl
`0x08`, confirmed by two independent lines of evidence — see
[`FORMAT.md` §5](FORMAT.md). So that early note reads Shift where it says Alt,
which is exactly why it was flagged unconfirmed. Anyone building on the upstream
table can take the values from here.

## What is decoded

- **Container** — gzip + XML + base64, byte-identical round trip
- **Modifiers** — Shift `0x02`, Alt `0x04`, Ctrl `0x08`
- **Keys** — any ASCII character, plus 45 named special keys: arrows, PageUp/Down,
  Home, End, Insert, F1–F24, the whole numpad
- **All 47 control slots** — including every combination: Side+D-pad, Top+D-pad,
  double presses, Prime Four pairs, Knob and Scroll held with a Prime Four button
- **The mouse wheel**, with modifiers — so a rotation adjusts whatever the
  pointer is over, which is how sliders with no shortcut get turned
- **Haptic strength**, set per rotation — confirmed
- **Turn speed**, set per rotation — implemented and strongly inferred from the
  corpus, but not yet read back from Console's own settings panel

Not addressable yet: a `Side+Dial` combination. The standard section has no such
row; Console can build one in its Custom Section, but which record id it takes is
uncalibrated — see [`FORMAT.md` §8](FORMAT.md).

Not decoded: mouse buttons and drag, TourMenu, macros, plugin commands. Drag is
the one that still matters — it is what a rotary would need to pull a curve
handle directly.

## Tools

| Tool | What it does |
|---|---|
| `tools/tbmake.py` | manifest → `.tb` |
| `tools/tbfmt.py` | `verify` round trip, `dump` bindings, `diff` two presets, `assign`/`set` raw edits |
| `tools/tbcorpus.py` | mine the vendor presets already on your machine — `kinds`, `keys`, `mods`, `qt` |

Python 3.9+, standard library only, no install, no hardcoded paths.

```
python tools/tbfmt.py dump  presets/10-browser-general.tb
python tools/tbfmt.py diff  probes/probe2.tb probes/roundtrip/*.tb
python tools/tbcorpus.py kinds
```

`tbcorpus.py` reads presets Console keeps under `%APPDATA%\TourBox Console\import`,
and can also read the several hundred presets embedded in Console's own program
library. Useful for seeing which codes the vendor actually uses in the wild.

## Layout

```
presets/      the .tb files you import — built artifacts
manifests/    the editable source
tools/        the three scripts
calibration/  calibration.json — every decoded fact, with a status
probes/       calibration probes, and Console's re-exports of them
vendor/       empty base preset used as the write template
Docs/         FORMAT.md, TRANSLATOR.md, the application map
```

## Reproducing the calibration

If you have a different TourBox model or firmware, do not trust this table —
re-derive it. The method is in [`FORMAT.md` §10](FORMAT.md) and takes about half
an hour: generate a probe, import it, read the labels, re-export, diff.

## Status

The keyboard half of the format is done and exercised end to end. The preset pack
itself is in progress — see
[`Audion_TourBox_Elite_Apps_Map.md`](Audion_TourBox_Elite_Apps_Map.md) for the
intended application maps.

Tested only on TourBox Elite, Windows, Console 5.11.3.

## License

MIT — see [`LICENSE`](../LICENSE). Everything in this repository is the author's
own work; nothing third-party is bundled, which
[`licenses/THIRD_PARTY_NOTICES.txt`](../licenses/THIRD_PARTY_NOTICES.txt) states
in full. The `.tb` container format itself belongs to TourBox and is described
here from observation, not from any TourBox source.

---

[Русская версия](README_RU.md)
