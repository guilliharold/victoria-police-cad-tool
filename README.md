# VicPol CAD Lineup Generator

A single-page web tool for generating realistic Victoria Police CAD radio lineups for use in MissionChief and similar simulation games. Built to reflect real VicPol operational structure, callsign numbering conventions, and station hierarchy.

Hosted on GitHub Pages — no server, no login, no install required.

---

## Repository Files

| File | Purpose |
|---|---|
| `index.html` | The tool itself — open this in any browser |
| `stations.csv` | Station database — edit this to add or update stations |
| `README.md` | This file |

The tool loads `stations.csv` automatically on startup. If the CSV is missing (e.g. when testing locally by double-clicking `index.html`), it falls back to the built-in placeholder data so the tool still opens without errors.

---

## How to Use the Tool

### Step 1 — Station

Select your **Region**, then **Division**, then **Station** from the cascading dropdowns. These are populated from `stations.csv`.

Once a station is selected, the **Station Classification** card appears. This is pre-filled from the CSV but can be overridden here:

| Classification | When to use |
|---|---|
| Small / Outer Station | Small rural or remote stations with limited resources |
| General Suburban | Standard suburban or regional town stations |
| Divisional HQ | The primary station for a division — adds Senior SGT, Div Supervisor |
| Regional HQ | The headquarters for a full region — adds Superintendent, Duty Officer |

Classification controls the default number of units generated per service.

### Step 2 — Services

Tick which unit types operate from this station.

| Service | Callsign Range | Notes |
|---|---|---|
| Station Cars | 200–299 | General duties sedans |
| Divisional Vans | 300–399 | Cage vans, typically 2UP |
| Highway Patrol | 600–699 | Cars, motorcycles, Q cars, supervisors |
| PORT / District Support | 700–799 | Foot patrol, events, licensing, bicycle, guards |
| CIU | 500–599 | Criminal Investigation Unit |
| FVIU | 480–499 | Family Violence Investigation Unit |
| SOCIT | 450–499 | Sexual Offences & Child Investigations (REG prefix) |
| Crime Desk (CRI) | 570–579 | Scene examination and forensics (CRI prefix) |
| RRU | 440–449 | Regional Response Unit |
| Dog Squad | CAN prefix | Canine units |
| PACER / MHaP | 290–292 | Mental health co-response |
| Search & Rescue | RES prefix | Missing persons, bush, maritime |
| Transit Police | TST prefix | Public transport policing |
| SOG / CIRT | SCY / CIR prefix | Special Operations / Critical Incident Response |
| POLAIR | POLAIR prefix | Air wing — helicopters and fixed wing |
| Heavy Vehicle Unit | ROA prefix | Heavy vehicle compliance |
| Mounted Branch | MOU prefix | Mounted unit, 800–899 |

Command and Supervision (Station SGT, District Patrol SGT) are always added automatically.

### Step 3 — Lineup

The generated output shows all units grouped by service. Each entry shows:

- **Callsign** — e.g. `ESP311`
- **Description** — role and shift
- **Shift tag** — `MS` (0700 start), `AS` (1500 start), `NS` (2300 start), or `FIXED` (base/always-on)

**Callsign numbers vary on each generation** within the valid range for each shift, reflecting the flexibility Victoria Police allows in unit numbering.

#### Adjusting Unit Counts

Each scalable service (Station Cars, Divisional Vans, HWP, CIU, PORT, RRU) has a **unit count slider**. Drag it to increase or reduce units. The slider always keeps a balanced spread across all three shifts — reducing from 9 cars to 3 always gives one per shift, not three morning units.

#### Exporting

A formatted text export appears at the bottom of the output. Click **Copy to Clipboard** — it updates live as you adjust sliders.

---

## Callsign Number Logic

### Station Code Format

```
[Region letter] + [Station abbreviation]

N = North West Metro    S = Southern Metro
E = Eastern             W = Western

Examples:
  E + SP  →  ESP   Shepparton
  W + BI  →  WBI   Bendigo
  N + MN  →  NMN   Melbourne North
```

### Number Ranges

