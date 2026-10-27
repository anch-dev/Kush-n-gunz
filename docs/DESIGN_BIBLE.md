# KUSH N GUNZ — Design Bible

*A third-person crew shooter mod for Half-Life 1 (GoldSrc). Fictional neighborhood, fictional crews, fictional people. Look and feel: a lost 2009 PC mod built out of camera-phone pictures.*

> **Ground rules (read once, apply everywhere).** Every crew, name, symbol, street, and death in this document is invented. No real people, gangs, feuds, or victims. Weapons exist only as arcade game objects — named, weighted for feel, and nothing more.

---

## 1. Pitch and titles

**Pitch:** Over one long night in the fictional Garrow Heights, five friends from Penny Row walk their block to get memorial shirts made for a dead friend — and every corner they cross pulls them deeper into a war with the crew across the tracks.

**Titles**

1. **Kush n Gunz** — the repo name. Loud, dumb-on-purpose, exactly what a 2009 ModDB page would be called. Keep it.
2. **Opp Block** — short, territorial, reads like a mixtape.
3. **Lackin'** — the game's core fear in one word: getting caught slipping.
4. **Free Pennies** — written on the memorial shirts; doubles as the chapter title.
5. **Flash On** — the camera, the night, the moment before it goes bad.

Recommendation: ship as **Kush n Gunz**, call Chapter 1 **"Free Pennies."**

---

## 2. Photo-derived visual bible

### 2.1 What the archive keeps showing us

| Recurring detail | What it tells us | What we build |
|---|---|---|
| 2–5 friends in one frame, one forward, others behind | The crew is the unit, not the hero | Crew always visible on screen; portraits are group shots |
| Harsh flash, face lit, background black | Night = small bright bubble of danger | Night maps use tight light pools and black falloff |
| Pale overcast daylight, muddy colors | Day is flat and tired, not pretty | Daytime maps: desaturated sky, no hard shadows |
| Orange sodium streetlight | The block at 1 a.m. | `light` entities ~`255 160 70`, low brightness, long falloff |
| Yellowish indoor bulbs | Somebody's grandma's kitchen | Interiors `255 230 170`, one source per room |
| Bootcut/baggy denim, long tees, belts, puffy coats | Silhouette = wide bottom, long torso, big feet | Every model: pants flare, shirt hem past the hips, oversized shoes |
| Memorial tees | Grief worn in public | A key texture asset and story object |
| Pointing/leaning/mugging at the lens | Performance for the camera | Idle anims include camera-aware poses (see 2.5) |

### 2.2 Character silhouettes and outfit combinations

Design to the **three-shape read**: at 40 meters in GoldSrc, a player must tell characters apart by *head shape + torso bulk + leg width*, not face.

- **Head shapes:** beanie (round cap), fitted cap forward (flat brim line), cap backward or sideways (asymmetric), long locs (vertical hang past shoulders), braids, bare short hair.
- **Torso bulk:** windbreaker (slick, mid), hoodie (soft, rounded), puffy coat (wide box), varsity jacket (structured shoulders + contrasting sleeves), white tank (narrow, skin showing), long tee (hem to mid-thigh).
- **Legs:** bootcut flare over boots, straight baggy stacked on sneakers, cargo pockets on the thigh, track pants with side stripe.

**Locked outfit combos (no two crew members share a top type):**

| Character | Head | Top | Bottom | Feet | Detail |
|---|---|---|---|---|---|
| Quiet (lead) | short hair | red windbreaker | faded bootcut jeans | tan work boots | big plain buckle |
| Tae Tae | fitted cap, sideways | varsity jacket (navy body, cream sleeves) over long white tee | baggy dark denim | white high-tops | tee hangs under jacket hem |
| Flash | long locs | white tank | loose light-wash denim, sagging | black-and-red sneakers | huge shiny buckle, chain |
| Big Dee | gray beanie | navy puffy coat open over memorial tee | black cargos | black boots | coat is the widest shape in the game |
| Tiny | backward snapback | gray zip hoodie, half-zipped, memorial tee under | camo cargo pants | oversized skate shoes | sleeves too long, hands half-hidden |
| Saint (skull) | skull face, hood up | bright pink hoodie | baggy jeans | huge tan work boots | toy proportions |

Rivals (Tollgate) get **navy, white, and athletic gold** as their block, so a red-or-pink silhouette is always ours and a navy-and-white one is always theirs.

### 2.3 Color palette and material treatment

