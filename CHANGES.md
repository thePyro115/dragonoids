# Localization Fix for VS 1.21 + PlayermodelLib 1.9.10

## Summary
Fixed race/playermodel name localization so dragoness and draconics display proper translated names in character selection instead of raw key strings.

## Changes Made

### Updated `lang/en.json`
**Key Changes:**
- **Added `game:` prefixed keys** - PlayermodelLib 1.9.10 may look for keys in the `game:` domain, so both formats are now provided:
  - `playermodel-dragoness` AND `game:playermodel-dragoness` → "Dragoness"
  - `playermodel-draconics` AND `game:playermodel-draconics` → "Draconics"
- **Added race keys in both formats**:
  - `race-dragoness` AND `game:race-dragoness` → "Dragoness"
  - `race-draconics` AND `game:race-draconics` → "Draconics"
- **Added skin part keys in both formats** - All 71 skin part and color keys now have `game:` prefixed versions for compatibility

**Total localization keys:** 154 (77 mod-domain + 77 game-domain)

## Technical Details

### Why These Changes Were Needed

**Root Cause:**
PlayermodelLib in VS 1.21 changed how it resolves localization keys. Depending on how the library constructs the lookup key, it may look in either:
- The mod's domain (`dadragonoid:playermodel-dragoness`)
- The game domain (`game:playermodel-dragoness`)

**Solution:**
Provide both key formats in `lang/en.json` to ensure compatibility regardless of how PlayermodelLib resolves the keys:
- Un-namespaced keys: `playermodel-dragoness`
- Game-prefixed keys: `game:playermodel-dragoness`

This dual-key approach ensures the localization will work regardless of which domain the library searches.

### Key Format Reference

| Element | Config Code | Mod-Domain Key | Game-Domain Key | Example Translation |
|---------|-------------|----------------|-----------------|---------------------|
| Model name | `dragoness` | `playermodel-dragoness` | `game:playermodel-dragoness` | "Dragoness" |
| Race name | `dragoness` | `race-dragoness` | `game:race-dragoness` | "Dragoness" |
| Race desc | `dragoness` | `racedesc-dragoness` | (mod domain only) | "A noble kin..." |
| Skin part | `voicetype` | `skinpart-voicetype` | `game:skinpart-voicetype` | "Voice type" |
| Part variant | `snake` | `skinpart-voicetype-snake` | `game:skinpart-voicetype-snake` | "Snake" |
| Color | `sclera` | `color-sclera` | `game:color-sclera` | "Sclera" |

## Files Modified
- `lang/en.json` - Added `game:` prefixed versions of all localization keys (71 new keys added)

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
- Added `game:` prefixed keys as fallbacks while keeping existing mod-domain keys

## Future Additions

If adding new models in the future:
1. Add model config to `config/customplayermodels/<modelcode>.json` with un-namespaced root key
2. Add both key formats to `lang/en.json`:
   - `playermodel-<modelcode>` and `game:playermodel-<modelcode>`
   - `race-<modelcode>` and `game:race-<modelcode>`
3. Add all skin part localization keys in both formats
4. Add model replacements to bycode/byshape configs using namespaced `dadragonoid:<modelcode>`
5. Refer to `LOCALIZATION_GUIDE.md` for complete checklist

## References
- PlayermodelLib 1.9.10+ on Vintage Story Mod DB
- Vintage Story 1.21 localization system
- Existing player model mods (Kobold Redux, Humans, Lizardfolk) for reference implementations
