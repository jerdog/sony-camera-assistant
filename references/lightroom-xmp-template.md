# Lightroom Develop Preset (.xmp) Template

Use this when the user asks to export a Lightroom preset for a shooting scenario/lens profile. Generate an actual `.xmp` file (via file-creation) using the skeleton below, filled in with develop-module values that match the mood of the scenario — not just the camera capture settings, but how the RAW should be *graded* (white balance feel, contrast, tone, color).

## Important caveat to tell the user

A Lightroom preset only affects how you **develop/grade a RAW or JPEG afterward** — it has no connection to the camera's in-camera settings (those are covered by the Sony Picture Profile / menu cheat-sheet in `references/sony-picture-profiles.md`). Mention this distinction when delivering a preset so it isn't confused with a camera setting.

## XMP skeleton

```xml
<?xml version="1.0" encoding="UTF-8"?>
<x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="XMP Core 5.4.0">
 <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
  <rdf:Description rdf:about=""
    xmlns:crs="http://ns.adobe.com/camera-raw-settings/1.0/"
    crs:PresetType="Normal"
    crs:Cluster=""
    crs:UUID="GENERATE-A-UUID-HERE"
    crs:SupportsAmount="False"
    crs:SupportsColor="True"
    crs:SupportsMonochrome="True"
    crs:SupportsHighDynamicRange="True"
    crs:SupportsNormalDynamicRange="True"
    crs:SupportsSceneReferred="True"
    crs:SupportsOutputReferred="True"
    crs:CameraModelRestriction=""
    crs:Copyright=""
    crs:ContactInfo=""
    crs:Version="16.0"
    crs:ProcessVersion="15.4"
    crs:WhiteBalance="Custom"
    crs:Temperature="TEMP_VALUE"
    crs:Tint="TINT_VALUE"
    crs:Exposure2012="EXPOSURE_VALUE"
    crs:Contrast2012="CONTRAST_VALUE"
    crs:Highlights2012="HIGHLIGHTS_VALUE"
    crs:Shadows2012="SHADOWS_VALUE"
    crs:Whites2012="WHITES_VALUE"
    crs:Blacks2012="BLACKS_VALUE"
    crs:Clarity2012="CLARITY_VALUE"
    crs:Dehaze="DEHAZE_VALUE"
    crs:Vibrance="VIBRANCE_VALUE"
    crs:Saturation="SATURATION_VALUE"
    crs:ConvertToGrayscale="False"
    crs:ToneCurveName2012="Linear"
    crs:CameraProfile="Adobe Standard"
    crs:HasSettings="True">
   <crs:Name>
    <rdf:Alt>
     <rdf:li xml:lang="x-default">PRESET_NAME</rdf:li>
    </rdf:Alt>
   </crs:Name>
   <crs:ShortName>
    <rdf:Alt>
     <rdf:li xml:lang="x-default">PRESET_NAME</rdf:li>
    </rdf:Alt>
   </crs:ShortName>
   <crs:SortName>
    <rdf:Alt>
     <rdf:li xml:lang="x-default">PRESET_NAME</rdf:li>
    </rdf:Alt>
   </crs:SortName>
   <crs:Group>
    <rdf:Alt>
     <rdf:li xml:lang="x-default">Sony Camera Assistant</rdf:li>
    </rdf:Alt>
   </crs:Group>
  </rdf:Description>
 </rdf:RDF>
</x:xmpmeta>
```

## Field mapping (what each value controls)

- `crs:Temperature` / `crs:Tint` — white balance in Kelvin and green/magenta shift. Leave `WhiteBalance="As Shot"` instead of `"Custom"` (and drop these two attributes) if the preset shouldn't override the camera's WB.
- `crs:Exposure2012` — exposure adjustment in stops, e.g. `+0.30` or `-0.50`. Usually `0.00` unless the scenario calls for a baked-in push/pull.
- `crs:Contrast2012`, `crs:Highlights2012`, `crs:Shadows2012`, `crs:Whites2012`, `crs:Blacks2012` — range roughly -100 to +100. Lifted shadows + pulled highlights = flatter/moodier; pushed whites + crushed blacks = punchier/contrastier.
- `crs:Clarity2012` — local contrast/texture, -100 to +100. Positive for landscapes/architecture, negative or 0 for portraits (avoid harsh skin texture).
- `crs:Dehaze` — -100 to +100. Positive cuts atmospheric haze (useful for distant landscapes, hazy astro skies); negative adds a soft/glowy look.
- `crs:Vibrance`, `crs:Saturation` — -100 to +100. Prefer Vibrance (protects skin tones) over Saturation for portraits; either is fine for landscapes.
- `crs:ProcessVersion` — ties to the Lightroom version's processing engine. `15.4` corresponds to recent Lightroom Classic/Lightroom (Process Version 6, "Current"); tell the user to verify against their own version and re-save if Lightroom auto-updates it on import.
- `crs:CameraModelRestriction` — leave empty so the preset appears for any camera. Set it to a body's model name only if the user wants the preset to show up for that body alone.
- `crs:UUID` — generate a fresh random UUID per preset (any v4 UUID generator/library works) so Lightroom treats it as a distinct preset rather than colliding with another.

## Worked example — "Overcast Irish Landscape" preset

Mood: protect sky highlights already handled in-camera via exposure comp, so lean the develop side toward lifted shadows (recover foreground detail), mild dehaze (cut through flat grey haze), gentle clarity (texture on stone/grass without looking harsh), and a touch of extra saturation since overcast light desaturates color.

```xml
crs:WhiteBalance="As Shot"
crs:Exposure2012="0.00"
crs:Contrast2012="+5"
crs:Highlights2012="-20"
crs:Shadows2012="+35"
crs:Whites2012="+5"
crs:Blacks2012="-5"
crs:Clarity2012="+15"
crs:Dehaze="+15"
crs:Vibrance="+15"
crs:Saturation="0"
```

(Drop `crs:Temperature`/`crs:Tint` entirely when `WhiteBalance="As Shot"`, since they're only meaningful with `WhiteBalance="Custom"`.)

## Delivery

Generate the `.xmp` file with the scenario's name (e.g. `overcast-irish-landscape.xmp`), present it to the user as a file, and tell them to drop it into Lightroom Classic's `Develop Presets` folder (or import via Develop module → Presets panel → `+` → Import Presets in Lightroom / Lightroom Classic) — do not just paste the XML inline unless the user's environment can't create files, in which case give it as a labeled code block they can save themselves.
