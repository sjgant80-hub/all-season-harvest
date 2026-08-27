# ALL-SEASON HARVEST · the multi-rung hybrid controller

**Defensive Publication (Public Domain / CC0). Published 2026-08-27 by Simon Gant (AI-Native Solutions). This disclosure places the described architecture in the prior art as of its publication date.**

### solar (light) + wind (motion) + thermal + RF · one battery · one small controller
### the big side-rung (wind) is strongest EXACTLY when the top rung (solar) dies
### keeps a northern setup at BULB + LAPTOP level all year · NOT free energy · for bench / build

> THE PATTERN: energy folds DOWN the ladder in cloud/dark (visible→infrared→thermal→RF),
> AND it folds SIDEWAYS into MOTION — the same weather that blocks your sun IS the atmosphere
> moving (wind). so the thief that steals your top rung (cloud) is delivering a big side-rung
> (wind). solar+wind are ANTI-CORRELATED: when one drops the other tends to rise. this
> controller treats solar, wind, thermal, and RF as FOUR RUNGS OF ONE HARVEST, pulling from
> whichever is live, into one battery — so the setup stays at real bulb+laptop power every
> season, because the big rung is always strongest when the top rung fails.

---

## §0 · THE RUNGS + HONEST NUMBERS (name them — this is what keeps it real)

```
per typical small off-grid setup, rough real-physics order of magnitude:

  TOP RUNG · LIGHT (solar PV, existing)
     sun direct      150–1000 W    unchanged
     cloudy diffuse   15–250 W     PV still works at 10–25%

  SIDE RUNG · MOTION (small wind turbine)  ← THE POWER PIECE
     small turbine    50–400 W     STRONGEST at night / cloud / winter
                                   (a laptop is 30–65 W, an LED bulb 5–10 W — this covers it)

  LOW RUNGS · TRICKLE (keep-alive, micro-loads)
     night thermal    0.01–1 W     panel's own radiative-cooling ΔT via thermoelectric layer
     ambient RF       µW–mW       always-on, cloud/night-blind

THE KEY: solar carries the sunny/day load; WIND carries the cloudy/night/winter load (the
exact hours solar can't); thermal+RF keep the battery alive and micro-loads running in dead
calm dark. one battery, four rungs, matched to conditions. bulb+laptop level, all year.
```

---

## §1 · THE ANTI-CORRELATION (why wind is THE missing piece, not just "add a turbine")

```
solar and wind are NATURE'S ANTI-PAIR:
  · cloudy = often windy (the weather system bringing cloud brings wind)
  · night  = often windy (many places wind up after dark)
  · winter = windiest (the season solar dies is the season wind peaks)

so the source that's STRONG is almost always the one you need — when solar drops, wind
tends to rise, and vice versa. that's not luck you're exploiting; it's WHY solar+wind is
the real off-grid standard. wind fills the exact gap solar leaves. the cloud that kills
your panel is being PUSHED by the wind that powers your battery. you harvest the thief.
```

---

## §2 · THE NEVER-BEFORE CORE (the two things that make it new, together)

```
A) THE PANEL AT NIGHT IS A RADIATIVE-COOLING HARVESTER
   facing the cold sky it drops colder than the ground → a thermal gradient → harvestable
   with a thermoelectric layer on the panel back. same hardware, second job, at night.
   (trickle — keeps micro-loads alive; not the power rung.)

B) ONE SMALL CONTROLLER TREATS ALL FOUR RUNGS AS ONE HARVEST
   solar+wind hybrids EXIST — but as two big separate systems, two controllers, bolted
   side by side. NOBODY packages solar + wind + thermal + RF as a SINGLE small all-rung
   unit that reads whichever rung is live and feeds one battery. THAT unification is the
   new thing. not "solar and wind and thermal" — four rungs of ONE harvest, one controller.
```

---

## §3 · THE CONTROLLER (what it does)

```
sits between all the sources and the ONE battery. reads which rung(s) are live, conditions
each, sums into the battery:

  SUN UP        → solar PV (MPPT) carries it · + wind if blowing
  CLOUDY DAY    → diffuse PV + WIND carries the load · + thermal top-up
  NIGHT         → WIND carries the load · + thermal trickle for micro-loads
  DEAD CALM DARK→ thermal + RF trickle keep the battery alive + micro-loads running
  WINTER        → WIND is peak, solar dead → wind carries the whole load

one controller. one battery. leans on whichever rung is strong — and the strong one is
almost always the one that matches the conditions (the anti-correlation does the work).
```

---

## §4 · THE PARTS (little, bolt-on, real)

```
1. SOLAR          existing panel · unchanged · MPPT input on the controller
2. WIND           a small turbine (50–400 W class) — the power rung for the bad seasons.
                  small vertical-axis (VAWT) is simplest/quietest for a cabin/rooftop.
                  rectifier → controller input. THIS is what reaches bulb+laptop level.
3. TEG LAYER      thermoelectric sheet bonded to the panel BACK + thermal path to ground.
                  night radiative-cooling ΔT → millivolts → boost. (trickle keep-alive.)
4. RF PICKUP      small wideband antenna + rectifier → µW–mW ambient RF. always-on trickle.
5. THE CONTROLLER the brain: per-rung input conditioning + rung-detect + combine → battery
                  · MPPT for solar · rectify+regulate for wind · ultra-low-V boost for TEG/RF
                  · sums all live rungs into one charge line · reports which rungs live + W in
6. BATTERY        existing battery · unchanged · now fed year-round, never fully dead
```

---

## §5 · THE CONTROLLER LOGIC (simple, condition-matched)

