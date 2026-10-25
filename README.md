# Sony Camera Assistant — Claude Skill

A [Claude Skill](https://www.anthropic.com/news/skills) that turns Claude into a working reference for shooting with Sony mirrorless cameras: it tracks your camera bodies, lens collection and filter inventory, gives lens- and body-specific shooting advice, builds reusable settings profiles for common scenarios, and exports those profiles as Lightroom develop presets and Sony Picture Profile/menu cheat sheets. It works with any Sony body and adapts to APS-C vs. full-frame, stabilization, card slots and video capabilities instead of assuming a single model.

This skill was previously `sony-a6600-assistant`, which was written around one body. If you installed that version, see [Migrating from sony-a6600-assistant](#migrating-from-sony-a6600-assistant).

## What it does

1. **Body-aware advice** — tell it which Sony body you shoot (or several) and it applies the right crop factor, stabilization, burst and card-slot assumptions. A reference of verified specs covers the a6600, a6700, a7 IV and a7C II, and it looks up and confirms specs for other bodies rather than guessing.
2. **Lens shooting advice** — ask about any lens (owned or hypothetical) and get effective focal length on your body, aperture guidance, IBIS/OSS interplay and AF notes, informed by a lens-category reference (primes, standard zooms, telephoto/wildlife, macro, wide/astro, video).
3. **Camera and lens collection tracking** — mention a body or lens you own and Claude tracks it via its memory feature, so it persists across conversations and devices (see [Notes](#notes)), including filter thread size.
4. **Filter inventory** — mention a filter you own (CPL, ND with stop value, UV, etc.) and Claude tracks it alongside your lenses, cross-referencing thread sizes so recommendations favor what you already own.
5. **Settings profiles** — baseline settings (mode, aperture, shutter, ISO, AF, drive) for portrait, action/wildlife, low-light/astro, video and street, adjusted to your body, lens and conditions, and saveable as named profiles.
6. **Lightroom & Sony Picture Profile export** — turn a saved profile into a Lightroom develop preset (`.xmp`) with matching exposure/tone/color grading, and/or a Picture Profile recommendation plus a menu-settings cheat sheet for replicating it on the camera.

## Installation

### claude.ai / Claude app (web, desktop, mobile)

1. Download `sony-camera-assistant.skill` from the [latest release](https://github.com/jerdog/sony-camera-assistant/releases/latest). A `.zip` of the identical archive is attached to the same release, for upload dialogs that only accept `.zip`.
2. Open **Settings → Capabilities** and make sure **"Code execution and file creation"** is turned on. Skills require this; if the Skills section is greyed out, this is why.
3. Go to **Settings → Customize → Skills**.
4. Tap/click **Add custom skill** and upload `sony-camera-assistant.skill`. If the upload dialog only accepts `.zip`, grab the `.zip` asset from the same release instead; it's the same archive under a different extension.
5. Toggle the skill on.

Skills are tied to your Claude account, not a specific device, so once installed it's available across web, desktop and mobile without reinstalling.

### Claude Code and other CLI-based agents (via `npx`)

If you're using Claude Code, Codex, Cursor, or another agent that supports the open Agent Skills format, you can install straight from this repo using the community [`skills` CLI](https://github.com/Harries/skills-cli):

```bash
# Install to Claude Code
npx skills add jerdog/sony-camera-assistant --agent claude-code

# Install to a different agent (see the CLI's docs for supported agents)
npx skills add jerdog/sony-camera-assistant --agent codex

# List what's in the repo without installing
npx skills add jerdog/sony-camera-assistant --list
```

This is a third-party CLI, not an official Anthropic tool; check its repo for the current list of supported agents before relying on it. It writes `SKILL.md` and `references/` into whatever skills directory your chosen agent expects.

## Usage

Once installed, just talk to Claude naturally; you don't need to invoke the skill by name. It triggers automatically when you:

- Mention a Sony camera, lens or filter, or ask what settings to use for a shoot
- Ask to add or look up a body, lens or filter in your collection
- Ask what to bring for a specific shoot or trip
- Ask for a settings profile for a scenario (e.g. "what settings for low light with my 15mm")
- Ask to export a Lightroom preset or get a Sony Picture Profile/menu cheat sheet for a profile

Example prompts:

```
I shoot a Sony a7 IV and an a6600. Add both to my gear.

I just got a Sigma 18-50mm f/2.8. Add it to my lens collection.

I picked up a K&F Concept 67mm variable ND filter.

What settings should I use for wildlife photography with my 70-350mm on the a6600?

I'm shooting a wedding indoors with the a7 IV — what lens should I bring and what settings profile fits?

Save that as a named profile.

Export a Lightroom preset for my Overcast Irish Landscape profile.

What Sony Picture Profile should I use for video with the Tamron 17-70mm?
```

## Using with other agents

`SKILL.md` follows the Agent Skills format, which a growing number of tools beyond Claude read directly (Claude Code, Codex, Cursor, Windsurf and others; see the `npx skills` option above). For agents that don't support that format, or for general repo-level context, this repo also includes an **`AGENTS.md`** at the root: a tool-agnostic summary of the same domain knowledge written as plain instructions rather than Claude-specific skill metadata. Point any coding agent or assistant at `AGENTS.md` (or paste it into a prompt) to get equivalent guidance without needing skill support.

One difference to know about: the **memory-backed gear collection, filter inventory and saved profiles** are a claude.ai-specific feature. Other agents won't have that persistence automatically; `AGENTS.md` describes a simple file-based fallback (local notes files you maintain yourself).

## Notes

- **Gear and custom profiles use Claude's memory feature**, not this skill's bundled files. Those bundled files (`SKILL.md`, `references/`) are typically read-only once a skill is installed, so personal data that needs to persist and update lives in Claude's memory. Your bodies, lenses, filters and saved profiles carry across every conversation and device tied to your account, even though the skill's own files never change.
- **Lightroom presets export develop-module values** (exposure, tone, color) and affect post-processing only. **Sony Picture Profile only affects JPEG/HEIF and video, never RAW data.** Both caveats are called out wherever the skill generates these exports.
- **Specs are checked, not assumed.** `references/camera-bodies.md` lists only figures verified against published sources, and the skill is instructed to look up and confirm a body's specs before relying on them. Firmware updates can change behavior, so confirm anything critical against your own manual.
- Not affiliated with or endorsed by Sony or Adobe.

## Migrating from sony-a6600-assistant

- **Same repo, renamed:** rename the GitHub repository to `sony-camera-assistant` (GitHub redirects the old URL for clones and links), merge this version in, and publish it as `v2.0.0`. The version jumps to 2.0.0 because the skill name changed.
- **Or a new repo:** keep `sony-a6600-assistant` as-is (or archive it) and create `sony-camera-assistant` as a fresh repo. Copy `CLAUDE.md`, `LICENSE` and `.github/workflows/release.yml` over from the old repo and update any place they mention the old skill name.
- **On claude.ai:** remove the old `sony-a6600-assistant` skill and add this one, so the two don't both trigger on the same requests. Your saved gear and profiles live in memory and carry over; add your camera body (or bodies) the first time it asks.

## Repo contents

```
sony-camera-assistant/                # repo root — this IS the skill
├── SKILL.md                          # Skill definition (Claude Skills format); metadata.version holds the version
├── AGENTS.md                         # Tool-agnostic version for other agents
├── CLAUDE.md                         # Maintainer notes: versioning + release process
├── README.md                         # This file
├── LICENSE                           # MIT
├── references/
│   ├── camera-bodies.md              # APS-C/full-frame class rules + verified per-body specs
│   ├── lens-shooting-guide.md        # Advice by lens category
│   ├── settings-profiles.md          # Baseline scene/lens profiles
│   ├── lightroom-xmp-template.md     # .xmp preset skeleton + field mapping for develop-preset export
│   └── sony-picture-profiles.md      # Picture Profile reference + menu-cheat-sheet guidance
└── .github/workflows/
    └── release.yml                   # Builds and publishes the archive on `v*` tags
```

## Versioning and releases

The skill's version lives in `metadata.version` in `SKILL.md`'s frontmatter (a bare top-level `version:` key isn't part of the accepted schema and fails validation), and git tags mirror it as `v<version>`. Pushing a `v*` tag triggers the release workflow, which verifies the tag matches the declared version (failing loudly if they've drifted), builds the archive, and publishes it to the Releases page as both `.skill` and `.zip`.

The published archive contains only `SKILL.md` and `references/`, nested under a `sony-camera-assistant/` folder; the repo's own docs aren't part of what gets installed. A `.skill` file is just a zip archive with a different extension, so you can also build one by hand from those two paths. See `CLAUDE.md` for the version-bump conventions.

## License

MIT — do whatever you'd like with it.