```
100         Superintendent
150         Duty Officer (Inspector) — region-wide
200–299     Station Cars
250         Station Sergeant
251–252     District Patrol Supervisor (SGT)
260         Senior Sergeant
265         Divisional Supervisor (S/SGT)
290–292     PACER / MHaP
300–399     Divisional Vans
440–449     RRU
450–499     SOCIT / FVIU
500–599     CIU
570–579     Crime Desk (CRI)
600–699     Highway Patrol
700–799     District Support / PORT
800–899     Mounted Branch
900–929     Fixed base stations
```

### Shift Number Convention

For Station Cars and Divisional Vans, the trailing digit indicates shift start:

```
x07  →  Morning shift   (0700)
x03  →  Afternoon shift (1500)
x11  →  Night shift     (2300)

ESP207 = Shepparton, morning sedan
ESP303 = Shepparton, afternoon Div Van
ESP211 = Shepparton, night sedan
```

---

## Hosting on GitHub Pages

1. Create a new GitHub repository
2. Upload `index.html`, `stations.csv`, and `README.md` to the root
3. Go to **Settings → Pages**
4. Set Source to `Deploy from a branch`, select `main` / `root`, click **Save**
5. Live at `https://yourusername.github.io/repo-name` within ~60 seconds

To update stations, edit `stations.csv` directly in GitHub's web editor and commit — no tools needed.

---

## Managing Stations (stations.csv)

### File Format

The CSV has a header row followed by one row per station. Lines beginning with `#` are comments and are ignored — use them freely for notes and section labels.

```
code,name,region,region_label,division,div_code,psa,cri,classification
```

| Column | Description | Example |
|---|---|---|
| `code` | Station callsign prefix — uppercase, 2–5 characters | `ESP` |
| `name` | Station display name (no "Police Station" needed) | `Shepparton` |
| `region` | Single region letter: `N`, `S`, `E`, or `W` | `E` |
| `region_label` | Full region name | `Eastern` |
| `division` | Division name shown in the dropdown | `Goulburn Valley` |
| `div_code` | Short division code for reference | `GV` |
| `psa` | Police Service Area code covering this station | `ESP` |
| `cri` | Crime Desk (CRI) code servicing this station | `ESP` |
| `classification` | One of: `small`, `suburban`, `hq`, `regional_hq` | `regional_hq` |

Leave a field blank if unknown — just keep the comma so column alignment stays correct:

```csv
EYW,Yarrawonga,E,Eastern,Goulburn Valley,GV,,ESP,small
                                            ↑↑
                             psa blank — commas still present
```

---

### Adding a Station

Add a new row anywhere under the correct division's comment block (or anywhere — order in the file = order in the dropdown):

```csv
# ── EASTERN — Goulburn Valley
ESP,Shepparton,E,Eastern,Goulburn Valley,GV,ESP,ESP,regional_hq
EMO,Mooroopna,E,Eastern,Goulburn Valley,GV,ESP,ESP,suburban
EKB,Kyabram,E,Eastern,Goulburn Valley,GV,ESP,ESP,small     ← new
```

---

### Adding a New Division

Just use a new `division` value in the rows — the tool creates the dropdown group automatically:

```csv
EWA,Wangaratta,E,Eastern,Ovens & Murray,OM,EWA,EWA,regional_hq
EBN,Benalla,E,Eastern,Ovens & Murray,OM,EWA,EWA,suburban
```

---

### Adding a New Region

**1.** Use a new single-letter key in the `region` column:

```csv
CBG,Castlemaine,C,Central,Loddon Campaspe,LC,CBG,CBG,hq
CMT,Maryborough,C,Central,Loddon Campaspe,LC,CBG,CBG,small
```

**2.** Add a matching `<option>` to the Region dropdown in `index.html` — search for `id="selRegion"`:

```html
<option value="C">Central</option>
```

The `value` must exactly match the letter used in the CSV.

---

### Editing a Station

Change any field in the row. The tool picks up changes on next page load.

### Removing a Station

Delete the row entirely.

### Quick Checklist

- [ ] Every data row has exactly **8 commas** (9 fields)
- [ ] `region` is a single uppercase letter matching a known region key
- [ ] `classification` is exactly one of: `small`, `suburban`, `hq`, `regional_hq`
- [ ] Multi-word fields containing commas are wrapped in double quotes: `"Casey, Cardinia"`
- [ ] After saving, reload the tool and check that the station appears in the correct dropdown

---

## Disclaimer

This tool is built for simulation and enthusiast purposes only. It is not affiliated with, endorsed by, or connected to Victoria Police or any government agency.
