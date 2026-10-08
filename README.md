# Pokewilds Android Mods

Separate mod downloads and a creation template for the PokeWilds Godot Android port.

## Downloads

- [Aurora Shinies](Aurora-Shinies.zip?raw=true)
- [Level 40 Evolutions](Level-40-Evolutions.zip?raw=true)
- [Winter Pines](Winter-Pines.zip?raw=true)
- [PokeWilds Godot Mod Template](PokeWilds-Godot-Mod-Template.zip?raw=true)

Each mod ZIP contains its own separate folder. Extract that download, then import the `.pwmod` inside it. Do not import the outer distribution ZIP directly. Extract the template on your computer to create a mod.

## Mod and Template Instructions

PokeWilds mod installation and creation guide for the current Part 13 **format-1 multi-mod loader**.

The library supports multiple separate imported packages. It starts empty on a fresh installation; built-in visual/audio options are separate from the imported-mod library. The directions here and the updated template README replace the older single-import instructions.

### 1. Installing and managing mods on Android

1. Download the mod ZIP onto your device and extract its individual folder.
2. Find the `.pwmod` inside. Leave **that file** intact; do not extract it.
3. Save your current game.
4. Swipe inward from the right edge to open the swipe panel.
5. Select **MODDING**, then **IMPORT MOD**.
6. Choose the `.pwmod` file. A new mod ID returns **Installed and enabled. Restart to apply.**
7. Repeat for any other separate mods you want. Importing a different ID adds another library entry.
8. Fully close PokeWilds and reopen it. Returning to the title screen is insufficient.
9. Open **MODDING** and check each entry's **ON/OFF** state. Check **CONFLICTS** when combining packs.

Accepted files are `.pwmod`, or a compatible ZIP with `mod.json` at its root. The three distribution ZIPs above contain folders and must be extracted first. Ordinary Java mod ZIP/RAR files are not automatically compatible.

#### Library navigation

Use the **UP/DN** touch buttons, D-pad, or left stick to scroll. **A** selects and **B** goes back. Selecting a mod opens its controls: **ENABLED: ON/OFF**, **LOAD EARLIER**, **LOAD LATER**, **REMOVE MOD**, and **BACK**.

#### Enable or disable one mod

Select the mod, toggle **ENABLED: ON/OFF**, save your game, and fully restart. Disabled packages remain installed and can be enabled again without importing them.

#### Turn every mod off

Select **ALL MODS OFF**, save, and fully restart. This disables every imported package without deleting it. It restores base resources/data for those overrides, but does not undo gameplay changes already written to a save.

#### Load order and conflicts

Enabled packages load from top to bottom. **Later enabled mods win** when they change the same resource path, individual species property, or species shiny palette. Use **LOAD EARLIER** or **LOAD LATER**, then fully restart.

Unrelated properties combine. For example, one mod can change a species' stats and another its catch rate. An entire array or nested object is one property: two learnsets do not merge move by move, and two `overworld_behavior` objects do not merge individual keys. A later shiny palette replaces that species' whole palette.

**CONFLICTS** lists overlapping entries and identifies the earlier mod and the winning later mod. It detects direct overlaps, not every possible gameplay interaction. Test each pack separately and then test the combination. The three packs provided here have no overlapping keys.

#### Update a mod

Import a new package using the same stable `id` to update that mod. The loader preserves its current load-order position and enabled state. Updates return **Updated. Restart to apply.** A disabled mod stays disabled after updating. A different ID creates a separate entry. Keep backup packages outside the app if you want to roll back.

#### Remove a mod

Select the mod, choose **REMOVE MOD**, then **YES - REMOVE MOD**. Choose **NO - KEEP MOD** to cancel. Save and fully restart to apply the removal. Removing a pack does not reverse saved Pokemon or world changes; reinstalling requires a copy of its package.

#### Storage and startup

There is no fixed mod-count cap. Device storage, decoded image/audio memory, and the package limits below still apply. Invalid imports leave the existing library intact. At startup, invalid or missing enabled packages are reported and valid packages continue loading. Installation, updates, enable/disable, ordering, and removal all require a full restart to take effect.

### 2. What can be modded?

The loader has three supported sections:


