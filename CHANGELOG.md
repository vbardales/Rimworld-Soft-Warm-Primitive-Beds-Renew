# Changelog

All notable changes to this mod are documented here.

## [Unreleased]

### Fixed

- **All six references into Soft Warm Beds are now guarded by `MayRequire="Mlie.JPTSoftWarmBeds"`** —
  four `<li Class="SoftWarmBeds.CompProperties_MakeableBed">` and two
  `<li>SoftWarmBeds.ITab_Bedding</li>`. The mod declares Soft Warm Beds in `modDependencies`, but
  that is a warning dialog, not a lock: RimWorld lets a player start with a missing dependency
  anyway, and the two `inspectorTabs` entries were the ones that mattered. A type name that
  resolves to nothing goes through `ParseHelper.ParseType`, which logs and returns **null**, and
  `InspectTabManager.GetSharedInstance` has no null guard — `Dictionary.TryGetValue(null)` throws
  inside `ThingDef.ResolveReferences`. `DefDatabase.ResolveAllReferences` catches it per def, so
  the load does not abort, but the def is abandoned early in its resolution, and the defs in
  question are **Primitive Workbenches'** `VBY_Haybed`, `VBY_DoubleHaybed` and `VBY_HayCrib` —
  not this mod's. A compat patch has no business damaging one of the two mods it joins.

  The four `comps` entries are worse, not milder. In 1.6 a def whose type is registered is built
  by `DirectXmlToObjectNew`, and there an unresolvable `Class=` does not degrade:
  `ResolveTypeForNode` throws an `ArgumentException` and the def is lost outright. The old
  `DirectXmlToObject.ClassTypeOf` path — which logs and falls back to the declared field type,
  leaving an inert `CompProperties` — still exists but no longer loads defs. So all six lines
  could take a Primitive Workbenches bed out of the game, by two different routes.

  `MayRequire` on a list element is read, unlike `MayRequire` on an `<Operation>`:
  `DirectXmlToObject.ListFromXml` checks it on every `<li>` right after `ValidateListNode` and
  calls `ModLister.AllModsActiveNoSuffix`. It is not, however, spell-checked for you:
  `DirectXmlCrossRefLoader.MistypedMayRequire` opens with `if (Application.isEditor)`, so a
  mistyped packageId is reported in Ludeon's Unity editor and nowhere else. `Mlie.JPTSoftWarmBeds`
  was read back against the subscribed mod's own `About.xml` by hand.

## [1.0.0] — 2026-09-07

First release. Port of cyanobot's **Soft Warm Primitive Beds** to RimWorld 1.6, with graphics by
Phaneron.

### Fixed

- **The Primitive Workbenches dependency pointed at nothing.** `About.xml` declared
  `PrimitiveProduction.velcroboy333`, which stopped at 1.5. The mod was taken over as *Primitive
  Workbenches (Continued)* under a new packageId; `zal.primitiveproduction` replaces it in
  `modDependencies` and in `loadAfter`, and the dead id is kept in `loadAfter` only. Every defName
  and every sub-element the patches reach for was checked against the continuation's `1.6/Defs`
  one at a time.
- **`VBY_TripleHayBed` was declared twice.** Primitive Workbenches ships that def and removes it
  again unless Psychology or Rational Romance 2 is loaded — its guard does not know about
  Polyamory Beds, which is why cyanobot reinstated the bed. But when one of those two *is* loaded
  the bed survived and this mod declared it a second time under the same defName;
  `DefDatabase.Add` logs an error, drops the def it already had and keeps the newer one, so it
  worked by luck of load order and wrote a red line every startup. `Patches/triplebeds.xml` now
  removes whatever copy is present (`<success>Always</success>`) and declares exactly one.
- **`VBY_TripleLeatherBed` survived the leather bed removal.** The mod deletes leather beds so
  their look can come back as bedding on a hay bed; the 3-person one was left standing, buildable
  out of leather with no bedding tab and no blanket. It joins the union in
  `Patches/leatherbeds_removal.xml`.
- **Eight crib operations fired at a def that was not there.** Primitive Workbenches deletes
  `VBY_HayCrib` unless Biotech or Children, school and learning is loaded. An xpath that matches
  nothing does not fail quietly — the operation returns false and the loader writes a red line —
  so a colony with neither mod took eight of them at every startup. The crib block is now wrapped
  in a `PatchOperationConditional` on the crib's existence.
- **`Table_LightEndTable` no longer exists.** Vanilla Furniture Expanded dropped it in 1.6, and
  Primitive Workbenches commented the same line out of its own beds. `MayRequire` checks that the
  mod is loaded, not that the def exists, so with Vanilla Furniture Expanded present the reference
  resolved to nothing and was logged every startup. Removed from the 3-person bed's
  `linkableFacilities`.

### Changed

- **`PatchOperationFindMod` replaced by `MayRequireAnyOf`.** The 3-person bed block was gated on
  the string `"Polyamory Beds (Vanilla Edition)"`, which `ModLister.HasActiveModWithName` matches
  against a mod's **display name** — one rename away from silently matching nothing. It is now
  `MayRequireAnyOf="Meltup.PolyamoryBeds.Vanilla,community.psychology.unofficialupdate,Mlie.RationalRomance2"`
  on the `ThingDef` node, which also widens the gate to the two mods Primitive Workbenches itself
  checks for. The attribute is on the def and not on the `<Operation>` deliberately: RimWorld
  reads it in `LoadedModManager.ParseAndProcessXML` after all patches have run, and never reads it
  on an operation at all.
- **`VBY_TripleHayBed` inherits from `ArtableBedBase`** rather than `BedWithQualityBase`. The old
  parent was the whole truth when this def only ever existed in place of one Primitive Workbenches
  had deleted; now that it also replaces a copy that survived, it would have quietly taken the art
  comp off a bed that had it. `ArtableBedBase` is what Primitive Workbenches gives its own
  3-person bed and the single and double hay beds this mod leaves alone.
- **`VBY_TripleHayBed` dropped out of the shared xpaths in `Patches/haybeds.xml`.** Whether that
  bed exists depends on the modlist, and whether the shared operations reach it depended on which
  patch file RimWorld read first — file order inside a mod's `Patches` folder is whatever
  `DirectoryInfo.GetFiles` returns, filled in by a pool of threads, and nothing promises it.
  `triplebeds.xml` now owns that bed outright.
- **`CYB_MultiTribalBedding` and `CYB_BedHay3Blanket` moved** from inside `Patches/triplebeds.xml`
  to `Defs/triplebeds.xml`, under the same three packageIds as the bed that names them. They are
  plain defs and nothing patches them; the identical gate is what keeps the bed's `blanketDef` and
  `beddingDef` from pointing at a def that was held back.

### Added

- French translation: 18 keys over the six beddings and blankets this mod defines and the
  3-person hay bed it declares. The hay bed, double hay bed and hay crib are deliberately left
  untranslated — those labels belong to Primitive Workbenches, which ships no French, and a French
  description under an English label reads worse than both in English.
- `<incompatibleWith>cyanobot.softwarmprimitivebeds</incompatibleWith>`, so the port and the
  original cannot run together.

### Unchanged

Every stat, cost, work amount, recipe, stuff category, graphic offset and draw size, all 23
textures, and every `defName`. The `CYB_` prefix is kept.
