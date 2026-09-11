# Test scenarios

This mod has never been loaded by RimWorld. Everything below is what the first run has to settle.

**Why this is a matrix and not a checklist.** Every fault found in this port was conditional on
the rest of the modlist, and **no single run shows more than one of them**. The 3-person bed
exists or not depending on which polyamory mod is loaded; the hay crib exists or not depending on
Biotech; the leather beds are removed in every case, but only two of the three were being removed
before. One "it loaded, looks fine" proves almost nothing here. Five short runs prove the lot.

The faults were all confirmed offline, by replaying both mods' patch operations over a combined
document once per modlist. That is why the list below is specific. It is not why it can be
skipped: a replay is not the game.

---

## Load order

Enable in this order. The first two are hard dependencies; RimWorld warns about a missing
dependency and lets you start anyway, which is exactly the case the guards were added for.

```
zal.primitivecore                  Primitive Core (Continued)          3751286931
zal.primitiveproduction            Primitive Workbenches (Continued)   3751287735
Mlie.JPTSoftWarmBeds               [JPT] Soft Warm Beds (Continued)    3006469889
Meltup.PolyamoryBeds.Vanilla       Polyamory Beds (Vanilla Edition)    3276496684   (scenarios B, D)
Mlie.RationalRomance2              Rational Romance 2 (Continued)      2593521827   (scenarios C, D)
nelim.softwarmprimitivebedsrenew   this mod                                         always last
```

## What to search the log for

`Player.log` sits in
`%USERPROFILE%\AppData\LocalLow\Ludeon Studios\RimWorld by Ludeon Studios\Player.log`.

These are the exact strings the 1.6 assembly writes, read out of it rather than remembered. Each
one means something different here.

| String in the log | Written by | What it would mean for this mod |
|---|---|---|
| `Adding duplicate` | `DefDatabase.Add` | **The fault this port exists to fix.** `VBY_TripleHayBed` declared twice. Must never appear, in any of the five runs. |
| `Patch operation` … `failed` | `PatchOperation.Complete` | An xpath matched nothing. Expected count from this mod: **zero**. |
| `Error while resolving references for def` | `DefDatabase.ResolveAllReferences` | A type named in element text resolved to null and threw during resolution. Names the def. |
| `Could not resolve cross-reference` | `DirectXmlCrossRefLoader` | A `defName` pointing at nothing — a blanket, a bedding, or the research prerequisite. |
| `Could not find type named` | `DirectXmlToObject.ClassTypeOf` | A `Class="..."` that does not exist. |
| `Could not find a type named` | `ParseHelper.ParseType` | A type named in element **text**, i.e. `inspectorTabs`. Different message, different code path, same cause. |
| `Failed to find any textures at` | `Graphic_Multi.Init` | A `texPath` with nothing behind it. |

A clean run means **none of those seven naming a `VBY_`, `CYB_` or `SoftWarmBeds` identifier**.
Lines naming other mods are not ours to fix, and are worth leaving in the paste anyway.

---

## The five scenarios

### A — the floor: dependencies only, Biotech **off**

The point is the hay crib. Primitive Workbenches deletes `VBY_HayCrib` unless Biotech or
Children, school and learning is loaded, and this mod used to aim eight patch operations at it
regardless — eight red lines at every startup on a colony with neither.

- No `Patch operation … failed` from this mod. **That is the whole scenario.**
- No hay crib in the Furniture tab, and no error about one.
- `simple crib bedding set` may still be craftable, with no crib to use it on. Known and
  deliberate — see *Known and accepted* below.
- The hay bed and the double hay bed behave as in scenario B.

### B — Polyamory Beds only

The case cyanobot's note describes, and the one that always worked.

- **`3-person hay bed` exists, exactly once**, in the Furniture tab, behind the
  `VBY_PrimitiveProduction` research.
- It has the **Bedding** tab, and its storage filter is set to Textiles.
- `simple multi bedding set` is craftable at a crafting spot or a tailoring bench.
- Build it, craft the multi bedding, let a colonist haul it on: the blanket is drawn over the bed.

