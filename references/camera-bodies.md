# Sony Camera Bodies: Class Rules and Verified Specs

Use this when a question depends on the body. It has two parts: rules that hold for any Sony body in a class, and a table of specs that were checked against published sources. Don't extend the table from memory; verify a new body first (see "Adding a body").

## Class rules

- **APS-C bodies** (a6000 series, ZV-E10 series and similar): crop factor 1.5x. Stated focal length × 1.5 = effective focal length. Native lenses are E-mount; FE lenses work but are used at the same crop.
- **Full-frame bodies** (a7 series, a7C series, a9, a1, ZV-E1, FX3/FX6 and similar): crop factor 1.0x. FE lenses use the full sensor. E-mount (APS-C) lenses work but the body normally switches to an APS-C/Super 35 crop mode, which reduces resolution and, for video, changes the field of view.
- **Stabilization, card slots, burst, video modes and Picture Profile support all vary by body** inside each class. Never carry a number from one body to another within a class.

## Verified specs

Figures below come from the sources listed under the table. A blank means it wasn't checked, not that the feature is absent.

| Body | Sensor | In-body stabilization | Card slots | Burst | Video |
|---|---|---|---|---|---|
| a6600 | APS-C | 5-axis, rated about 5 stops | One slot, UHS-I (SD/SDHC/SDXC or Memory Stick Duo) | Up to 11 fps | 4K up to 30p |
| a6700 | APS-C, 26 MP | 5-axis, rated 5 steps | One slot, UHS-I/II SD | 11 fps | 4K60p; 4K120p with a 38% crop |
| a7 IV | Full-frame, 33 MP | 5-axis, rated 5.5 steps | Two slots: slot 1 SD or CFexpress Type A, slot 2 SD | 10 fps (about 5–6 fps in some RAW modes) | 4K30p full width; 4K60p in APS-C/Super 35 mode |
| a7C II | Full-frame, 33 MP | 5-axis, rated up to 7 stops | One slot, SD UHS-II | 10 fps | 4K30p oversampled; 4K60p in APS-C/Super 35 mode |

Other a6600 facts from the same source: native ISO 100–32000 (extended 50–102400, Auto 100–6400); shutter range listed as 1/4000s to 30s (the source does not break out a separate electronic-shutter range); up to 46 RAW or 116 JPEG frames in a full-speed burst.

Sources: B&H Photo product overview for the a6600; Have Camera Will Travel's a6600 memory-card guide; AlphaShooters' a7 IV vs a6700 comparison; Photography Blog's A7C II vs A7 IV comparison. Specs can change with firmware and manufacturer updates, so confirm against the user's own manual for anything that matters.

## Adding a body

When the user shoots a body that isn't in the table:
- Search for Sony's official spec page or the manual and pull sensor format, stabilization, card slots/UHS class, burst rate, and video modes.
- Tell the user which figures came from where, and note anything you couldn't confirm.
- Record the body in the "Camera bodies" section of `/topics/camera-gear.md` using the schema in `SKILL.md` section 2, using only the confirmed figures.

## Things to check per body before advising

- Is there in-body stabilization, and how many stops are claimed? A lens without OSS leans on it.
- How many card slots, and what is the fastest supported card class? This decides whether burst and high-bitrate video need a specific slot.
- Burst rate and RAW buffer depth, and whether the rate drops with uncompressed RAW or a particular shutter mode.
- Video capability: maximum resolution and frame rate, whether higher modes crop the sensor, and whether focus-breathing compensation exists.
- Picture Profile presets and log support. Check the body's help guide rather than assuming.
- Menu structure. Newer bodies reorganized the menu, so avoid quoting tab names you haven't confirmed.
