# Soft Warm Primitive Beds Renew — attribution

A 1.6 port of **Soft Warm Primitive Beds**, by **cyanobot**
([3449202390](https://steamcommunity.com/sharedfiles/filedetails/?id=3449202390)), whose bed and
blanket graphics are **Phaneron's**, from
[Primitive Workbenches Retexture](https://steamcommunity.com/sharedfiles/filedetails/?id=3038407835).

## Status: public

The source mod is **dead** — it declares 1.5 and nothing further — and **no licence is declared
anywhere**, checked at the four places one could be, plus a fifth this mod happened to have:

- No `LICENSE` file in the mod. Its whole tree is `About/`, `Defs/`, `Patches/`, `Textures/`.
- Nothing in `About.xml` — no licence element, and no mention in the body of `<description>`.
- No linked repository: `<url>` is absent entirely.
- Nothing in the body of the description on its Steam page. That is the check that matters: it is
  the one that was skipped once on たたら製鉄, whose ban on redistribution turned out to be a
  sentence in its description and nowhere else. Read in full here; it credits jptrr, Mlie,
  velcroboy3333 and Phaneron, and says nothing about reuse either way.
- The page links cyanobot's own site (`cyanobotcodes.tumblr.com`), which some authors use to state
  a modding policy. Fetched; it contains no statement about licences, permission, redistribution or
  continuations.

This is the usual convention for ports on the RimWorld Workshop: republished with **credit by
name** and **removal on request, without argument**. The `<author>` field reads
`cyanobot, graphics by Phaneron - 1.6 adapted by Nelim`, and the removal clause is in the description.

### Phaneron's graphics, and the word "permission"

cyanobot's Steam page says the bed and blanket graphics are Phaneron's, *"used with permission"*.
That permission was given to cyanobot, and it is not transitive: it says nothing about this port.
Phaneron's own mod declares no licence either — no file, nothing in its `About.xml`, nothing on its
Steam page, which is plain text about what the retexture covers and how to load it.

So the same convention covers both, and it is stated in both directions: Phaneron is credited by
name in `<author>`, in the description, in the README and here, and the removal offer is theirs to
take up as much as cyanobot's. If either would rather this port did not exist, it comes down.

One detail worth recording, because it decides who to credit for what: Phaneron's retexture page
lists **"beds for 3 pawns"** among the things it does *not* retexture. The `HDM_BedHay3_*` textures
this mod ships are therefore not Phaneron's — they are cyanobot's own, which matches the page's
"Some of them are edited by me", and they differ byte for byte from the ones Primitive Workbenches
ships under the same names.

## What was carried over

Everything the mod defined. Nine `ThingDef`s, three patch files, 23 textures, no C#, no research,
no recipe of its own beyond the `recipeMaker` on the beddings.

| defName | label | what it is |
|---|---|---|
| `CYB_SingleTribalBedding` | simple bedding | Item, tailoring bench / crafting spot |
| `CYB_DoubleTribalBedding` | large simple bedding | Item |
| `CYB_CribTribalBedding` | simple crib bedding set | Item |
| `CYB_MultiTribalBedding` | simple multi bedding set | Item, gated on a polyamory mod |
| `CYB_HayBedBlanket` | hay bed blanket | Drawn over the sleeper, not selectable |
| `CYB_DoubleHayBedBlanket` | hay bed blanket | Ditto |
| `CYB_HayCribBlanket` | hay crib blanket | Ditto |
| `CYB_BedHay3Blanket` | hay bed blanket | Ditto, gated on a polyamory mod |
| `VBY_TripleHayBed` | 3-person hay bed | Building, gated on a polyamory mod — see below |

The stats, the costs, the work amounts, the stuff categories, the recipe users, the graphic offsets
and the draw sizes are cyanobot's, unchanged. So are the 23 textures, byte for byte.

**`About/Preview.png` is NOT his**, and this section said the opposite until 2026-09-11. His was
copied over when this port was first assembled, and has since been dropped: a port credits its
author, it does not borrow his shop front. His preview also carried a `1.5` ribbon in the corner,
which could not simply be repainted to `1.6` — the version suffix was retired from this
repository's names that same day, on the grounds that a number dates a mod and ages badly, and an
engraved image is the hardest place to correct it. A new one is being made; the brief for it lives
in `PROMPT_SOFTWARMPRIMITIVEBEDSRENEW.md` at the repository root until it exists.

`About/PublishedFileId.txt` was not carried over, for the obvious reason: it names cyanobot's
Workshop item.

## The `CYB_` prefix was kept

Eight of the nine defs carry it, it is distinctive, and it collides with nothing — the ninth,
`VBY_TripleHayBed`, is Primitive Workbenches' own name and has to stay that name for the def to
replace the one this mod removes. A rename is permanent in a way a port is not: it would take every
bedding set already crafted out of every existing save, and it would buy nothing.

## The dependency that had moved

The original declared two:

| packageId | versions | state |
|---|---|---|
| `Mlie.JPTSoftWarmBeds` | 1.1 – 1.6 | alive, unchanged |
| `PrimitiveProduction.velcroboy333` | 1.2 – 1.5 | **dead** |

The second was taken over as **Primitive Workbenches (Continued)**, packageId
`zal.primitiveproduction`, Workshop id
[3751287735](https://steamcommunity.com/sharedfiles/filedetails/?id=3751287735), declaring 1.6. A
`modDependencies` entry names a mod by packageId and nothing else, so the declared dependency named
a mod no 1.6 modlist can contain. It now names the continuation; the dead id is kept in `loadAfter`,
where an id that matches nothing is harmless.

The continuation kept every defName the patches target, and every sub-element the xpaths reach for.
Confirmed one at a time against its `1.6/Defs/ThingDefs_Buildings/PrimitiveBeds.xml` rather than
assumed:

| xpath target | present |
|---|---|
| `VBY_Haybed` — `description`, `costList/VBY_Haycloth`, `costList/Hay`, `comps`, `graphicData/drawSize`, `statBases/BedRestEffectiveness`, `statBases/Comfort`, `building/bed_showSleeperBody` | yes |
| `VBY_DoubleHaybed` — same set | yes |
| `VBY_HayCrib` — the above plus `costList`, `uiIconScale`, `size` | yes, but conditionally removed |
| `VBY_TripleHayBed` | yes, but conditionally removed |
| `VBY_Leatherbed`, `VBY_DoubleLeatherbed` | yes |
| `VBY_TripleLeatherBed` | yes, but conditionally removed |

## The def that was declared twice

`Patches/triplebeds.xml` reinstated `VBY_TripleHayBed`, with cyanobot's note:

> have to reinstate since Primitive Workbenches doesn't recognise Polyamory Beds

The interesting part is that the note is **still half true**, and that is what made both obvious
answers wrong.

`PolyBedCribNoMatch.xml`, in the continuation, is a `PatchOperationFindMod` with a `nomatch` branch
that removes `VBY_TripleHayBed` and `VBY_TripleLeatherBed` unless one of two mods is loaded:
**Psychology (unofficial)** or **Rational Romance 2**. Polyamory Beds is not on that list. So:

| modlist | Primitive Workbenches' copy | what the original did | what happened |
|---|---|---|---|
| no polyamory mod | removed | nothing (FindMod did not match) | no 3-person bed — correct |
| Polyamory Beds only | removed | reinstated it | one bed — correct, and this is the case the note describes |
| Psychology or RR2 only | kept | nothing | a 3-person bed with no bedding comp, no blanket, no bedding tab |
| Polyamory Beds **and** one of those two | kept | reinstated it **again** | **two defs, same defName** |

That last row is the fault. RimWorld does not refuse a duplicate defName: `DefDatabase.Add` logs
one error, drops the def it already holds, and keeps the newer one. Which of the two is "newer"
comes down to document order in the combined XML, so it worked, silently, by luck — and wrote a red
line every startup while doing so.

Comparing the two definitions field by field is what decided the fix, because it showed they are
not two versions of the same bed. They are the same bed before and after this mod:

| field | Primitive Workbenches | cyanobot's copy |
|---|---|---|
| `BedRestEffectiveness` | 0.96 | 0.50 |
| `Comfort` | 0.72 | 0.20 |
| `BedStuffEffectMultiplierInsulation_Cold` | — | 0.2 |
| `BirthRitualQualityOffset` | — | 1 |
| `bed_showSleeperBody` | true | false |
| storage settings | — | textiles, fixed and default |
| `inspectorTabs` | — | `SoftWarmBeds.ITab_Bedding` |
| `CompProperties_MakeableBed` | — | blanket + bedding |
| `linkableFacilities` | 2 entries | 43 entries |
| `costList` | 8 wood, 27 haycloth, 75 hay | 3 wood, 100 hay |
| `uiIconPath` | — | `HDM_BedHay3_south` |
| `drawSize` | (3.25,2.25) | (3.5,2.5) |
| `ParentName` | `ArtableBedBase` | `BedWithQualityBase` |

Every difference in that table except the last is this mod doing its job: under Soft Warm Beds the
frame is not supposed to be comfortable, the bedding is. Dropping cyanobot's copy would have left a
3-person bed that behaved like nothing else in the mod. **Keeping it as a deliberate override is
the right answer, and the fix is to make it actually be one:**

```xml
<Operation Class="PatchOperationRemove">
	<success>Always</success>
	<xpath>Defs/ThingDef[defName="VBY_TripleHayBed"]</xpath>
</Operation>
```

`<success>Always</success>` is what makes that safe in the rows where the def was already gone: a
`PatchOperationRemove` whose xpath matches nothing returns false, and the loader writes an error
for it.

The one field that did change is `ParentName`. `BedWithQualityBase` was the whole truth when this
def only ever existed in place of one Primitive Workbenches had deleted. Now that it also replaces
a copy that survived, the old parent would quietly take the art comp off a bed that had it.
`ArtableBedBase` is what Primitive Workbenches gives its own 3-person bed, and what it gives the
single and double hay beds this mod leaves alone. Nothing else about the def moved.

The gate widened at the same time, from Polyamory Beds to any of the three mods that put a third
pawn in a bed, so the third row of that table — a 3-person bed with none of this mod's work on it —
stops happening too.

## Why `MayRequire` is on the def and not on the operation

This is the part that looks like a style choice and is not.

`PatchOperationFindMod` calls `ModLister.HasActiveModWithName`. It matches a **display name**,
which is the one string a continuation reliably changes. Polyamory Beds is alive in 1.6 and still
carries the exact name the original typed, so nothing was broken yet — it was one rename away from
matching nothing, with no error to say so. That has bitten this repository before, on Fullzoon's
Cookies.

The replacement had to be packageId-based, and there is exactly one form of that which works.
Verified against `Assembly-CSharp.dll` rather than taken on trust, because the wrong form looks
right and does nothing:

- **`MayRequire` on a def node works, including on a def a patch has just added.**
  `LoadedModManager.ParseAndProcessXML` reads `MayRequire` and `MayRequireAnyOf` off each top-level
  node of the combined document and calls `ModLister.AllModsActiveNoSuffix` /
  `AnyModActiveNoSuffix` before `DirectXmlLoader.DefFromNode`. Patches are applied to that document
  before this runs, so a `<ThingDef MayRequireAnyOf="…">` inside a `PatchOperationAdd`'s `<value>`
  is gated exactly like one written in `Defs/`.
- **`MayRequire` on an `<Operation>` element is read by nothing.** `Verse.PatchOperation` has three
  fields — `sourceFile`, `neverSucceeded`, `success` — and no `mayRequire`. `ModContentPack.
  LoadPatches`, which turns the XML into operations, contains no reference to either attribute
  name; the only `MayRequire` handling inside `DirectXmlToObject.ObjectFromXml` sits in the field
  cross-reference path, next to `DirectXmlCrossRefLoader.RegisterObjectWantsCrossRef`, and never
  looks at the root node. An operation carrying the attribute simply runs.

  A live 1.6 mod relies on the wrong form — Polyamory Beds itself writes
  `<Operation Class="PatchOperationSequence" MayRequire="Ludeon.RimWorld.Ideology">` — which is
  precisely why this was checked in the assembly instead of copied from a mod that works.
- **`PatchOperationFindModById` is not vanilla.** It shows up in patch files across the Workshop and
  would be the obvious answer; it is not in `Assembly-CSharp.dll`, which contains sixteen
  `PatchOperation*` types and not that one. It comes from a framework mod, and depending on it here
  would add a dependency to save an attribute.

## The other four faults

**`VBY_TripleLeatherBed` survived the leather bed removal.** The premise of the mod is that leather
beds go and their look comes back as bedding on a hay bed. `Patches/leatherbeds_removal.xml` removed
`VBY_Leatherbed` and `VBY_DoubleLeatherbed` and stopped, so with Psychology or Rational Romance 2
loaded a colony could still build a 3-person leather bed — out of leather, with no bedding tab and
no blanket, next to a hay bed that had both. It joins the same union, which also keeps the operation
succeeding when the continuation has already deleted the 3-person beds itself: the single and the
double are always there to match.

**Eight crib operations fired at a def that was not there.** The same `PolyBedCribNoMatch.xml`
deletes `VBY_HayCrib` unless **Biotech** or **Children, school and learning** is loaded. Without
either, all eight of this mod's crib operations aimed at a def no longer in the document, and an
xpath that matches nothing does not fail quietly — the operation returns false and the loader writes
a red line. They are now inside a `PatchOperationConditional` on the crib's existence.

That conditional had to be checked too, since its whole purpose is to stop writing errors:
`PatchOperationConditional.ApplyWorker` returns `nomatch.Apply(xml)` when the xpath finds nothing
and `nomatch` is set, and otherwise returns `true` if `match` is non-null. A conditional with a
`match` and no `nomatch` is therefore a clean no-op, which is what this needs.

**`Table_LightEndTable` no longer exists.** The 3-person bed offered to link to it under
`MayRequire="VanillaExpanded.VFECore"`. Vanilla Furniture Expanded dropped that def in 1.6 —
present in its `1.0` folder, absent from `1.6`, which is the one `LoadFolders.xml` serves — and
Primitive Workbenches commented the identical line out of its own beds. `MayRequire` checks that
the mod is loaded, not that the def exists, so with Vanilla Furniture Expanded present the
reference resolved to nothing and RimWorld logged it every startup. Removed.

**Nothing guarded the references into Soft Warm Beds.** Found on 2026-09-10, during a
dependency audit across the whole repository. Six references — four
`<li Class="SoftWarmBeds.CompProperties_MakeableBed">` and two
`<li>SoftWarmBeds.ITab_Bedding</li>` — carried no `MayRequire`, and `modDependencies` is not a
substitute: RimWorld shows a dialog for a missing dependency and lets the player start anyway.

The two `inspectorTabs` entries are the shape a `Class="..."` audit structurally cannot see: a
bare `List<Type>` whose elements are type names in the element *text*, with no attribute and no
field name to key on. `ParseHelper.ParseType` logs and returns **null** for a type it cannot
find; `InspectTabManager.GetSharedInstance` has no null guard, so `Dictionary.TryGetValue(null)`
throws inside `ThingDef.ResolveReferences`. `DefDatabase.ResolveAllReferences` catches that per
def, so the load does not abort — but the def is abandoned early in its resolution.

The four `comps` entries are worse, not milder, and the first draft of this section had it
backwards. In 1.6 a def whose type is registered is built by `DirectXmlToObjectNew`, and there an
unresolvable `Class=` does not degrade: `ResolveTypeForNode` builds an `ArgumentException` and
throws, and the def is lost outright. The gentle path — `DirectXmlToObject.ClassTypeOf`, which
logs and falls back to the declared field type — still exists but no longer loads defs.

Both routes end in the same place, and that place is the point: the defs are **Primitive
Workbenches'** `VBY_Haybed`, `VBY_DoubleHaybed` and `VBY_HayCrib`, not this mod's. A compat patch
damaging one of the two mods it exists to join is the one outcome it must not have. That is also
the line between this and the rest of the repository: a mod naming a class from its own hard
dependency, in its own defs, breaks only itself — see the sweep note below.

`MayRequire` on a list element *is* read, unlike the operation-level form dissected above:
`ListFromXml` checks it on every `<li>` right after `ValidateListNode` and calls
`ModLister.AllModsActiveNoSuffix`. It is not spell-checked for you, though:
`DirectXmlCrossRefLoader.MistypedMayRequire` opens with `if (Application.isEditor)`, so a
mistyped packageId is reported in Ludeon's Unity editor and nowhere else — silent for every
player. `Mlie.JPTSoftWarmBeds` was read back against the subscribed mod's own `About.xml` by hand.

### The rest of the repository, swept the same way

This became `scripts/Check-TypeRefs.ps1`, the fifth checker in that folder. Every field of type
`Type` or `List<Type>` is enumerated by reflection — 77 element names, of which 9 are
`List<Type>`, once the compiler-generated names and the three vanilla declares with more than one
field type are dropped — and the 1 287 XML files of this repository were read against them. Outside
this mod, the `inspectorTabs` shape occurs in exactly two places: **Medieval Homestead's** wine
and mead barrels, `<li>PipeSystem.ITab_Processor</li>`, unguarded.

Left alone, deliberately. `PipeSystem.dll` ships **inside** Vanilla Expanded Framework, which
Medieval Homestead declares as a hard dependency; the defs are its own, not a third party's; and
the same barrels already carry `<li Class="PipeSystem.CompProperties_AdvancedResourceProcessor">`,
so without the framework the def is lost at the `Class=` step whatever the tab line says. A guard
on the tab alone would be cosmetic, and would leave a barrel that loads without its processor.

The rest of that list was audited the same way, matching each entry to the packageId its own
`MayRequire` names, across the 9 664 subscribed Workshop mods: 38 of the 42 remaining entries
resolve, and the four that cannot be checked belong to 【ZP】Rice cultivating civilization, which is
not installed here and which Primitive Workbenches still names on its own beds.

## Two structural changes, both about ordering

**`VBY_TripleHayBed` left the shared xpaths in `haybeds.xml`.** The original listed it in every one
of the shared unions — sleep stats, sleeper body, bedding tab, storage filter — while also creating
it in `triplebeds.xml`. Whether those operations reach the bed depends on which of the two files
RimWorld reads first, and file order inside a mod's `Patches` folder is whatever
`DirectXmlLoader.XmlAssetsInModFolder` gets back from `DirectoryInfo.GetFiles`, filled in by a pool
of worker threads. It is stable in practice and promised nowhere. `triplebeds.xml` now owns that bed
outright and writes its values inline; the cost is four lines of duplication and the gain is that
the result no longer depends on a filesystem detail.

**`CYB_MultiTribalBedding` and `CYB_BedHay3Blanket` moved to `Defs/triplebeds.xml`.** They were
inside the patch only because the whole block was wrapped in one `PatchOperationFindMod`. They are
plain defs, nothing patches them, and they carry the same `MayRequireAnyOf` as the bed — the same
list, deliberately, because the bed's `CompProperties_MakeableBed` names both of them and a gate
that let the bed through while holding these back would be an unresolved cross-reference at startup.

They could not both be moved and the bed with them: a def in `Defs/` would be in the combined
document *before* the removal ran, and `Defs/ThingDef[defName="VBY_TripleHayBed"]` would have taken
out our own copy along with Primitive Workbenches'. The bed stays in the patch, after the removal,
for that reason.

## What was left alone on purpose

**Beds under Soft Warm Beds lose their art tab.** `haybeds.xml` adds an `inspectorTabs` list holding
only `SoftWarmBeds.ITab_Bedding`, and list inheritance in RimWorld appends the parent's `<li>`
elements to the child's — verified in `XmlInheritance.RecursiveNodeCopyOverwriteElements`, where
`IsListElement` returns true for any node named `li` and the parent's items are imported and
appended. So `ITab_Art` from `ArtableBedBase` is kept, not replaced, and there is nothing to fix
here. Soft Warm Beds does the identical thing to vanilla `Bed`, `DoubleBed`, `HospitalBed` and
`RoyalBed`; a primitive bed that behaved differently from every other bed in the game would be the
bug.

**`CYB_HayCribBlanket` is `Graphic_Multi` with a single un-suffixed PNG.** That looks wrong and is
not. `Graphic_Multi.Init` falls back to `ContentFinder<Texture2D>.Get(path)` when none of
`_north`, `_east`, `_south`, `_west` is found, and only logs "Failed to find any textures at" if
that comes back null too.

**`Things/Beddings/FurBeddingSingle` and `FurBeddingDouble`** are Soft Warm Beds' own textures, and
in 1.6 that mod serves them from an asset bundle rather than the loose PNGs in `LegacyAssets`, which
`LoadFolders.xml` no longer maps. Both are in the bundle's manifest, so both still resolve.

**The balance.** Not one stat, cost or work amount was touched, including the places where the
3-person bed sits slightly off the line its siblings are on — `BedRestEffectiveness` 0.50 where they
get 0.45, insulation 0.2 where they get 0.3, three wood where the single costs three and the double
five. Those are cyanobot's numbers for that bed and the port's job is not to relitigate them.

**The crib's blanket and bedding are not gated**, although the crib itself may be deleted by
Primitive Workbenches. There is no gate that mirrors that condition exactly, and the two failure
modes are not symmetrical: an ungated pair leaves two unused items in a crafting menu, while a gate
that guessed wrong would leave the crib pointing at defs that do not exist.

## Notes from the port

- **`Check-XmlFields.ps1` skips patch files**, and this mod is mostly patches, so a clean run on it
  proves very little. The `<value>` fragments were pulled into a scratch `Defs` file and run
  through separately, and the run was confirmed meaningful by repeating it *without*
  `SoftWarmBeds.dll`, where `blanketDef` and `beddingDef` correctly come back as unknown fields on
  `CompProperties`. Both `SoftWarmBeds.CompProperties_MakeableBed` and `SoftWarmBeds.ITab_Bedding`
  exist in the 1.6 assembly.
- **`Check-DefInjected.ps1` cannot see a mod that lives inside `.claude/`**: it skips those paths on
  purpose, because a session worktree is a full copy of the monorepo. Run from a copy outside it,
  the answer is 18 keys and 0 errors. It is one more reason a mod does not stay in a worktree.
- **The patch pipeline was replayed outside the game**, because every fault here is conditional on
  the modlist and no single run of RimWorld would show more than one of them. Primitive
  Workbenches' patches and then this mod's, applied to a combined document by a throwaway script
  implementing `Remove`, `Replace`, `Add`, `Sequence`, `Conditional` and `FindMod` with the
  `success` field, then `MayRequire` applied to the top-level nodes the way
  `ParseAndProcessXML` does it. Four modlists, before and after:

  | modlist | original | this port |
  |---|---|---|
  | nothing | no 3-person bed; **2 failing operations** at the crib | no 3-person bed, no failures |
  | Polyamory Beds | one bed, with its bedding | same |
  | Rational Romance 2 | one bed, **no bedding comp at all** | one bed, with its bedding |
  | both | **`VBY_TripleHayBed` declared twice** | one bed, with its bedding |

  `VBY_TripleLeatherBed` survives all four in the original and none of them here, and no operation
  of this mod's fails in any of the four. The script is not shipped: it is not a patch engine, it
  is a way of asking one question, and Primitive Workbenches' own patches fail in it for want of
  Core and the rest of a modlist.
- **Soft Warm Beds' vocabulary did not shift between 1.5 and 1.6.** Diffing the two folders: two
  assemblies rebuilt, one new patch file for Loft Beds, one Vanilla Furniture Expanded patch
  changed. `BlanketBase`, `BeddingBase`, the `Beddings` category, `CompProperties_MakeableBed` and
  `ITab_Bedding` are unchanged in shape.

## Adoption

If I do not answer within a reasonable time after being contacted, anyone may freely update this or
any other of my mods, including publishing a continuation of it. All credit must be preserved.
