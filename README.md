# Kush n Gunz

A third-person crew shooter mod for **Half-Life 1 (GoldSrc)**. Late-2000s camera-phone look: harsh flash, dirty denim, brick three-flats, corner-store glow.

**Chapter 1 — "Free Pennies":** on the one-year anniversary of a friend's death, five friends from Penny Row walk their block to pick up memorial shirts — and every corner pulls them deeper into a war with Tollgate, the crew across the Tracks.

> Everything here is fiction: the neighborhood (Garrow Heights), both crews, every character and event.

## Docs

- [`docs/DESIGN_BIBLE.md`](docs/DESIGN_BIBLE.md) — pitch, visual bible, cast, crews, locations, five missions, game loop, GoldSrc implementation plan, and the four-week plan.

## Layout

```
kng/                 # the mod folder — copy/symlink into your Half-Life install
  liblist.gam        # mod definition
  maps/              # compiled .bsp files
  models/            # compiled .mdl files
  sound/             # .wav effects and voice lines
  media/             # .mp3 music
  gfx/               # menu / HUD art
  sprites/           # HUD sprites, snapshot overlay
src/
  maps/              # TrenchBroom / J.A.C.K. sources (.map)
  models/            # Blender/SMD sources + .qc
  textures/          # source art before WAD conversion
docs/
```

## Setup (when code lands)

1. Install Half-Life on Steam.
2. Copy or symlink `kng/` into `Steam/steamapps/common/Half-Life/`.
3. Restart Steam — **Kush n Gunz** shows up in the library.

Game code will be based on the community-maintained updated Half-Life SDK; build output goes to `kng/cl_dlls/` and `kng/dlls/`.

## Vertical slice target

Missions 1 ("Shirts") and 3 ("Get Tiny Out"): two compact maps, third-person camera, crew follow, Press/Pull Back choice, snapshot system, Heat.
