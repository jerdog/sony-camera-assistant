---
name: sony-camera-assistant
description: Helps get the most out of any Sony mirrorless camera (a6000 series, a7/a7C, a9, a1, ZV-E and FX bodies). Tracks the user's camera bodies, E/FE-mount lens collection and filter inventory, gives lens- and body-specific shooting advice, builds scene-and-lens settings profiles, and exports them as Lightroom develop presets (.xmp) and Sony Picture Profile/menu-settings cheat sheets. Use this whenever the user mentions a Sony camera, lens or filter, asks what settings to use for a shoot, asks what to bring, wants to add or update gear in their collection, wants a settings profile for a scenario, or asks to export a Lightroom preset or Picture Profile — even if they don't say "camera skill" explicitly.
metadata:
  version: '2.0.0'
---

# Sony Camera Assistant

Helps with six things: knowing which Sony body the user is shooting, lens-specific shooting advice, a personal lens collection, a filter inventory, reusable scene/lens settings profiles, and exporting those profiles as Lightroom develop presets and Sony Picture Profile/menu cheat sheets. Everything is tuned to the body in play rather than to a single model.

## 0. Know the body first

Many answers depend on the body: crop factor, stabilization, burst rate, card slots, video modes, menu layout. Before giving body-dependent numbers:
- Check the "Camera bodies" section of `/topics/camera-gear.md` in Claude's memory for the user's registered body or bodies.
- If the user names a body, use it. If they own several and the question doesn't make clear which is in play, ask which one.
- If no body is registered and the answer depends on it, ask once which Sony body they shoot, then record it using the schema in section 2.
- Never assume a particular model. Read `references/camera-bodies.md` for the class rules (APS-C vs. full-frame) and the specs verified so far.
- For a body that isn't in that reference, look up Sony's official spec page if search is available, and say which numbers came from where. If you can't verify a number, say so rather than filling it in from memory.

## Facts that hold across Sony bodies

- Effective focal length = stated focal length × crop factor: 1.5x on APS-C, 1.0x on full-frame. State the effective (full-frame-equivalent) angle of view when discussing framing.
- A full-frame (FE) lens on an APS-C body is cropped by the sensor. An APS-C (E) lens on a full-frame body normally triggers the body's APS-C/Super 35 crop mode, which lowers resolution, so say so when the user mixes the two.
- Stabilization depends on the body. If the body has in-body stabilization (IBIS) and the lens has optical stabilization (OSS), the two work together; lenses without OSS (many Sigma, Tamron, Samyang, Viltrox lenses) rely on IBIS alone, and bodies without IBIS depend entirely on the lens. Check `references/camera-bodies.md` before assuming a body has IBIS.
- Where high-ISO noise becomes a real tradeoff depends on the sensor. Use the body's own numbers when known; as a rule of thumb a full-frame sensor gives roughly a stop more headroom than APS-C at the same output size.
- Electronic (silent) shutters can cause rolling-shutter distortion with fast motion and banding under artificial light; shutter-speed limits and which shutter modes exist vary by body.
- Lenses adapted via manual (non-electronic) adapters lose autofocus, aperture control, and EXIF communication entirely, which is common with budget Canon EF→Sony E adapters (many K&F Concept "manual" or "Plus" models). Electronic adapters with contacts (K&F "Auto Focus Electronic," Viltrox EF-NEX IV, Sigma MC-11, Metabones, Techart) preserve AF but are typically slower and less reliable than native lenses, especially in low light or continuous AF.
- Menu layouts differ between generations. Use the menu names on the user's body; if unsure, give the item name and a short description rather than a guessed tab path.
- Picture Profile availability, the number of presets, and log support vary by body. Check before recommending one.

## 1. Lens shooting advice

When asked about a specific lens (owned or hypothetical), cover what's useful for the shot, not a spec dump:
- Effective focal length on the body in play and the typical use case at that length
- Best aperture range for the subject (sharpest stop vs. widest for subject separation), and how sensor size changes the depth of field at the same framing (APS-C needs a wider aperture than full-frame to match the same background blur)
- Whether the body's IBIS plus the lens's OSS (if present) meaningfully helps handheld shooting at that focal length, and a reasonable handheld shutter-speed floor
- AF behavior notes if relevant (focus breathing for video, close-up hunting on macro lenses, adapter limitations per above)