| Section | What it changes |
| --- | --- |
| resources | Existing, approved PNG artwork and OGG audio |
| species | Data belonging to existing Pokémon records |
| palettes | Existing Pokémon’s two-color shiny replacements |




#### Artwork and audio

You can replace destinations listed in asset-catalog.json, including catalog-listed:

- Pokémon overworld, front, back and shiny sprites.
- Player-character artwork.
- Terrain, trees, furniture and other world artwork.
- Existing battle-effect sprite sheets.
- Menu backgrounds, frame artwork and emotes.
- Music tracks.
- Pokémon cries.
- Existing sound effects.
The catalog is the authority. An asset category being supported does not mean every recently added file is available. Unlisted destinations are rejected.

Replacing artwork does not change its gameplay rules. For example:

- A new bridge image does not change bridge placement or collision.
- A new animation sheet does not define new attack effects or timing.
- Replacing character artwork does not add another character-selection slot.
- A new menu image cannot add buttons or options.

#### Existing Pokémon data

Supported patches include existing fields for:

- Display names.
- Types and six base stats.
- Catch rate, base experience and growth rate.
- Height, weight and Pokédex information.
- Level-up learnsets.
- Evolution rules.
- Egg groups, egg moves, egg cycles and breeding base forms.
- Habitat, harvest and spawning tokens.
- Special/disabled field skills.
- Existing overworld-behavior settings.
- Artwork and cry references.
- Other existing fields with the expected data type.
Important: Passing validation establishes that a patch fits the loader’s format. It does not prove every value is meaningful or that a particular gameplay system uses that field.


#### Not supported by this loader

- New scripts or executable code.
- Entirely new Pokémon IDs.
- New moves or new move-effect logic.
- New battle systems or abilities.
- New building recipes or collision systems.
- New menu entries or controls.
- New world-generation algorithms, quests or scripted events.
- Arbitrary replacement of project files.
- Direct installation of Java code, JARs or unconverted Java mods.
Those require game-development changes.


### 3. Understanding the template

Extract the template on a computer. It contains:


```text
PokeWilds-Mod-Template/
├── mod.json
├── build_mod.py
├── asset-catalog.json
├── species-reference.json
└── README.txt
```


| File | Purpose |
| --- | --- |
| mod.json | Your mod’s identity and actual changes |
| build_mod.py | Packages the manifest and referenced assets |
| asset-catalog.json | Allowed replacement paths and image sizes |
| species-reference.json | Reference Pokémon fields and move IDs |
| README.txt | Current multi-mod instructions |



Create a files folder beside mod.json for your artwork/audio.

Editing the reference files does not change the game. Put your changes in mod.json.


### 4. Creating your first mod


#### Step A: Set the identity

Open mod.json in a text editor:


```json
{
  "format": 1,
  "id": "my-first-mod",
  "name": "My First Mod",
  "version": "1.0.0",
  "author": "Your Name",
  "resources": {},
  "species": {},
  "palettes": {}
}
```

Keep "format": 1. Use a unique `id` for each separate mod and keep that ID stable for updates.

Use valid JSON: double quotes, no comments and no trailing commas.


#### Step B: Add an artwork replacement

For an Abra overworld replacement, create:


```text
files/abra-overworld.png
```

Then map the approved game destination to your package file:


```json
"resources": {
  "res://assets/original_pokemon/abra/overworld.png": "files/abra-overworld.png"
}
```

- Left side: exact destination from the catalog.
- Right side: relative path inside your package.
- Paths and capitalization should match exactly.
- Merely putting an image in files does not activate it.
For this Abra destination, the expected sheet is 96 × 16 pixels.


#### Step C: Add a Pokémon-data patch if wanted

This example changes Abra’s base HP from 25 to 30:


```json
"species": {
  "abra": {
    "stats": [30, 20, 15, 90, 105, 55]
  }
}
```

The stat order is:

HP, Attack, Defense, Speed, Special Attack, Special Defense.

Only include properties you intend to change.


#### Complete example


