# Soft Warm Primitive Beds Renew

Port of **cyanobot's Soft Warm Primitive Beds** to RimWorld 1.6, with graphics by **Phaneron**.

**I am not the author of this mod.** The defs, the balance and the artwork are theirs — all I did
was the work needed to make it run on 1.6, repoint it at the version of Primitive Workbenches that
is still alive, fix what that turned up, and write the French. Credit goes to them; mistakes in the
port are mine.

Original mod: https://steamcommunity.com/sharedfiles/filedetails/?id=3449202390 — declares 1.5 and
nothing further. The page is still online; the mod is abandoned, not withdrawn.

## What the mod does

Primitive Workbenches gives a neolithic colony a hay bed, a double hay bed, a hay crib and a
leather bed. Soft Warm Beds splits every bed into a frame plus a bedding set that pawns craft,
haul and lay on it. This mod joins the two, and the join is the point: the leather beds are
deleted, and what they looked like comes back as bedding on a hay bed. A tribal colony builds one
bed and dresses it, instead of building two different ones.

Six defs of its own, two support defs for the 3-person bed, three patch files, 23 textures, no
assembly.

| What | Where it comes from |
|---|---|
| **simple bedding**, **large simple bedding**, **simple crib bedding set** | This mod. Any leather or cloth, at a tailoring bench or a crafting spot. |
| **simple multi bedding set** | This mod, with a polyamory mod loaded. |
| hay bed blankets (single, double, crib, 3-person) | This mod. Drawn over the sleeper; not selectable. |
| **hay bed**, **double hay bed**, **hay crib** | Primitive Workbenches, patched here. |
| **3-person hay bed** | Declared here — see below. |
| leather bed, double leather bed, **3-person leather bed** | Primitive Workbenches, removed here. |

The three hay beds lose most of their base rest and comfort and get it back from the bedding, gain
the Soft Warm Beds bedding tab and a textile storage filter, drop haycloth from their cost, and use
the frame-only textures Phaneron drew so a blanket can be drawn on top of them.

Available in English and French.

Content mod: removing it mid-save destroys anything already built from it, and takes the bedding
tab off beds that had it.

## Requirements

