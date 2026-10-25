# Sony Picture Profiles and Menu-Settings Export

Two different exports live here: Picture Profiles (color and tone, PP1–PP10 on recent bodies) and a plain menu-settings cheat sheet (everything else: AF, drive, metering, and so on). Neither is a file format Lightroom or any external app reads; they are things the user enters in the camera's own menu, or saves and loads with the camera's native Save/Load Settings feature.

## Important caveat to tell the user

**Picture Profile only affects JPEG/HEIF rendering and video. It does not touch RAW data.** If the user shoots RAW (recommended for stills in most of this skill's profiles), Picture Profile only changes the JPEG preview and thumbnail, and the actual grading happens in Lightroom via the preset from `references/lightroom-xmp-template.md`. Picture Profile matters for real when shooting video, where it is baked into the footage, or when shooting JPEG-only.

## Availability varies by body

The preset list below is what Sony's published help guide lists for recent bodies. Earlier or entry-level bodies may have fewer presets or no log profiles, so confirm in the user's own menu or help guide before recommending one by number. Presets can also be edited, so a user's PP numbers may not match the factory defaults.

## Factory Picture Profile defaults (PP1–PP10)

- **PP1**: Gamma Movie. General-purpose video gamma with moderate contrast.
- **PP2**: Gamma Still. Mimics the still-photo tone response for video.
- **PP3**: Gamma ITU709. Sony describes it as a natural color tone, a reasonable base for light grading.
- **PP4**: Sony describes it as faithful to the ITU709 standard. Standard broadcast-style contrast and color.
- **PP5**: Gamma Cine1. Soft gradation in the dark tones and deep colors, good for a cinematic look that grades easily.
- **PP6**: Gamma Cine2. Similar to Cine1 but tuned to retain more highlight detail, easier for broadcast delivery.
- **PP7**: Gamma S-Log2. A flat log profile for heavy grading; wide dynamic range, needs grading to look normal.
- **PP8**: Gamma S-Log3 with color mode S-Gamut3.Cine. A flatter, wider-latitude log profile with a cinema-oriented gamut; needs confident grading.
- **PP9**: Gamma S-Log3 with color mode S-Gamut3. Same log curve as PP8 with a wider gamut.
- **PP10**: Gamma HLG2 (Hybrid Log-Gamma). For direct HDR delivery to HDR displays without a grading pass; not a log profile to grade.

Log profiles typically raise the minimum ISO and look flat on the LCD, so check exposure with a histogram or waveform rather than by eye, and look up the minimum ISO for the user's body in Sony's help guide rather than quoting a number.

## Recommended Picture Profile by scenario

- **Portrait**: PP3 or the standard JPEG rendering; avoid log profiles unless the user grades every shoot, since skin tones are easiest to get right straight out of camera.
- **Action/Wildlife (stills)**: irrelevant if shooting RAW (the normal recommendation); leave it off.
- **Low-light/Astro (stills)**: also irrelevant for RAW. Leave it off, and don't add log noise and latitude tradeoffs to an already high-ISO RAW workflow.
- **Video (general handheld)**: PP1 or PP4 for straight-out-of-camera footage with minimal grading; PP5/PP6 (Cine1/Cine2) for a filmic look that still grades easily; PP7–PP9 (S-Log2/S-Log3) only if the user is comfortable grading and wants maximum dynamic range, and mention the raised minimum ISO.
- **Street**: leave it off if shooting RAW; PP3 if shooting JPEG for a natural, true-to-scene look.

## Menu-settings cheat sheet (Save/Load Settings)

Sony bodies can save their entire menu configuration to the memory card and reload it, but this writes a proprietary file only the camera can read, and it isn't something this skill can generate or edit. What the skill can do is produce a readable checklist: a menu-by-menu list the user can follow to set the camera to match a profile from `references/settings-profiles.md` or a profile saved in `/topics/camera-gear.md`.

Format it as a short checklist, menu area → item → value. The example below uses the a6600-style tabbed menu; newer bodies (a7 IV, a6700 and others) reorganized the menu, so use the names on the user's body and, if unsure, give the item name and a plain description instead of a guessed tab:

```
Camera Settings 1 → Drive Mode → Continuous Hi
Camera Settings 2 → Focus Mode → AF-C
Camera Settings 2 → Focus Area → Zone
Camera Settings 2 → Metering Mode → Multi
Exposure/Color → ISO → Auto (cap at 6400 via ISO Range Limit)
```

Pull the actual values from the relevant settings profile rather than inventing new ones. The checklist restates the same profile in menu-navigable form; it is not a new recommendation.