```json
{
  "format": 1,
  "id": "my-abra-mod",
  "name": "My Abra Mod",
  "version": "1.0.0",
  "author": "Your Name",
  "resources": {
    "res://assets/original_pokemon/abra/overworld.png": "files/abra-overworld.png"
  },
  "species": {
    "abra": {
      "stats": [30, 20, 15, 90, 105, 55]
    }
  },
  "palettes": {}
}
```


### 5. Sprite rules


#### Ordinary images

Match the dimensions in asset-catalog.json. Preserve the existing sheet’s:

- Frame order.
- Frame positions.
- Transparent background.
- Directional arrangement.
- Required padding.
Do not assume one sprite layout works for every species.


#### Overworld sprites

The loader accepts the supported 96 × 16 horizontal strip layout for overworld replacements. That is six 16 × 16 frames.

Preserve the frame order of the corresponding existing sheet. Larger species can have different catalog dimensions; use their recorded dimensions rather than shrinking everything into the Abra layout.


#### Front battle sprites

Destinations marked "front": true accept square frames stacked vertically:

- Width greater than zero and no more than 96 pixels.
- Height divisible by width.
- Total height no more than 8,192 pixels.
For example, two 40 × 40 frames form a 40 × 80 sheet.

Acceptance does not guarantee arbitrary frame counts produce the intended animation. Match the species’ existing presentation and test it.


#### Back sprites and other artwork

Match the catalog dimensions. The flexible front-sheet rule does not apply to every PNG.


#### Pokémon with several visual resources

Changing an overworld sheet does not automatically replace its battle front, back or authored shiny sheet. Map each relevant destination separately.


### 6. Music, cries and sound effects

Audio replacements must be actual OGG Vorbis files. Renaming an MP3 to .ogg does not convert it.

Example:


```json
"resources": {
  "res://assets/environment/music/night1.ogg": "files/night1.ogg"
}
```

The game keeps the original destination’s playback role and loop settings.

For music with a separate introduction and loop, replace both matching destinations if you want a complete arrangement. Keep their join compatible; the loader does not compose transitions for you.


#### Built-in option interactions

Options may select a different asset:

- LOFI MUSIC ON selects the built-in lo-fi version where available. A replacement of the normal music destination may therefore be bypassed.
- HD EMOTES ON selects the alternate emote sheet. Replacing the normal emote sheet does not replace that alternate sheet.
- DARK MODE may transform supported UI textures.
- CIRCLE LIGHTS selects alternate light masks.
Test the option combinations your mod claims to support. The loader cannot replace an alternate destination absent from its catalog.


### 7. Pokémon-data editing rules

Use exact existing species and move IDs from the references.


#### Lists and nested objects replace existing values

The species patch merges at the property level. It does not append individual learnset or evolution entries.

For example:


```json
"species": {
  "abra": {
    "learnset": [
      {
        "level": 1,
        "move": "TELEPORT"
      },
      {
        "level": 10,
        "move": "CONFUSION"
      }
    ]
  }
}
```

This replaces Abra’s complete learnset with those two entries.

The same principle applies to arrays such as evolutions, types and egg_moves, and nested objects such as overworld_behavior. Preserve every entry or key you still need.


#### Validation rules

- Stats must contain six numeric values, each 1–255.
- Learned moves must already exist.
- Learning levels must be 0–100.
- Evolution destinations must already exist.
- Evolution parameter values must be strings.
- Artwork/cry references must point to catalog-listed destinations.
- Properties must use the expected data type.
- Unknown species and unsupported properties are rejected.
Recognized evolution method names are:


```text
EVOLVE_LEVEL
EVOLVE_ITEM
EVOLVE_TRADE
EVOLVE_HAPPINESS
EVOLVE_MOVE
EVOLVE_INPARTY
EVOLVE_HASINPARTY
EVOLVE_STAT
```

Copy parameter conventions from an existing matching rule rather than inventing them.

Changes to data may not retroactively rebuild already-generated encounters or stored Pokémon. Test new encounters and a separate save as well as loading existing records.


### 8. Shiny palettes

Each species palette requires:


```json
"palettes": {
  "existing_species_id": {
    "normal": [
      [0, 0, 0],
      [0, 0, 0]
    ],
    "back_normal": [
      [0, 0, 0],
      [0, 0, 0]
    ],
    "colors": [
      [0, 0, 0],
      [0, 0, 0]
    ]
  }
}
```

