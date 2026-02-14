# DA Dragonoid Localization Guide

## Overview
This document explains the localization key structure for the DA Dragonoid mod and how it integrates with Vintage Story 1.21 and PlayermodelLib 1.9.10.

## Localization Key Structure

### Player Model Names
Player model names in character selection use the format:
- `playermodel-<code>` where `<code>` is the un-namespaced model code from the config file

**Important:** VS 1.21 + PlayermodelLib 1.9.10 may look for keys in the `game:` domain. For maximum compatibility, provide both:
```json
{
  "playermodel-dragoness": "Dragoness",
  "game:playermodel-dragoness": "Dragoness"
}
```

Examples:
- `playermodel-dragoness` → "Dragoness"
- `playermodel-draconics` → "Draconics"
- `game:playermodel-dragoness` → "Dragoness" (fallback for game domain lookup)
- `game:playermodel-draconics` → "Draconics" (fallback for game domain lookup)

### Race Names
Race keys can also be used:
- `race-<code>` → Display name
- `racedesc-<code>` → Description text

Examples:
- `race-dragoness` → "Dragoness"
- `game:race-dragoness` → "Dragoness" (fallback)
- `racedesc-dragoness` → "A noble and prideful kin, hailed in legend for being stronk."

### Skin Parts
Skinnable parts and their variants follow the pattern:
- `skinpart-<partcode>` → Part category name
- `skinpart-<partcode>-<variantcode>` → Variant name
- `game:skinpart-<partcode>` → Game domain fallback
- `game:skinpart-<partcode>-<variantcode>` → Game domain variant fallback

Examples:
- `skinpart-voicetype` → "Voice type"
- `skinpart-voicetype-snake` → "Snake"
- `game:skinpart-voicetype` → "Voice type"
- `game:skinpart-voicetype-snake` → "Snake"

### Color Keys
Color selections use:
- `color-<colorcode>` → Color name
- `game:color-<colorcode>` → Game domain fallback

Examples:
- `color-lizardman-skin1` → "Cobblestone"
- `color-sclera` → "Sclera"
- `game:color-lizardman-skin1` → "Cobblestone"

## Domain/Namespace Behavior

### Model Codes in Config Files
The custom player model config files (`config/customplayermodels/*.json`) use un-namespaced codes as the root keys:
```json
{
  "dragoness": { ... },
  "draconics": { ... }
}
```

### Model References in Replacements
Model replacement files use fully namespaced codes:
```json
{
  "dadragonoid:dragoness": { ... },
  "dadragonoid:draconics": { ... }
}
```

### Localization Keys
Localization keys in `lang/en.json` should include **both** mod-domain and `game:` prefixed versions for maximum compatibility:

**Recommended format:**
```json
{
  "playermodel-dragoness": "Dragoness",
  "game:playermodel-dragoness": "Dragoness",
  "skinpart-voicetype-snake": "Snake",
  "game:skinpart-voicetype-snake": "Snake"
}
```

## Consistency Guidelines

1. **Provide both key formats** - Include un-namespaced and `game:` prefixed keys for all UI-visible strings
2. **Match the exact codes** used in the playermodel config files
3. **Include both part category and variant names** for complete UI coverage
4. **Provide descriptive names** that are clear to players

## Complete Localization Checklist

For each custom player model, ensure you have:

- [ ] `playermodel-<code>` key for the model name
- [ ] `game:playermodel-<code>` key for game domain fallback
- [ ] `race-<code>` and `game:race-<code>` keys
- [ ] `racedesc-<code>` key for description
- [ ] `skinpart-<partcode>` for each skinnable part category
- [ ] `game:skinpart-<partcode>` for game domain fallback
- [ ] `skinpart-<partcode>-<variantcode>` for each variant option
- [ ] `game:skinpart-<partcode>-<variantcode>` for game domain variant fallback
- [ ] `color-<colorcode>` for any custom color selections
- [ ] `game:color-<colorcode>` for game domain color fallback

## Example: Adding a New Model

If you add a new model called "wyvern":

1. Create `config/customplayermodels/wyvern.json` with root key `"wyvern"`
2. Add localization to `lang/en.json`:
```json
{
  "playermodel-wyvern": "Wyvern",
  "game:playermodel-wyvern": "Wyvern",
  "race-wyvern": "Wyvern",
  "game:race-wyvern": "Wyvern",
  "racedesc-wyvern": "A fierce winged dragon kin."
}
```
3. Add skin part localizations for any custom parts (both formats)
4. Add model replacements to `config/model-replacements-bycode.json` and `config/model-replacements-byshape.json` using the namespaced code `dadragonoid:wyvern`

## Migration Notes

### Previous Issues
1. Mixed use of `game:` and un-namespaced keys caused PlayermodelLib to look in wrong domain
2. Race keys (`race-dadragonoid`) didn't match actual model codes (`dragoness`, `draconics`)
3. Missing `playermodel-<code>` keys that PlayermodelLib 1.9.10+ expects for model names
4. Incomplete skin part localization - many UI elements showed raw key strings

### Solution
Provide **both** key formats (mod-domain and `game:` prefixed) to ensure compatibility regardless of how PlayermodelLib resolves the keys:

```json
{
  "playermodel-dragoness": "Dragoness",
  "game:playermodel-dragoness": "Dragoness",
  "skinpart-voicetype": "Voice type",
  "game:skinpart-voicetype": "Voice type"
}
```

## Testing Localization

To verify localization is working:

1. Launch the game with the mod installed
2. Go to character creation
3. Check that:
   - Model names appear correctly in the model selection dropdown (not showing code strings like `playermodel-dragoness`)
   - Skin part categories have readable names (not showing `skinpart-voicetype`)
   - Skin part variants have readable names (not showing `skinpart-voicetype-snake`)
   - All UI elements display properly localized text

If you see raw key strings (like `playermodel-dragoness` instead of "Dragoness"), the localization key is not being found. Ensure both `playermodel-dragoness` and `game:playermodel-dragoness` keys exist in the lang file.