| Mod | packageId | Why |
|---|---|---|
| [[JPT] Soft Warm Beds (Continued)](https://steamcommunity.com/sharedfiles/filedetails/?id=3006469889) | `Mlie.JPTSoftWarmBeds` | The bedding system itself. |
| [Primitive Workbenches (Continued)](https://steamcommunity.com/sharedfiles/filedetails/?id=3751287735) | `zal.primitiveproduction` | The beds this mod patches. |

Optional, and all this mod cares about is whether one of them is loaded: **Polyamory Beds (Vanilla
Edition)**, **Psychology (unofficial)** or **Rational Romance 2**. Any one of them turns on the
3-person hay bed and its bedding.

## What changed in the 1.6 port

### The dependency pointed at nothing

The original declared:

```xml
<packageId>PrimitiveProduction.velcroboy333</packageId>
```

That mod stopped at 1.5. It was taken over as **Primitive Workbenches (Continued)** under a new
packageId, `zal.primitiveproduction`, so the declared dependency named a mod that no longer exists
in any 1.6 modlist. It now names the continuation.

The continuation kept every defName the patches aim at — `VBY_Haybed`, `VBY_DoubleHaybed`,
`VBY_HayCrib`, `VBY_Haycloth`, `VBY_TripleHayBed`, `VBY_Leatherbed`, `VBY_DoubleLeatherbed`,
`VBY_TripleLeatherBed` — and every sub-element the xpaths reach for is still where it was:
`description`, `costList/Hay`, `costList/VBY_Haycloth`, `comps`, `graphicData/drawSize`,
`statBases/BedRestEffectiveness`, `statBases/Comfort`, `building/bed_showSleeperBody`,
`uiIconScale`, `size`. Checked one by one against its `1.6/Defs`, not assumed.

### The 3-person hay bed was declared twice

`Patches/triplebeds.xml` reinstated `VBY_TripleHayBed`, with this note from cyanobot:

> have to reinstate since Primitive Workbenches doesn't recognise Polyamory Beds

Half of that is still true, and it is the half that decided the fix.

**Still true.** Primitive Workbenches ships `VBY_TripleHayBed` in its own `Defs` and then deletes
it again, in `PolyBedCribNoMatch.xml`, unless **Psychology** or **Rational Romance 2** is loaded.
Polyamory Beds is not on that list and never was. So with Polyamory Beds alone the bed really is
gone by the time this mod's patches run, and something does have to put it back.

**No longer true.** When Psychology or Rational Romance 2 *is* loaded, the bed survives — and the
original then declared a second `ThingDef` with the same `defName`. RimWorld does not refuse that.
`DefDatabase.Add` logs one error, drops the def it already had, and keeps the newer one. It worked
by luck of load order, and it wrote a red line every startup while doing so.

So the answer was neither "drop the copy" nor "always override". It is:

```xml
<Operation Class="PatchOperationRemove">
	<success>Always</success>
	<xpath>Defs/ThingDef[defName="VBY_TripleHayBed"]</xpath>
</Operation>
<!-- then add exactly one, gated on the three mods that put a third pawn in a bed -->
```

Remove whatever copy is there, then declare exactly one — and declare it whenever **any** of the
three mods that put a third pawn in a bed is loaded, which is what "Polyamory Beds is not
recognised" was reaching for in the first place.

Comparing the two definitions field by field is what showed the override was worth keeping rather
than dropping. cyanobot's is the Soft Warm Beds version of the bed and Primitive Workbenches' is
not: base rest 0.50 against 0.96, comfort 0.20 against 0.72, no sleeper body drawn, a textile
storage filter, a bedding tab, a `CompProperties_MakeableBed` naming its blanket and its bedding,
its own icon, and a cost in wood and hay rather than in haycloth. Dropping it would have left a
3-person bed that behaved like nothing else in the mod. Only one field was changed: it now inherits
from `ArtableBedBase` rather than `BedWithQualityBase`. That was safe when the def only ever existed
in place of one Primitive Workbenches had deleted; now that it also replaces a copy that survived,
the old parent would have quietly taken the art comp off a bed that had it. `ArtableBedBase` is what
Primitive Workbenches gives its own 3-person bed and what it gives the single and double hay beds
this mod leaves alone.

### `PatchOperationFindMod` matched a display name

The reinstatement was wrapped in:

```xml
<Operation Class="PatchOperationFindMod">
	<mods><li>Polyamory Beds (Vanilla Edition)</li></mods>
```

`PatchOperationFindMod` calls `ModLister.HasActiveModWithName` — it compares the string against a
mod's **display name**, which is the one thing a continuation is most likely to change. One rename
and the block silently matches nothing, with no error to say so. Polyamory Beds is alive in 1.6 and
still carries that exact name, so nothing was broken yet; it was one rename away from being broken
in a way nobody would notice.

It is now `MayRequireAnyOf` on the def node, taking packageIds — what a mod is actually identified
by:

```xml
<ThingDef ParentName="ArtableBedBase"
          MayRequireAnyOf="Meltup.PolyamoryBeds.Vanilla,community.psychology.unofficialupdate,Mlie.RationalRomance2">
```

**The attribute has to sit on the `ThingDef`, not on the `Operation`.** That is worth stating
plainly, because the wrong form looks right and does nothing. RimWorld reads `MayRequire` /
`MayRequireAnyOf` in `LoadedModManager.ParseAndProcessXML`, walking the top-level nodes of the
combined document *after* every patch has been applied — which is exactly why it works on a def a
patch has just added. `ModContentPack.LoadPatches`, which builds the operations, never looks at the
attribute at all: there is no `mayRequire` field on `Verse.PatchOperation`, and no string
`"MayRequire"` in any method that loads one. `MayRequire` on an `<Operation>` element is read by
nothing, and the operation runs regardless. (`PatchOperationFindModById`, which would take
packageIds directly, is not a vanilla operation — it comes from a framework mod.)

### The 3-person leather bed was left standing

`Patches/leatherbeds_removal.xml` removed `VBY_Leatherbed` and `VBY_DoubleLeatherbed` and stopped
there. `VBY_TripleLeatherBed` is a leather bed by the same definition, and it survived — so a
colony that had removed leather beds could still build one of them: out of leather, with no bedding
tab and no blanket, next to a hay bed that had both. It is now removed with its two siblings, in the
same union, which also keeps the operation from failing when Primitive Workbenches has already
deleted the 3-person beds itself.

### Eight patch operations fired at a def that was not there

Primitive Workbenches deletes `VBY_HayCrib` unless **Biotech** or **Children, school and learning**
is loaded. Without either, all eight of this mod's crib operations aimed at a def that was no longer
in the document — and an xpath that matches nothing does not fail quietly: the operation returns
false and the loader writes a red line for it. Eight of them, every startup, on any colony without
those two mods.

They are now inside a `PatchOperationConditional` on the crib's existence. When the crib is gone the
whole block is a no-op, which is what it always meant. (`PatchOperationConditional` with a `match`
and no `nomatch` returns true when the xpath finds nothing — checked in the 1.6 assembly, not
assumed, because the whole point was to stop writing error lines.)

### One dead facility reference

The 3-person bed offered to link to `Table_LightEndTable`. Vanilla Furniture Expanded dropped that
def in 1.6, and Primitive Workbenches commented the same line out of its own beds for the same
reason. `MayRequire="VanillaExpanded.VFECore"` is not a guard against this — it checks that the mod
is *loaded*, not that the def *exists* — so with Vanilla Furniture Expanded present, which is the
common case, the reference resolved to nothing and RimWorld logged it every startup. The line is
gone; the 42 facilities left in that list were checked one by one against the mods that own them,
and all 38 that could be checked resolve.

### Nothing guarded the references into Soft Warm Beds

Six references — four `<li Class="SoftWarmBeds.CompProperties_MakeableBed">` and two
`<li>SoftWarmBeds.ITab_Bedding</li>` — carried no `MayRequire`, and a `modDependencies` entry is
not a substitute: RimWorld shows a dialog for a missing dependency and lets the player start
anyway.

The `inspectorTabs` pair is the shape a `Class="..."` audit structurally cannot see: a bare
`List<Type>` whose elements are type names in the element *text*. `ParseHelper.ParseType` logs
and returns **null**, `InspectTabManager.GetSharedInstance` has no null guard, and the resulting
throw inside `ThingDef.ResolveReferences` abandons the def early. The four `comps` entries are
worse rather than milder: in 1.6 a registered def type is built by `DirectXmlToObjectNew`, where
an unresolvable `Class=` throws and the def is lost outright.

Either way the defs are *Primitive Workbenches'* `VBY_Haybed`, `VBY_DoubleHaybed` and
`VBY_HayCrib`, not this mod's — which is what made it worth fixing. All six now carry
`MayRequire="Mlie.JPTSoftWarmBeds"`. Unlike the operation-level form discussed above,
`MayRequire` on a list element *is* read — `ListFromXml` checks it on every `<li>` — but it is
not spell-checked outside Ludeon's editor, so the packageId was verified by hand.

### The 3-person bed moved out of the shared xpaths

`Patches/haybeds.xml` listed `VBY_TripleHayBed` in every one of its shared unions — the sleep stats,
the sleeper body, the bedding tab, the storage filter. That is now `triplebeds.xml`'s business
alone, because whether the bed exists depends on the modlist and whether the shared operations reach
it first depends on which file RimWorld happens to read first. Patch file order inside a mod is
whatever `DirectoryInfo.GetFiles` hands back, filled in by a pool of threads; it is not part of the
API and nothing promises it. One file owning that bed outright costs four lines and removes the
question.

## What did not change

The stats, the costs, the work amounts, the recipes, the stuff categories, the graphics offsets, the
draw sizes, the 23 textures and every `defName`. `CYB_MultiTribalBedding` and `CYB_BedHay3Blanket`
moved out of the patch file into `Defs/triplebeds.xml`, gated by the same three packageIds as the
bed that names them — they are plain defs and nothing patches them, and an identical gate is what
keeps the bed's `blanketDef` and `beddingDef` from ever pointing at a def that was held back.

The `CYB_` prefix is kept: it is a clean prefix, it collides with nothing, and renaming would take
every bedding set already crafted out of every existing save.

Beds under Soft Warm Beds lose their **art** tab, because the framework adds `inspectorTabs` to
vanilla `Bed`, `DoubleBed`, `HospitalBed` and `RoyalBed` in exactly the same way. That is Soft Warm
Beds' own behaviour, not this mod's, and it was left alone rather than "fixed" here — a primitive
bed that behaved differently from every other bed in the game would be the bug.

## Checks run

- `scripts/Check-XmlFields.ps1` against the 1.6 assembly plus `SoftWarmBeds.dll` — clean. It skips
  patch files, which is most of this mod, so the `<value>` fragments were pulled out into a scratch
  `Defs` file and run through it separately; also clean, and confirmed meaningful by running it
  again *without* `SoftWarmBeds.dll`, where `blanketDef` and `beddingDef` correctly come back as
  unknown.
- `scripts/Check-DefRefs.ps1` with Soft Warm Beds, Primitive Workbenches, Vanilla Furniture Expanded
  and Polyamory Beds as `-AlsoScan` — every reference resolves, every `ParentName` resolves, every
  XML file well-formed.
- `linkableFacilities` audited by hand against the 9 664 installed Workshop mods, matching each
  entry to the packageId its `MayRequire` names. One dead reference found, listed above; four
  entries belong to a mod that is not installed and could not be verified — they are the same four
  Primitive Workbenches still carries on its own beds.
- `scripts/Check-DefInjected.ps1` — 18 keys, 0 errors.
- **The patch pipeline was replayed** outside the game: Primitive Workbenches' own patches, then
  this mod's, against a combined document, once for each modlist that changes the answer. Before
  and after, on the four that matter:

  | modlist | original | this port |
  |---|---|---|
  | nothing | no 3-person bed; **2 failing operations** at the crib | no 3-person bed, no failures |
  | Polyamory Beds | one bed, with its bedding | same |
  | Rational Romance 2 | one bed, **no bedding comp at all** | one bed, with its bedding |
  | both | **`VBY_TripleHayBed` declared twice** | one bed, with its bedding |

  `VBY_TripleLeatherBed` survives all four in the original and none of them here. No operation of
  this mod's fails in any of the four.

## Credit and removal

cyanobot declared no licence: no file in the mod, nothing in its `About.xml`, no linked repository,
and nothing in the body of the description on its Steam page. It is republished here under the usual
convention for abandoned mods — full credit, a link to the original, and removal on request without
argument. The same goes for Phaneron, whose graphics this mod carries.

The original defNames are kept, so a save moves between the two mods without losing a bed. The two
cannot run together: `cyanobot.softwarmprimitivebeds` is declared in `<incompatibleWith>`.

See [ATTRIBUTION.md](ATTRIBUTION.md) for the full account and [CHANGELOG.md](CHANGELOG.md) for the
list of changes.

## Adoption

If I do not answer within a reasonable time after being contacted, anyone may freely update this or
any other of my mods, including publishing a continuation of it. All credit must be preserved.