Those zeros are placeholders, not a usable palette.

- normal: two source colors used for front-art matching.
- back_normal: two source colors used for back-art matching.
- colors: two replacement RGB colors, using 0–255 channels.
Source-color matching uses the game’s quantization: int(channel × 32), where the image channel is normalized to 0–1. Ordinary non-white channels therefore typically produce 0–31.

Overworld matching discovers its two source colors from the artwork.

Existing distinct authored shiny artwork takes priority over generated recoloring. If your palette appears ineffective, check whether that species already uses an authored shiny sheet.


### 9. Building the package

You need Python 3 on your computer to use the included builder; you do not need to export an APK.

1. Finish mod.json.
2. Confirm every referenced file exists.
3. Open a terminal in the template folder.
4. Run:

```text
python build_mod.py
```

On Windows, if Python uses the launcher:


```text
py build_mod.py
```

The result is:


```text
my-mod.pwmod
```

You can rename it to something descriptive, such as:


```text
My-Abra-Mod-1.0.0.pwmod
```


#### What the builder includes

It includes:

- mod.json.
- Files referenced by resources.
It does not automatically include the reference databases, README or credits.

To distribute credits, include them beside the package or add them to the finished ZIP archive manually.


#### Manual packaging

A .pwmod is a ZIP with a different extension. Its root must look like:


```text
mod.json
files/
  abra-overworld.png
```

It must not look like:


```text
My-Mod-Folder/
  mod.json
  files/
```

The loader requires mod.json directly at the archive root.


### 10. Package limits


| Rule | Limit |
| --- | --- |
| Archive entries | At most 12,000 |
| Each referenced PNG/OGG file after ZIP decompression | At most 32 MiB |
| Total referenced PNG/OGG file bytes after ZIP decompression | At most 256 MiB |
| Resource formats | PNG and OGG Vorbis |
| Active imported packages | Multiple; no fixed count cap |
| Script replacement | Unsupported |



Paths cannot contain parent-directory traversal, backslashes, drive letters or absolute paths. Duplicate archive entries are rejected.

The byte limits are not guarantees of low memory use: decoded images and audio can consume additional memory.


### 11. Testing and troubleshooting

Before sharing a mod:

1. Back up your save and world caches. Test with the visual/audio options your mod claims to support.
2. Import the package and restart.
3. Check the library entry name, enabled state, and load order.
4. Test every changed view: overworld, party, Pokédex and both battle sides where applicable.
5. Test shiny artwork separately.
6. Test applicable day/night music and intro-to-loop transitions.
7. Test saving and reloading.
8. Disable the tested pack or select ALL MODS OFF, restart, and verify its base resources return.
9. Re-enable it and test the intended mod combination, conflicts, and load order.

| Message or problem | What to check |
| --- | --- |
| “Cannot open mod package” | Damaged download or non-ZIP file |
| “Not a Godot PokeWilds mod package” | Missing root mod.json or excessive entries |
| “Invalid mod manifest” | JSON structure and "format": 1 |
| “Unsupported asset destination” | Exact catalog destination and referenced file |
| “Wrong image dimensions” | Catalog size or front-sheet rules |
| “Invalid mod audio” | Genuine OGG Vorbis encoding |
| “Unknown species or invalid patch” | Canonical species ID and object structure |
| “Invalid learned move” | Existing move ID and valid level |
| Installed but unchanged | Fully restart; check enabled state, load order, selected visual/audio options, and resource destination |
| Shiny unchanged | Authored shiny artwork may override palette recoloring |
| Import updated an existing mod | Both packages use the same id; use distinct IDs for separate mods |
| One enabled mod overrides another | Check CONFLICTS and load order; later matching entries win |
| Mod missing after restart | Check the startup report for missing or invalid packages |
| Mod disabled but save still differs | Resource rollback does not undo saved gameplay changes |



The app validates the package before applying its resources. The included builder is a packaging tool, not a complete gameplay validator.

Current limitation: the template references reflect the supported loader/catalog, not an unrestricted modding API. Successful installation alone does not establish that every changed mechanic, sprite or sound behaves correctly in play.