```
loop:
  read PV_power, WIND_power, TEG_voltage, RF_level, battery_%
  # each live rung is conditioned and summed — not either/or, ADDITIVE:
  charge_in = 0
  if PV_power  > 0 : charge_in += MPPT(PV)             # solar when light
  if WIND_power> 0 : charge_in += regulate(WIND)       # wind when moving (the power rung)
  charge_in += boost(TEG_voltage)                      # thermal trickle, always tried
  charge_in += boost(rectify(RF_level))                # RF trickle, always
  route charge_in → battery
  report: live rungs, total W in, battery %, which rung is carrying

key parts:
  · MPPT for solar (as now) · a rectifier+regulator for the wind AC → DC
  · ultra-low-voltage boost IC for TEG/RF (starts from ~20mV) — makes trickle reach battery
  · all rungs ADD into one battery — the sum across conditions is what keeps it powered
```

---

## §6 · WHAT IT HONESTLY DELIVERS (bulb + laptop level)

```
· SUMMER/SUN     : full solar (+ wind bonus if blowing) — plenty
· CLOUDY DAY     : diffuse solar + wind → real watts, runs laptop + lights
· NIGHT          : wind → real watts (laptop + lights) · thermal/RF keep-alive in calm
· WINTER         : wind peak carries the load — the season solar died is now covered
· DEAD CALM DARK : trickle rungs keep the battery alive + micro-loads (sensor/LED/radio)

honest pitch: "a solar setup that stays at laptop-and-lights level every season, because
when the sun's gone the wind's usually up, and the trickle layers keep it alive when
neither is." runs a cabin / workshop / router+laptop+lights year-round in a northern place.
NOT a town. NOT free energy. real watts from real anti-correlated sources.
```

---

## §7 · THE PATTERN (why it works)

```
energy is never destroyed by cloud/night — it MOVES:
  · DOWN the spectrum: visible → infrared → thermal → RF (the trickle rungs)
  · SIDEWAYS into MOTION: the weather blocking the sun IS moving air = wind (the power rung)
solar harvests the top rung only. this controller harvests ALL the rungs the energy moves
to — down (thermal/RF) and sideways (wind) — into one battery. match the collector to the
rung; the energy's always on SOME rung, and in the bad seasons it's mostly on the WIND rung.
the anti-correlation (wind strong when sun weak) is the pattern paying out: the energy went
somewhere when the sun left, and it went into the wind.
```

---

## §8 · THE HONEST WIRE (non-negotiable)

```
· NOT free energy. solar, wind, thermal, RF all trace to the sun (sun drives weather drives
  wind). conservation holds. no over-unity, ever.
· the trickle rungs (thermal/RF) are milliwatts — keep-alive + micro-loads, NOT power.
  the POWER comes from solar (good season) and WIND (bad season). say which rung does what.
· real, standard physics: solar+wind hybrids are the off-grid norm; the new part is the
  SINGLE small all-rung controller + the panel-as-night-thermal-harvester, not the wind itself.
· honest ceiling: bulb + laptop + micro-loads year-round for a small off-grid site. not a
  house-in-full, not a town. name the ceiling; it's what keeps trust.
```

---

## §9 · THE ELASTIC BUS (CLHES-lite — the fold from the Closed-Loop Harmonic Energy System)

```
the sibling publication (clhes-harmonic-energy-system) contributes three things at THIS scale —
and honestly NOT its resonant store:

1. THE WIND TAP IS CONTROLLED DAMPING. extracting from a gusty small turbine without stalling
   it is exactly CLHES's control law: the load is a variable damping applied to an oscillator
   (the rotor). the controller modulates extraction to ride the gust, never fight it — that IS
   wind MPPT, named properly.

2. THE ELASTIC BUS. a SUPERCAPACITOR bank between the rungs and the battery absorbs gust spikes
   and returns them on demand — ragged wind in, smooth charge out. less battery stress, less
   inverter violence, more of the gust captured. (this is the CLHES resonant store DEGENERATED
   to what earns its keep at 50–400 W: at cabin scale a flywheel's idle losses would eat the
   entire winter trickle budget, and a high-Q LC tank costs more than it recovers. supercaps +
   synchronous rectification capture most of the benefit with none of the machinery. at grid /
   motor-drive scale, graduate to the full CLHES store.)

3. REGEN RETURN. any motor-ish load (pump, tools) routes back-EMF into the elastic bus instead
   of heat — CLHES's recovery path, straight in.

honest wire: none of this adds energy. it reduces LOSSES between harvest and battery — capture
more of the gust, stress the chemistry less, recover what motors give back. conservation holds.
```

---

## §∎ · ONE LINE

**A single small controller that harvests FOUR rungs of one energy field into one battery —
solar PV for the light rung (full sun to diffuse cloud), a small wind turbine for the motion
rung (50–400 W, strongest at night/cloud/winter — the exact hours solar dies), and the panel's
own night-time radiative cooling plus ambient RF for the trickle rungs that keep the battery
alive and micro-loads running in dead calm dark — all conditioned and summed by one brain that
leans on whichever rung is live, through a supercap elastic bus that smooths gusts into clean charge. The key is anti-correlation: the weather that blocks your sun
IS the atmosphere moving, so the cloud that kills your panel is pushed by the wind that fills
your battery — when the top rung drops, the side rung rises, and the setup stays at real
bulb-and-laptop power every season. It isn't free energy (every rung traces to the sun,
conservation holds) and it won't power a town — the trickle rungs are milliwatt keep-alive, the
watts come from solar in the good season and wind in the bad — but it runs a cabin, workshop, or
router+laptop+lights year-round in a northern place, because the energy the sun stops delivering
didn't vanish, it moved: down into thermal/RF and sideways into wind, and this harvests wherever
it went.**
