# Almanac o' Adventures: shared fixes

The Almanac works out how to show every creature, item, structure and biome of any mod by itself. Sometimes it gets one wrong: an animation runs too fast, a creature that should stand alone is grouped with another, a head doesn't turn. This repository is where those are reported and put right, so that one answer fixes it for everyone.

## How it works

1. A player opens the close-up of the page that is wrong and clicks the **!** button (top right of the frame), then picks what is wrong.
2. The book saves a report (`almanac_o_adventures/reports/…json`) and copies it. If the pack's config names this repository, it also offers to open a new issue here with the report filled in.
3. Someone looks at the report, works out the right values and adds an entry to `fixes.json`.
4. Every game whose config points at `fixes.json` fetches it when a world is joined and uses the fix from then on. No new version of the mod is needed.

A report holds the page, the mod and its version, the versions of the Almanac, Minecraft and Forge, and what the book worked out. It holds nothing about the player.

## Pointing a pack at this repository

In `config/almanac_o_adventures-common.toml`:

```toml
fixesUrl = "https://raw.githubusercontent.com/<owner>/almanac-fixes/main/fixes.json"
reportUrl = "https://github.com/<owner>/almanac-fixes/issues/new?labels=report"
fetchFixes = true
```

A pack can also keep fixes of its own in `config/almanac_o_adventures-fixes.json` (same format). Those win over the shared list.

## The format of `fixes.json`

```json
{
  "format": 1,
  "creatures": {
    "somemod:fast_walker": { "pace": 0.4, "note": "legs swing 2.5x too fast", "versions": "[1.0,2.0)" },
    "somemod:statue":      { "still": true },
    "somemod:blob":        { "noHead": true },
    "somemod:copy":        { "copyOf": "somemod:original" },
    "somemod:lookalike":   { "separate": true },
    "somemod:marker":      { "leaveOut": "a marker, not a creature" }
  }
}
```

| Key | Meaning |
|---|---|
| `pace` | How fast its legs go when it walks, as a share of the usual (0.05 to 4). |
| `clock` | How fast its whole animation runs (0.05 to 4). |
| `still` | `true`: nothing of its own moves, so the book makes it look about. `false`: it does move. |
| `noHead` | `true`: it has no head to turn, so its body follows the pointer instead. |
| `copyOf` | It is the same creature as this one, registered twice: shown once. |
| `separate` | Never group it with look-alikes. |
| `leaveOut` | Not worth a page; the text says why. |
| `versions` | Optional. The versions of its mod the fix applies to, as a Forge range. Left out: all versions. |
| `note` | Optional. Why the fix exists. |

Only creatures have fixes so far. Items, structures and biomes can be added to the format when a report calls for it.