**Penny Row:** brick red `#8E2A22`, faded denim `#5F7690`, sweatshirt gray `#8A8A86`, off-white `#E6E1D3`, one hot accent (Saint's pink `#E25C9A`).

**Tollgate:** navy `#1F2C4A`, athletic white `#F2F2EE`, gold `#C9A13B`, black.

**World:** brick `#7A4434`, mortar `#A8A091`, sidewalk `#9C978C`, chain-link `#77786F`, sodium orange `#E89A3C`, store-window fluorescent `#D8F0E0`.

**Fabric rules for 8-bit textures:**

- Denim: paint the knees and thighs 15–20% lighter (fade), darken the hem (dirt), add 1–2 px frayed edge at the cuff. Bootcut flare gets a darker inner crease.
- Hoodies: big soft value shapes, a drawn kangaroo pocket, drawstrings as two 2px lines.
- Windbreaker: one hard specular stripe painted across the shoulder (no real specular in GoldSrc, so fake the shine).
- Puffy coat: horizontal baffle bands with a highlight on top of each band.
- White clothing is never pure white — `#E6E1D3` or dirtier. Pure white blooms under flash and looks like a missing texture.
- Faces: low-poly, flat-lit, minimal features. Paint eyes slightly shadowed so flash scenes read as "caught by the camera."

### 2.4 Camera, lighting, grain, and image quality

Two visual modes, never mixed in the same shot:

**Mode A — Street (gameplay):** The game camera. Flat overcast day or orange-night. Mild grain overlay, no flash. This is "you are there."

**Mode B — Snapshot (story beats):** Simulates a 2009 phone camera.
- 1-frame white `env_fade` pop (the flash)
- Freeze on a posed group via `scripted_sequence`
- Overlay sprite: JPEG blockiness, slight purple/green color fringe, rounded vignette
- Orange digital date stamp bottom-right: `10 27 2009  11:48PM`
- Return to Street mode with a 0.5s fade

Rules:
- Snapshots happen at **chapter breaks, safe arrivals, and right before something goes wrong**. Never during a firefight.
- Flash = intimacy and exposure. A snapshot in front of the store is a memory; a snapshot taken *by the rivals* (Mission 1) is a threat.

### 2.5 Neighborhood architecture and map dressing

Build the ordinary. The brief's archive is couches and corner stores, not warehouses.

**Kit of parts (make these textures/brushes once, reuse everywhere):**
- 3-flat brick building face with stone lintels, concrete stoop (5 steps), black iron railing
- Corner store front: 2 big windows with taped-up price signs, a glowing ATM sign, a "WE ACCEPT EBT"-style generic sign (write our own text — e.g. "CHECKS CASHED / PHONE CARDS / HOT FOOD")
- Chain-link fence sections (masked texture `{` prefix), with a gate and a bent post
- Bare tree (sprite or 3-plane model)
- Older sedans and a boxy minivan (static models, 300–500 polys)
- Playground: one slide, a rusted climber, rubber-mat ground
- Interior kit: couch with a throw blanket, CRT TV on a stand, fridge with magnets, 4-burner stove, kitchen chair, laundry pile, doorframe with a bedsheet as a curtain

**Dressing that makes it specific:** a lawn chair on the sidewalk, a milk crate seat, a broken Hot-Wheels track in the courtyard, a laminated memorial photo zip-tied to a fence, stuffed animals and candles, a "NO LOITERING" sign everyone loiters under, a sneaker hanging from a wire.

### 2.6 Group poses, idle animations, crew presentation

Anim set every crew model shares (rename per model only if needed):

| Anim | Description | Use |
|---|---|---|
| `idle_lean` | back against wall, one boot up | waiting at store |
| `idle_wide` | feet wide, arms crossed, chin up | standoffs |
| `idle_clown` | half-laugh, pushes another guy | banter |
| `idle_bored` | checks phone (flip phone prop), looks away | contrast in portraits |
| `pose_point` | points straight at camera | snapshot |
| `pose_lean_in` | leans toward lens, hand on knee | snapshot |
| `pose_couch` | slumped, arm over couch back | basement portrait |
| `look_back` | quick glance over shoulder | follow-mode "checking six" |

**Portrait composition rule** (straight from the archive): one guy forward, two shoulder-to-shoulder behind, one half-out of frame, one not paying attention. Cast those roles to whoever fits the moment — Flash is always forward, Tiny is usually half-hidden.

### 2.7 Ordinary snapshot vs. dramatic mission scene

| | Ordinary | Dramatic |
|---|---|---|
| Light | flat, mixed, accidental | one strong directional source (headlights, a single store window) |
| Camera | slightly tilted, cuts off a head | level, framed, lingers |
| Crew | clowning, bored, mid-sentence | silent, all facing one way |
| Sound | TV in another room, traffic | everything drops except a hum and footsteps |
| Color | muddy | one color dominates (Tollgate navy filling the frame, or sodium orange) |

---

## 3. Original cast — Penny Row

### Marlon "Quiet" Reese — the lead (player)
- **Age:** 19
- **Silhouette:** medium build, slick red windbreaker, flared faded bootcut over tan boots. The only character whose shape is "clean."
- **Color motif:** red
- **Personality:** watches before he talks; counts exits; holds it all in.
- **Voice:** low, few words, flat delivery — the jokes come out dry.
- **History:** Pennies' best friend since third grade. Was supposed to be with him the night he died and wasn't. Nobody blames him out loud.
- **Gameplay role:** the player. Unique ability **Read the Block** — hold a key to slow your walk and highlight rival silhouettes in your line of sight for a few seconds.
- **Goal:** get through the anniversary without losing anyone else.
- **Flaw:** won't say what he wants; the crew fills the silence with their own plans.
- **Line:** *"Nah. We walk the long way. I ain't in a rush to see nobody tonight."*

### Kevontae "Tae Tae" Wilkes — the instigator
- **Age:** 19
- **Silhouette:** varsity jacket with cream sleeves, long white tee hanging under the hem, sideways fitted. Always in motion.
- **Color motif:** navy-and-cream jacket (bought before colors meant anything — a running joke and a real problem)
- **Personality:** funniest guy on the block and the reason half the fights start. Talks to cameras, talks to rivals, talks to cars driving by.
- **Voice:** fast, loud, laughing at his own lines.
- **History:** moved to Penny Row in seventh grade; Pennies adopted him on day one.
- **Gameplay role:** **Draw** — on command, he yells and pulls rival attention toward himself for a few seconds. Useful, but it raises Pressure.
- **Goal:** make sure nobody forgets Penny Row exists.
- **Flaw:** can't let anything go; he'll start a war over a picture.
- **Line:** *"They put bro shirt on upside down? Upside DOWN? Nah, somebody hand me a phone, I'm posting back."*

### Ricky "Flash" Batiste — the show-off
- **Age:** 20
- **Silhouette:** long locs, bright white tank, light-wash denim sagging under a giant chrome buckle and a chain. Narrowest torso, loudest belt.
- **Color motif:** white and chrome
- **Personality:** lives in front of a camera. Charming, vain, generous — buys everybody's food.
- **Voice:** smooth, sing-songy, ad-libs.
- **History:** took most of the crew's photos on his phone — the in-game snapshots are "his" pictures.
- **Gameplay role:** **Hype** — while he's near the crew and unhurt, Morale recovers faster. If he's downed, Morale drops hard.
- **Goal:** get the crew famous — a song, a video, anything.
- **Flaw:** will stop to pose in the middle of something dangerous.
- **Line:** *"Hold up, hold up — everybody get in. Pennies would want the picture."*

### Darius "Big Dee" Holloway — the protective friend
- **Age:** 22
- **Silhouette:** biggest shape in the game: navy puffy coat hanging open over a memorial tee, gray beanie, black cargos. Slow walk, wide stance.
- **Color motif:** gray and navy (the one Penny Row member who wears dark — and it's deliberate: he works night shifts and sleeps in that coat)
- **Personality:** the older one. Tired, patient, quietly furious.
- **Voice:** deep, measured, calls everybody "young" even people his age.
- **History:** Pennies' cousin. Paid for the memorial shirts.
- **Gameplay role:** **Carry** — can pick up a downed crew member and move them (slow, can't fight while carrying). Can shoulder certain doors.
- **Goal:** keep Tiny out of all this.
- **Flaw:** protects people by making decisions for them.
- **Line:** *"Tiny, you stay on my left. My left. I ain't telling you again."*

### Tavion "Tiny" Gaines — the newer member
- **Age:** 18
- **Silhouette:** small frame swimming in a gray zip hoodie, memorial tee under it, camo cargos, backward snapback, giant skate shoes.
- **Color motif:** camo green
- **Personality:** eager, nervous, overcompensating. Laughs too hard at Tae Tae.
- **Voice:** higher, cracks when he's scared, tries to copy Tae Tae's slang and gets it slightly wrong.
- **History:** just started hanging with the crew this year; barely knew Pennies, which he feels guilty about.
- **Gameplay role:** **Lookout** — posted at a spot, he calls out rivals the player hasn't seen yet. Wanders if his Fear is high.
- **Goal:** be treated like he belongs.
- **Flaw:** will do something stupid to prove himself.
- **Line:** *"I got the corner. I'm good. I'm GOOD, Dee, chill."*

### Isaiah "Saint" Hollis — the skull mask
- **Age:** 20
- **Silhouette:** short and chunky in the toy-like style of the concept art — pale skull face mask with black eye sockets and little teeth, bright pink hoodie, baggy jeans, enormous tan work boots.
- **Color motif:** pink (Pennies' favorite color; the crew wears it on the anniversary because of him)
- **Personality:** barely speaks since the funeral. Draws constantly — the skull faces are his art, sharpied on walls, shirts, and phone wallpapers.
- **Voice:** whispered, sparse; when he does talk, everybody stops.
- **History:** Pennies' younger brother. Started wearing the skull mask at the repast and hasn't taken it off around people since.
- **Gameplay role:** **Ghost** — a scout who moves ahead on his own and shows up at flanks. Rivals hesitate when they see him (a short delay before they act).
- **Goal:** finish the mural for his brother.
- **Flaw:** disappears when the crew needs him in one place.
- **Line:** *"He'd laugh at y'all. Arguing about a shirt."*

### The skull-mask approach (recommendation)

**Use the skull figures as a heightened visual layer anchored to one real character.**

- **Grounded:** Saint is a real person in a real (cheap, store-bought) mask. In normal gameplay he's just a guy in a pink hoodie with his face covered.
- **The layer:** Saint's drawings are everywhere — the crew symbol on walls, a sticker on the HUD phone, the loading screens. When the player's **Pressure maxes out** (see §6), the game swaps every character's model to the chunky skull-figure variant (camo hoodie, red emblem hoodie, gray zip, navy-and-white sports hoodie, beanie, backward cap) for a few seconds: the world turns into Saint's sketchbook. Friends and rivals both. It's panic made visible — and it makes everyone look the same, which is exactly the problem.
- **Why this one:** it gives the concept art a job, keeps the human cast grounded, is cheap in GoldSrc (bodygroup or model swap, same skeleton), and it says something: in the moment, nobody can tell who's who anymore.
- **Also:** the skull is Penny Row's crew mark in graffiti/stickers — so it's diegetic, not just a filter.

---

## 4. Fictional crews and neighborhood

### Garrow Heights

A fictional South-Side-style grid of three-flats, courtyard buildings, and corner stores, cut in half by an elevated rail embankment (**the Tracks**). North of the Tracks: Penny Avenue. South: Tollgate Courts, a courtyard project with one way in by car.

### Penny Row (the player's crew)
- **Name origin:** Penny Avenue — and Deshawn "Pennies" Hollis, who got the nickname for always paying in change at the store.
- **Symbol:** Saint's skull face, and a scratched-up penny (kids stick pennies to the memorial fence).
- **Palette:** red, gray, faded denim, one pink accent on the anniversary.
- **Slang (invented/generic):** "the Row" (home), "on Pennies" (I swear), "pull up / pull back," "lackin'," "tap in."
- **Internal dynamics:** no leader. Quiet is the one people watch; Tae Tae is the one people hear; Big Dee is the one who pays. That vacuum is the chapter's tension.

### Tollgate (the rivals)
- **Name origin:** Tollgate Courts; old joke that you pay a "toll" to cut through.
- **Symbol:** a padlock with a "T" for a shackle, stenciled in white spray paint.
- **Palette:** navy, athletic white, gold — matching sports jackets and fitted caps, because their oldest member, Keys, used to buy them in bulk.
- **Slang:** "the Gate," "locked in" (loyal), "out the gate" (going on the attack), "keys" (control).
- **Key people:**
  - **Corey "Keys" Bell, 24** — older, tired, wants it quiet. Grew up playing ball with Pennies. Not a villain; a man who can't control his own people.
  - **Lamont "Lil Duke" Price, 19** — Keys' younger cousin. Took the upside-down-shirt photo. Desperate to be taken seriously — Tiny's mirror image.
  - **"Reebok"** — Tollgate's version of Flash: photographs everything, posts everything.
- **Internal dynamics:** Keys wants a truce; Lil Duke wants a reputation. Every escalation in the story is Lil Duke going around Keys.

### The rivalry (why it hurts)

A year ago, a fight at the Tracks basketball court ended with Pennies dead. Nobody saw who did it. Penny Row believes it was Tollgate; Tollgate says it wasn't. The game never confirms it. On the anniversary, Lil Duke posts a photo of a Tollgate kid wearing a Pennies memorial shirt upside down. That photo is the inciting incident — a picture, because the whole era ran on pictures.

### Locations

#### 4.1 Mr. Oh's Corner Store — *"Oh's Food & Liquor"*
- **Layout:** L-shaped corner. Two lit windows, a recessed door, ATM sign glowing in the right window. Sidewalk 6 m deep; a "NO LOITERING" sign over a milk crate and lawn chair (the gathering spot). Side alley leads to the back door; parked sedan at the curb.
- **Lighting:** the store windows are the brightest thing on the map (fluorescent green-white). Everything else is sodium orange. Inside: harsh tube light, checkered tile.
- **Sound:** fridge hum, buzzing sign, a TV with a game on behind the counter, bus brakes.
- **Story use:** Mr. Oh prints the memorial shirts in the back room. Neutral ground — both crews shop here.
- **Gameplay purpose:** tension hub. No shooting near the windows without a heavy **Heat** penalty. Teaches reading rivals and choosing to back down.

#### 4.2 The Courtyard and Playground (Penny side)
- **Layout:** U-shaped brick courtyard opening onto the street; playground in the middle (slide, climber, two benches); three stoop entrances; a chain-link back fence with a gap to the Tracks.
- **Lighting:** day: flat overcast, no shadows. Night: one working light over the middle stoop; the far corner is dark.
- **Sound:** a swing creaking, a baby crying upstairs, music out of a window, dogs.
- **Story use:** vigil site; where kids play during the day. Saint's mural wall is here.
- **Gameplay purpose:** hold-the-position arena with three approach lanes (street mouth, fence gap, a stoop door) and good low cover (benches, the climber).

#### 4.3 Aunt Netta's Building — hallway, stairwell, kitchen
- **Layout:** 3 floors. Narrow hallway (2 doors each side), U-shaped stairwell with a window at each landing, back porch stairs (wooden, exterior). Netta's apartment: kitchen with a fridge covered in report cards and magnets, a stove with a pot on it, a small TV on the counter.
- **Lighting:** dim yellow bulbs, one burned out per floor. The kitchen is the warmest room in the game.
- **Sound:** TVs behind every door, a door chain rattling, footsteps on wood stairs that echo up the stairwell.
- **Story use:** Tiny's aunt lives here. The extraction mission.
- **Gameplay purpose:** close-quarters, sound-driven. Families behind every door = shooting in the hallway is a Heat disaster. Two exits (front stairwell, back porch) and a choice.

#### 4.4 The Shell Station Edge at Night
- **Layout:** a lit canopy over four pumps; a dark parking lot beside it with five older cars, a dumpster, and a fence onto an empty lot. One exit to the main road, one footpath to the Tracks.
- **Lighting:** the canopy is a white-hot box; the lot is near-black with one flickering pole light. Headlights from passing cars sweep the lot.
- **Sound:** pump beeps, the canopy's high hum, car stereos, a bell on the door.
- **Story use:** the parley. Neutral-ish ground, Tollgate's side of the Tracks.
- **Gameplay purpose:** sightline puzzle. Standing under the canopy, you're seen by everyone. In the lot, you're hidden but blind. Teaches light as danger.

#### 4.5 Penny Avenue Sidewalk and the Tracks Fence
- **Layout:** a 60 m straight sidewalk: brick three-flats on one side, chain-link fence and the rail embankment on the other. Gangways (narrow gaps between buildings) every 15 m. A pedestrian underpass at the end goes under the Tracks.
- **Lighting:** sodium pools every 12 m with dark gaps between — rhythm of light/dark/light.
- **Sound:** a train every couple of minutes that drowns everything out (and can be used to cover movement).
- **Story use:** the connective tissue — every mission walks some of this.
- **Gameplay purpose:** the "walk with the crew" space: reading gangways, choosing pace, stopping under lights or in darkness.

#### 4.6 Big Dee's Basement
- **Layout:** stairs down from a side door. One room: a sagging plaid couch, a CRT on a milk crate with a game console, a mini fridge, a weight bench, a mattress against the wall, a doorway with a bedsheet curtain to the boiler room. One small window at ground level.
- **Lighting:** a bare bulb and the TV's blue glow. Flash glare on the walls in snapshots.
- **Sound:** a furnace kicking on, a game's menu music looping, the upstairs TV through the ceiling.
- **Story use:** home base; mission briefings and the chapter's group portraits.
- **Gameplay purpose:** the safe room. Save point, crew banter triggers, the place you return to. In Mission 5 it becomes the thing you defend.

#### 4.7 The Fence (memorial site)
- **Layout:** a stretch of chain-link at the end of the courtyard facing the basketball court where Pennies died. Zip-tied laminated photo, deflated balloons, candles in glass jars, stuffed animals, pennies stuck in the links, a pink ribbon.
- **Lighting:** candlelight only (a cluster of small flickering lights with `light` style 6 or a custom flicker).
- **Sound:** wind in the fence, distant traffic, nothing else. Music cuts out here.
- **Story use:** start and end of the chapter.
- **Gameplay purpose:** no combat ever happens here. Walking up to it slows the player and lowers the camera. It's the one place the game asks you to stand still.

---

## 5. Story — Chapter 1: "Free Pennies"

**Setup:** It's the one-year anniversary of Pennies' death. Big Dee paid Mr. Oh to print memorial shirts for tonight's vigil. Lil Duke has just posted the upside-down-shirt photo. The crew has to walk the block — store, courtyard, back — and everyone on both sides of the Tracks has seen the picture.

### Mission 1 — "Shirts"
- **Location:** Penny Avenue sidewalk → Mr. Oh's Corner Store
- **Objective:** walk to the store with the crew and pick up the box of shirts.
- **Rival presence:** three Tollgate kids (including Reebok) buying snacks inside. Neutral ground — for now.
- **Complication:** Tae Tae recognizes the kid from the photo. He wants to say something. The player chooses: **Pull Back** (hold Tae Tae, wait it out) or **Press** (let him talk). Press leads to a shoving match on the sidewalk that ends when Mr. Oh comes out yelling; a short fistfight-only scuffle (no weapons drawn — the store is a no-shoot zone).
- **Character moment:** in the back room, Big Dee opens the box. Pink shirts with Pennies' face. Nobody says anything. Flash takes a snapshot; for once, nobody poses.
- **Climax / outcome:** walking out, a phone flash goes off across the street — Reebok just took the crew's picture. It'll be online in ten minutes.
- **Changes the next mission:** if you Pressed, Mission 2 starts at higher Pressure and Tollgate arrives sooner. The photo means Tollgate knows where the vigil is.

### Mission 2 — "The Vigil"
- **Location:** the Courtyard and the Fence
- **Objective:** set up the vigil — hang the shirts on the fence, light candles, gather the neighbors.
- **Rival presence:** mid-vigil, a Tollgate group comes through the fence gap to tag over Saint's mural.
- **Complication:** the courtyard is full of neighbors and kids. Heat is very sensitive here. The crew splits: Saint runs for the mural, Tae Tae runs after Saint.
- **Character moment:** Saint sees his brother's face sprayed over with the padlock. He takes his mask off for the first time — the player sees his real face, briefly — then puts it back on.
- **Climax / outcome:** a fight in the courtyard: first arcade gunfight of the game, hold-the-position against three lanes. Tollgate pulls back over the Tracks. In the chaos, Tiny is cut off and runs into his aunt's building — with Tollgate kids following him in.
- **Changes the next mission:** the mural's state (saved or tagged) stays in the world. Heat earned here sets how many neighbors are watching in Mission 3.

### Mission 3 — "Get Tiny Out" *(extraction — no kill objective)*
- **Location:** Aunt Netta's building — stairwell, hallway, kitchen, back porch
- **Objective:** reach Tiny on the 3rd floor and get him out of the building alive. Optional: get him out without anyone in the building seeing a weapon.
- **Rival presence:** Lil Duke and two others are working up the front stairwell floor by floor, listening at doors.
- **Complication:** Aunt Netta is home and refuses to leave her kitchen. Tiny won't go without her. Sound is everything — footsteps on the wooden stairs carry.
- **Character moment:** in the kitchen, Netta makes Big Dee sit down and eat a plate while the rivals are two floors down. He does. It's the calmest 30 seconds of the chapter — and the most frightening.
- **Climax / outcome:** the player chooses the back porch (quiet, slow, Netta has to be helped down) or the front (fast, but past Lil Duke). The back porch escape uses a train passing to cover the noise. If the player fights in the hallway, they win but Heat maxes and the police siren timer starts.
- **Changes the next mission:** Keys calls Big Dee. He wants to meet at the Shell station to end this before someone dies. How violent Mission 3 was decides how many Tollgate show up to the meeting.

### Mission 4 — "Parley"
- **Location:** the Shell Station Edge at night
- **Objective:** walk Big Dee to the meet with Keys and keep it calm.
- **Rival presence:** Keys and two older guys under the canopy. Lil Duke and his friends are somewhere in the dark lot — Keys doesn't know they came.
- **Complication:** the player has to position the crew: under the light (seen, trusted) or in the dark (safe, but it reads as an ambush to Keys). Tae Tae won't stop talking.
- **Character moment:** Keys and Big Dee, alone by the ice machine. Keys says he didn't do it and he doesn't know who did. Big Dee believes him. Neither of them can do anything about it.
- **Climax / outcome:** Lil Duke breaks it — headlights flash, and it goes bad. Big arcade escape across the lot and down the footpath to the Tracks, with Keys' guys and Lil Duke's guys not even on the same side. Flash goes down; Big Dee carries him.
- **Changes the next mission:** Flash is hurt and can't hype (Morale recovers slower). Keys has lost control of Tollgate. Lil Duke is coming to the Row.

### Mission 5 — "Basement / The Fence"
- **Location:** Big Dee's Basement → Penny Avenue → the Fence at dawn
- **Objective:** Phase 1 — defend the basement while Flash is patched up. Phase 2 — at dawn, walk to the Fence and finish the vigil.
- **Rival presence:** Lil Duke's group coming down Penny Avenue, hitting the basement window and side door.
- **Complication:** one door, one window, Flash on the couch. Tiny wants to go out and "end it." Saint is gone.
- **Character moment:** Tiny tries to leave through the boiler room. Quiet stops him — first time Quiet raises his voice. Then Saint appears at the end of the block in the skull mask and pink hoodie, standing in the street; Lil Duke's crew hesitate long enough for the player to push them back.
- **Climax / outcome:** the attackers scatter at first light (sirens getting closer). The crew walks to the Fence. The shirts are still there. Saint finishes the mural over the padlock. Final snapshot: the crew in pink in front of the fence — one guy forward, two behind, one on the couch-cushion they carried for Flash, and an empty space where Pennies would stand.
- **Changes the next chapter:** depending on total Heat and choices — Keys may be gone (moved away), Lil Duke may now run Tollgate, and either Tiny or Tae Tae's trust in Quiet is damaged. The war isn't over. Chapter 2 hook: someone sends Quiet a photo from the night Pennies died.

---

## 6. Core game loop

```
SAFE ROOM (basement)  →  WALK THE BLOCK with the crew  →  READ the street
       ↑                                                       ↓
   RETURN SAFE  ←  ENGAGE or EVADE  ←  ENCOUNTER (rivals spotted / spot you)
```

1. **Brief in the basement.** Banter, a snapshot, pick a destination.
2. **Walk.** The crew follows in a loose wedge. Pace matters: walking keeps everyone together; running splits the crew.
3. **Read.** Gangways, cars, windows. Use Read the Block. Tiny calls out what he sees.
4. **Encounter.** A rival group comes into view — or you hear them first.
5. **Decide.** Press (advance, provoke, fight) or Pull Back (detour, wait, use a train or a crowd as cover).
6. **Resolve.** Arcade fight, a scuffle, or a clean slip.
7. **Get home.** The mission isn't over until the crew is back somewhere safe. Getting out is half the game.

### Meters (four, all visible and simple)

| Meter | Goes up when | Goes down when | Effect |
|---|---|---|---|
| **Cohesion** | crew within ~10 m of player | someone strays, gets split by running | High: allies follow orders, call targets. Low: allies act on their own. |
| **Pressure** (fear) | rivals in view, shots nearby, someone downed | backing off, darkness, time | At max: aim sway + the **skull layer** kicks in for a few seconds. |
| **Morale** | Flash near & unhurt, banter, winning a fight | crew member downed, memorial tagged | Low morale: allies hesitate, Tae Tae goes quiet (bad sign). |
| **Heat** (neighborhood) | fighting near the store, in the hallway, around neighbors | leaving the area, waiting it out | High: siren timer starts; at the end, you must leave or the mission fails. Total Heat changes the ending. |

### Crew commands (4 keys, nothing more)
- **Follow** (default)
- **Hold** (stay here)
- **Pull Back** (everyone retreat to last safe point)
- **Ability** (context: Tae Tae Draw / Big Dee Carry / Tiny Lookout)

### Fictional arcade weapons
Kept nontechnical — game objects only:
- **"Old Rusty"** — a battered prop revolver: slow, heavy-hitting, loud. The default.
- **"The Brick"** — a fictional boxy pistol: faster, weaker, quieter.
- **"The Long One"** — a fictional long gun found only twice: big hit, slows your walk, everyone hears it.
- **Fists** — scuffles in no-shoot zones, knocks people down without injuring.

Downed allies can be revived (hold Use) or carried by Big Dee. Nobody in the crew dies in Chapter 1 — injury and Heat are the consequences. Rivals are knocked down and either crawl off or are carried away by their friends: readable, fictional, human.

---

## 7. GoldSrc implementation

### Base
- Start from the **updated Half-Life SDK** (the community-maintained `halflife-updated` fork) so the code builds on modern Visual Studio / GCC and runs against the 25th-anniversary Steam build.
- Maps: **TrenchBroom** or **J.A.C.K.**, compiled with **VHLT** (Vluzacn's Half-Life Tools).
- Models: **Blender** → export SMD (Blender Source Tools, GoldSrc mode) → compile with **Crowbar**/studiomdl. Or **MilkShape 3D** if one of you already knows it.
- Textures: Wally or **HLTexture**/`Half-Life Texture Tools` for WADs; 8-bit indexed, 256 colors per texture.

### Third-person camera and aiming — **practical**
- `cam_command 1` / `thirdperson` exists already, but it's raw. Write your own in the client's `V_CalcRefdef` (view.cpp): offset camera back ~80 units and right ~25 (over-the-shoulder), trace from the player's eyes to the desired camera spot, and pull the camera in if it hits a wall. Smooth the offset with a lerp so it doesn't snap.
- **Aiming:** trace from the camera through the crosshair to find the target point, then fire from the player toward that point. This avoids the classic "shoulder cam shoots into the wall next to you" problem.
- Player model must face the aim direction while firing; let it face movement direction when not.
- **Ambitious:** a cover-snap system. Skip it. Use crouch + good map cover instead.

### Movement and combat feel — **practical**
- Slow the default HL speed (it's a sprint by street standards): walk ~140, run ~220. Disable bunny-hop, long jump, and fall-damage weirdness.
- Arcade feel: a big readable hit reaction (flinch anim + screen punch), short knockdown instead of instant ragdoll (GoldSrc has no ragdolls — use death/knockdown sequences).
- Fake **Pressure aim sway** client-side by drifting the crosshair.

### Ally follow and enemy AI — **practical with care**
- Allies: derive from `CTalkMonster` (Barney/scientist code). It already does "follow the player," idle chatter, and sentences. Add a formation offset per ally so they don't stack single-file.
- Enemies: derive from `CSquadMonster` (the grunt squad code). Squads, cover-seeking, and flanking already exist. Re-tune: they should hesitate, shout, and retreat more than grunts do.
- Add two custom states: **Taunt** (stand, shout, don't shoot yet) and **Flee** (run to a `info_node` marked as their exit).
- **Ambitious:** allies who really use cover intelligently. HL's node graph is coarse. Accept "good enough," hand-place nodes generously, and stage encounters so allies stand somewhere sensible by design.

### Dialogue and mission scripting — **practical, mostly no code**
- `scripted_sentence`, `scripted_sequence`, `ambient_generic`, `multi_manager`, `trigger_once`, `trigger_relay`, `env_message`, `game_text` do almost everything.
- Use `sentences.txt` for crew barks (randomized groups like `TAE_TAUNT0..5`).
- Add a few custom entities in code: `trigger_heat` (adds Heat while player fires inside it), `env_snapshot` (white fade + freeze + overlay + date stamp), `trigger_choice` (Press/Pull Back prompt that fires one of two targets).
- Mission state between maps: use `trigger_changelevel` with landmarks, and store choice flags in a small save-persistent global (`env_global` works for this out of the box).

### Models, textures, animation, sound
- **Characters:** 6 crew + 3 named rivals + 2 generic rival bodies (with 3–4 head/hat bodygroups each) + 2 neighbor bodies. Target **1,500–3,000 polys** each, one 512×512 (or 2–3 smaller) 8-bit texture.
- **Skull layer:** a single chunky skull-figure body on the **shared skeleton**, with hoodie-color skins. Model swap at max Pressure.
- **Animations** (shared skeleton across all humans so anims are reusable): idle ×4, walk, run, crouch walk, aim, shoot (pistol/long), reload-gesture (generic), flinch ×2, knockdown, get-up, downed-crawl, carry/carried, the 8 pose/idle anims in §2.6.
- **Sound:** 22 kHz mono WAV for effects and voices. Record your own barks (both of you, plus friends). Music: original tracks — slow, dark, booming 808s, sparse piano/bell melodies, period-appropriate drill-inspired production — play as MP3 via the engine's `mp3 play` command triggered from map logic.
- **Voice budget for the slice:** ~150 lines total.

### Map size and staging
- Keep each map small: **one street + one interior** per BSP. GoldSrc maps get ugly fast past ~4,000 × 4,000 units with lots of detail. Target **r_speeds wpoly < 800** in any view.
- **Sightlines:** use the Tracks embankment, parked cars, and building corners to break long views. Every encounter starts with a **reveal moment**: turning a corner, a door opening, a train passing.
- **Doors:** treat doors as the main tension tool. Rivals behind a door you can hear before you see.
- **Sound as warning:** place `ambient_generic` rival chatter and a car stereo *before* the rivals are visible. Players learn: music = someone's there.
- **Light as danger:** bright = exposed. Make that consistent across every map.

### HUD and menu
- HUD in a **flip-phone frame** bottom-left: low-res 4-bar meters for Cohesion/Pressure/Morale/Heat, a tiny 2-color crew status (6 face icons, grayed when downed).
- Mission cards and loading screens: a blurry group snapshot, JPEG-crunched (actually save them at low JPEG quality before converting), with the orange date stamp and an MSN-style caption: *"free pennies 4ever </3 penny row"* — plain text only, since GoldSrc fonts won't render emoji.
- Main menu background: a basement couch portrait, grainy, flash-lit. Menu font: chunky Arial Black / Impact, slightly stretched.

### Realistic vertical slice
**Missions 1 and 3**: "Shirts" and "Get Tiny Out." Together they show walking, reading, the Press/Pull Back choice, the snapshot system, a scuffle, an extraction, and Heat — without needing the big setpiece fights.

**Slice includes:** 2 maps, 4 crew models (Quiet, Tae Tae, Big Dee, Tiny), 2 rival models, 1 skull-layer model, camera, 2 weapons + fists, Cohesion/Pressure/Heat (Morale can wait), ~60 voice lines, 2 music tracks.

**Postpone:** vehicles of any kind, Flash's Hype system, Mission 4's lot fight, the full skull swap on every character (do it on rivals only first), multiplayer, cover system, a world map/hub.

---

## 8. First-month plan

Two people. Call them **Code** (SDK, camera, AI, entities) and **World** (maps, textures, models, sound). Swap if that's not how you split.

### Week 1 — "Walk the block"
- **Code:** set up the SDK, build `hl.dll`/`client.dll`, create `kng/` mod folder; over-the-shoulder camera with wall collision; camera-trace aiming; slowed movement.
- **World:** graybox Penny Avenue sidewalk + the corner store front (dev textures only); orange light pass; place a train `func_train` on the Tracks.
- **Milestone:** you can launch Kush n Gunz from Steam, walk the graybox block in third person, look into the store window, and watch a train go by.

### Week 2 — "The crew follows"
- **Code:** ally follow from `CTalkMonster` with formation offsets; the 4 crew commands; Cohesion meter (simple HUD bar).
- **World:** first proxy crew model (just one body, 4 skins) on the shared skeleton with walk/run/idle; interior graybox for Mr. Oh's store; first texture WAD: brick, sidewalk, chain-link, store window.
- **Milestone:** walk to the store with 3 crew members following in a wedge, tell them to Hold, walk off, come back, tell them to Follow.

### Week 3 — "Somebody's at the store"
- **Code:** rival AI from `CSquadMonster` with Taunt/Flee states; `trigger_choice` (Press / Pull Back); fists + "Old Rusty"; Pressure meter with aim sway.
- **World:** rival proxy model (navy/white skins); full scripting for Mission 1 — barks, the standoff, Mr. Oh's interruption; record the first 30 voice lines.
- **Milestone:** Mission 1 playable start to finish in graybox — both choices work, the scuffle happens or doesn't, you get the shirts.

### Week 4 — "Flash on"
- **Code:** `env_snapshot` (flash, freeze, overlay, date stamp); Heat meter + `trigger_heat` + siren timer; skull-layer model swap on rivals at max Pressure.
- **World:** art pass on the store map (real textures, signs, dressing, the lawn chair and milk crate); first real Quiet and Tae Tae models; one original music track in the store; graybox Aunt Netta's building for Mission 3.
- **Milestone:** a **polished Mission 1** — textured, voiced, with a snapshot at the store and Reebok's camera flash at the end — and Mission 3 walkable in graybox. Record a video of it. That's your ModDB page.
