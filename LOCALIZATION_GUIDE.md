# DA Dragonoid Localization Guide

## Overview
This document explains the localization key structure for the DA Dragonoid mod and how it integrates with Vintage Story 1.21 and PlayermodelLib 1.9.10.

## Localization Key Structure

### Player Model Names
Player model names in character selection use the format:
- `playermodel-<code>` where `<code>` is the un-namespaced model code from the config file

Examples:
- `playermodel-dragoness` → "Dragoness"
- `playermodel-draconics` → "Draconics"

### Race Names (Alternative)
Race keys can also be used:
- `race-<code>` → Display name
- `racedesc-<code>` → Description text

Examples:
- `race-dragoness` → "Dragoness"
- `racedesc-dragoness` → "A noble and prideful kin, hailed in legend for being stronk."

### Skin Parts
Skinnable parts and their variants follow the pattern:
- `skinpart-<partcode>` → Part category name
- `skinpart-<partcode>-<variantcode>` → Variant name

Examples:
- `skinpart-voicetype` → "Voice type"
- `skinpart-voicetype-snake` → "Snake"
- `skinpart-facialexpression` → "Facial expression"
- `skinpart-facialexpression-neutral` → "Neutral"
- `skinpart-baseskin` → "Base skin"
- `skinpart-baseskin-skin1` → "Skin 1"

### Color Keys
Color selections use:
- `color-<colorcode>` → Color name

Examples:
- `color-lizardman-skin1` → "Cobblestone"
- `color-sclera` → "Sclera"

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
Localization keys in `lang/en.json` should use un-namespaced keys within the mod's domain. The game automatically applies the `dadragonoid:` domain prefix when looking up keys.

**Correct format:**
```json
{
  "playermodel-dragoness": "Dragoness",
  "skinpart-voicetype-snake": "Snake"
}
```

**Incorrect format (don't use):**
```json
{
  "game:playermodel-dragoness": "Dragoness",
  "dadragonoid:skinpart-voicetype-snake": "Snake"
}
```

## Consistency Guidelines

1. **Use un-namespaced keys** in lang files - the mod domain is automatically applied
2. **Match the exact codes** used in the playermodel config files
3. **Include both part category and variant names** for complete UI coverage
4. **Provide descriptive names** that are clear to players

## Complete Localization Checklist

For each custom player model, ensure you have:

- [ ] `playermodel-<code>` key for the model name
- [ ] `race-<code>` and `racedesc-<code>` keys (optional, for alternative display)
- [ ] `skinpart-<partcode>` for each skinnable part category
- [ ] `skinpart-<partcode>-<variantcode>` for each variant option
- [ ] `color-<colorcode>` for any custom color selections

## Example: Adding a New Model

If you add a new model called "wyvern":

1. Create `config/customplayermodels/wyvern.json` with root key `"wyvern"`
2. Add localization to `lang/en.json`:
```json
{
  "playermodel-wyvern": "Wyvern",
  "race-wyvern": "Wyvern",
  "racedesc-wyvern": "A fierce winged dragon kin."
}
```
3. Add skin part localizations for any custom parts defined in the model
4. Add model replacements to `config/model-replacements-bycode.json` and `config/model-replacements-byshape.json` using the namespaced code `dadragonoid:wyvern`

## Migration Notes

### Previous Issues
- Mixed use of `game:` and un-namespaced keys caused inconsistent localization
- Keys didn't match the actual model codes (`race-dadragonoid` vs model code `dragoness`)
- Missing skin part category names and many variant names

### Changes Made
- Removed all `game:` prefixes from localization keys
- Added `playermodel-<code>` keys matching the exact model codes
- Updated race keys to match model codes (`race-dragoness` instead of `race-dadragonoid`)
- Added comprehensive skin part localization with category and variant names
- Standardized key naming conventions throughout

## Testing Localization

To verify localization is working:

1. Launch the game with the mod installed
2. Go to character creation
3. Check that:
   - Model names appear correctly in the model selection dropdown (not showing code strings like `playermodel-dragoness`)
   - Skin part categories have readable names (not showing `skinpart-voicetype`)
   - Skin part variants have readable names (not showing `skinpart-voicetype-snake`)
   - All UI elements display properly localized text

If you see raw key strings (like `playermodel-dragoness` instead of "Dragoness"), the localization key is not being found, indicating a mismatch between the key format and what PlayermodelLib expects.