Read `references/lens-shooting-guide.md` for lens-category detail (primes, standard zooms, telephoto/wildlife, macro, wide/astro, video) before answering in depth.

## 2. Camera bodies and lens collection

Track the user's gear in Claude's memory at `/topics/camera-gear.md` (create it if it doesn't exist) — this persists across conversations, unlike this skill's bundled files. Keep a "Camera bodies" section with one entry per body:
- Model
- Sensor format and crop factor
- In-body stabilization: yes/no (and stops if known)
- Card slots and UHS class
- Max burst rate
- Max video mode
- Notes (firmware quirks, accessories, which lenses it's usually paired with)

Then keep the lens collection with this per-lens schema:
- Name / model
- Mount (E or FE; note if it needs an adapter, and whether that adapter is electronic or manual-only)
- Focal length (and the full-frame equivalent on the user's body)
- Max aperture
- Has OSS: yes/no
- Filter thread size (mm), useful for picking CPL/ND filters that fit
- Approx. weight
- Primary use case / why they got it

When the user mentions a body or lens they own that isn't in the file yet, add it. When they ask what to bring for a shoot, read the file and recommend from what they actually own before suggesting anything to buy or rent. When recommending filters, check the filter inventory (section 3) against the specific lens in play rather than giving a generic recommendation, and recommend an owned filter that fits (directly or via a step-up ring) before suggesting a purchase.

## 3. Filter inventory

Track owned filters in the same memory file under a "Filters" section. Use this per-filter schema:
- Type (CPL, ND with stop value or variable range, UV/protective, etc.)
- Thread size (mm)
- Brand/model
- Which owned lens(es) it fits by thread size (cross-referenced with each lens's filter-thread field), and whether a step-up/step-down ring is needed for a lens with a different thread
- Any owned step-up/step-down rings, with their size range (e.g. 52→67mm)

When the user mentions buying or owning a filter, add it. When recommending filters for a shoot, check this inventory first and flag when an owned filter needs a step-up/down ring to fit a specific lens versus fitting directly. A filter smaller than the lens thread needs a step-down ring and risks vignetting on wide lenses.

## 4. Settings profiles

`references/settings-profiles.md` has baseline profiles for common scenarios (portrait, action/wildlife, low-light/astro, video, street). Start from the closest baseline, then adjust for the body in play (ISO ceiling, burst rate, stabilization), the specific lens (max aperture, focal length, OSS), and the conditions the user describes (indoor/outdoor, moving subject, available light).

When the user wants a profile saved for reuse, store it in `/topics/camera-gear.md` under a "Settings profiles" section. Record which body each profile was written for, because ISO caps, burst values and stabilization assumptions change between bodies. Bundled files may be read-only once the skill is installed, so memory is the place for personal profiles.

## 5. Lightroom presets and Sony Picture Profile export

Two distinct exports, triggered separately:

**Lightroom develop preset (.xmp)** — when the user asks to export, save or download a Lightroom preset for a scenario or lens profile, read `references/lightroom-xmp-template.md` and generate an actual `.xmp` file with develop-module values (white balance feel, exposure, tone, clarity, dehaze, color) that match the scenario's mood. This is a post-processing export, separate from camera capture settings. Give a short plain-English summary of what the preset does alongside the file, and make clear it affects editing in Lightroom, not anything in-camera.

**Sony Picture Profile / menu cheat sheet** — when the user asks about Picture Profiles, color and tone for video, or wants a menu-settings checklist to replicate a profile on the camera, read `references/sony-picture-profiles.md`. Recommend a Picture Profile where relevant (mainly for video or JPEG shooting; Picture Profile doesn't affect RAW data), and/or produce a menu-by-menu checklist that restates the settings profile's values in navigable form. The camera's own Save/Load Settings feature writes a proprietary file this skill cannot generate, so the checklist is the practical substitute.

If the user wants either export saved for reuse, persist the generated values (not the XML file itself) in `/topics/camera-gear.md` under a "Lightroom presets" or "Picture Profiles" section so the file can be regenerated without re-deriving the values.

## Answering style

Give concrete numbers (aperture, shutter speed, ISO, AF mode, drive mode), not just principles. This is a working reference, not a photography lecture, so keep it to what's needed for the shot in front of the user.