### C — Rational Romance 2 only, no Polyamory Beds

The case that was quietly broken before this port: Primitive Workbenches kept its own 3-person
bed, this mod added nothing to it, and the result was a 3-person bed with no bedding comp at all.

- **`3-person hay bed` exists, exactly once.**
- It has the Bedding tab and accepts the multi bedding, exactly as in scenario B. If the bed is
  there but has no Bedding tab, the remove-and-redeclare did not fire.

Psychology (unofficial) can stand in for Rational Romance 2; Primitive Workbenches checks for
either.

### D — Polyamory Beds **and** Rational Romance 2

The duplicate. Before this port, Primitive Workbenches kept its copy and this mod declared a
second under the same `defName`; the later one silently won, and the game logged an error.

- **`Adding duplicate` must not appear.** The single most important line in this file.
- One `3-person hay bed` in the menu, not two.
- Its stats are this mod's, not Primitive Workbenches': low rest effectiveness and comfort, no
  sleeper body drawn, Bedding tab present. A bed with high rest and no Bedding tab means
  Primitive Workbenches' copy survived.

### E — Biotech **on**

- `primitive hay crib` exists, 1×1, with the Bedding tab, costing 3 wood and 50 hay — no haycloth.
- `simple crib bedding set` is craftable and can be hauled onto it.
- No `Patch operation … failed` from this mod.

---

## Cross-cutting checks, in any one scenario

**The leather beds are gone — all three.** `leather bed`, `double leather bed` and `3-person
leather bed` must be absent from the Furniture tab. The third is new to this port: the original
removed the first two and left it standing, so a colony that had lost leather beds could still
build one of them, out of leather, with no bedding tab and no blanket.

**The hay beds are dressed, not comfortable on their own.** `hay bed` and `double hay bed`:
Bedding tab present, storage filter on Textiles, cost in wood and hay with **no haycloth**,
description reading *"A bed made from hay. It's soft…ish and kinda pokey."* Craft a `simple
bedding` or a `large simple bedding` out of any leather or cloth, haul it on, and the blanket is
drawn over the sleeper.

**The textures are this mod's, not Primitive Workbenches'.** The hay beds should show Phaneron's
frame-only art — a bare frame with the bedding drawn on top — rather than a frame with bedding
already painted into it. Both mods ship files at the same paths and the last loaded wins, which is
why this mod loads after.

**French.** Switch the language and check a few: `literie simple`, `grande literie simple`,
`literie simple de berceau`, and in scenarios B–D `lit de paille pour trois` and `literie simple
pour trois`. The beds themselves stay in English on purpose — those labels belong to Primitive
Workbenches, which ships no French, and a French description under an English label reads worse
than both in English.

---

## What the run does **not** need to re-prove

Settled without the game, and not worth eyes on:

- Every element maps to a real 1.6 field — `Check-XmlFields.ps1`, including the patch values
  pulled out into a scratch `Defs` file, since that checker skips patch files.
- Every def and every `ParentName` resolves — `Check-DefRefs.ps1`, with Primitive Core passed in.
  `VBY_PrimitiveProduction` lives there, not in Primitive Workbenches; without it the checker
  reports a false missing reference.
- All 18 translation keys land on something — `Check-DefInjected.ps1`.
- No unguarded reference to a third-party class — `Check-TypeRefs.ps1`.

## Known and accepted

- **The crib blanket and `simple crib bedding set` are not gated**, although Primitive Workbenches
  may delete the crib. No guard reproduces its condition exactly, and a wrong guard would leave
  the crib pointing at defs that do not exist — worse than a spare craftable item. In
  `ATTRIBUTION.md`.
- **Four `linkableFacilities` entries could not be checked**: they name 【ZP】Rice cultivating
  civilization, which is not installed here. They come unchanged from the list Primitive
  Workbenches carries on its own beds — inherited, not introduced. If that mod is ever installed,
  re-read the log for `Could not resolve cross-reference`.
