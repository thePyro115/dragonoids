# Localization Fix for VS 1.21 + PlayermodelLib 1.9.10

## Summary
Fixed race/playermodel name localization so dragoness and draconics display proper translated names in character selection instead of raw key strings.

## Changes Made

### 1. Updated `lang/en.json`
**Key Changes:**
- **Removed all `game:` prefixes** - PlayermodelLib expects mod-domain keys (automatically prefixed with `dadragonoid:`)
- **Added `playermodel-<code>` keys** - Primary keys for model names in character selection:
  - `playermodel-dragoness` → "Dragoness"
  - `playermodel-draconics` → "Draconics"
- **Fixed race key naming** - Changed from generic `race-dadragonoid` to specific model codes:
  - `race-dragoness` → "Dragoness"
  - `race-draconics` → "Draconics"
  - `racedesc-dragoness` and `racedesc-draconics` for descriptions
- **Added comprehensive skin part localization** - Full coverage of all customizable parts:
  - Category names (e.g., `skinpart-voicetype` → "Voice type")
  - All variant names (e.g., `skinpart-voicetype-snake` → "Snake")
  - Added 70+ new localization keys for complete UI coverage
- **Standardized color keys** - Removed `game:` prefix from color keys

### 2. Created `.gitignore`
Added appropriate gitignore for Vintage Story content mod:
- OS-generated files (.DS_Store, Thumbs.db, etc.)
- Editor files (.vscode/, .idea/, etc.)
- Temporary and build artifacts

### 3. Created `LOCALIZATION_GUIDE.md`
Comprehensive documentation covering:
- Localization key structure and naming conventions
- Domain/namespace behavior (when to use prefixes)
- Consistency guidelines and best practices
- Complete checklist for adding new models
- Migration notes explaining previous issues and fixes
- Testing procedures to verify localization

## Technical Details

### Why These Changes Were Needed

**Previous Issues:**
1. Mixed use of `game:` domain and un-namespaced keys caused PlayermodelLib to look in wrong domain
2. Race keys (`race-dadragonoid`) didn't match actual model codes (`dragoness`, `draconics`)
3. Missing `playermodel-<code>` keys that PlayermodelLib 1.9.10+ expects for model names
4. Incomplete skin part localization - many UI elements showed raw key strings

**Root Cause:**
PlayermodelLib in VS 1.21 changed how it resolves localization keys. It now looks for keys in the mod's own domain (e.g., `dadragonoid:playermodel-dragoness`) rather than the `game:` domain. The `game:` prefix in lang files prevented proper resolution.

**Solution:**
- Use un-namespaced keys in `lang/en.json` - the mod domain (`dadragonoid:`) is automatically applied
- Match key names exactly to the model codes used in `config/customplayermodels/*.json`
- Provide comprehensive localization for all UI-visible elements

### Key Format Reference

| Element | Config Code | Localization Key | Example Translation |
|---------|-------------|------------------|---------------------|
| Model name | `dragoness` | `playermodel-dragoness` | "Dragoness" |
| Race name | `dragoness` | `race-dragoness` | "Dragoness" |
| Race desc | `dragoness` | `racedesc-dragoness` | "A noble kin..." |
| Skin part | `voicetype` | `skinpart-voicetype` | "Voice type" |
| Part variant | `snake` | `skinpart-voicetype-snake` | "Snake" |
| Color | `sclera` | `color-sclera` | "Sclera" |

## Files Modified
- `lang/en.json` - Complete localization overhaul with 78 total keys

## Files Added
- `.gitignore` - Standard gitignore for VS content mods
- `LOCALIZATION_GUIDE.md` - Comprehensive localization documentation
- `CHANGES.md` - This file

## Files Unchanged
- `config/customplayermodels/dragoness.json` - No changes needed (uses correct un-namespaced codes)
- `config/customplayermodels/draconics.json` - No changes needed
- `config/model-replacements-bycode.json` - No changes needed (correctly uses namespaced codes)
- `config/model-replacements-byshape.json` - No changes needed

## Testing Recommendations

To verify these changes work correctly:

1. **Character Creation Test**
   - Start game with mod and PlayermodelLib 1.9.10+
   - Open character creation screen
   - Verify "Dragoness" and "Draconics" appear in model dropdown (not `playermodel-dragoness`)

2. **Skin Customization Test**
   - Select Dragoness model
   - Check all skin part categories have readable names
   - Verify all variants display proper names (not raw codes like `skinpart-voicetype-snake`)

3. **Multiplayer Test**
   - Join server with mod
   - Verify other players see correct model names
   - Check that model selection persists correctly

## Backward Compatibility

These changes maintain full compatibility with the existing mod structure:
- Config files remain unchanged (still use un-namespaced model codes)
- Model replacements remain unchanged (still use namespaced codes)
- Asset paths remain unchanged
- Only localization keys were updated to match PlayermodelLib 1.9.10 expectations

## Future Additions

If adding new models in the future:
1. Add model config to `config/customplayermodels/<modelcode>.json` with un-namespaced root key
2. Add `playermodel-<modelcode>` and `race-<modelcode>` keys to `lang/en.json`
3. Add all skin part localization keys following the `skinpart-<code>` pattern
4. Add model replacements to bycode/byshape configs using namespaced `dadragonoid:<modelcode>`
5. Refer to `LOCALIZATION_GUIDE.md` for complete checklist

## References
- PlayermodelLib 1.9.10+ on Vintage Story Mod DB
- Vintage Story 1.21 localization system
- Existing player model mods (Kobold Redux, Humans, Lizardfolk) for reference implementations
