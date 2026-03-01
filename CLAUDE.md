# SF Schools Map

## Goal
Interactive HTML map of SF public/private K-12 schools to help with SFUSD lottery decisions and neighborhood selection.

## Output
- Single self-contained file: `sf_schools_map.html`
- No backend, no dependencies to install — just open in browser

## Tech Stack
- Leaflet.js (CDN), CartoDB Positron basemap (muted, free, no API key)
- Vanilla JavaScript, HTML, CSS
- All data embedded directly in the HTML file

## Data Sources
- **Public schools**: CA Dept of Education Excel (`SF Schools from Cal Department of Education.xlsx`) — 132 schools
- **Private schools**: CA Dept of Education Excel (`SF Private Schools.xlsx`) — 108 schools (2 skipped, no coords)
- **CA Dashboard ratings**: CDE 2024 ELA/Math data — 112 public/charter schools matched by name
- **Tuition**: SF Chronicle 2025 article — 71 private schools matched, shown as max cost/yr
- **Attendance boundaries**: SF Open Data API (live GeoJSON fetch), toggled on/off
- **SFUSD K lottery**: SFUSD Main Round Success Rates [Public].xlsx — 3-yr avg (2023-24, 2024-25, 2025-26); 70 public elementary schools; shows total requests, assigned, % assigned, % success; segmented by All / Attendance Area / No Tiebreaker / CTIP1; split by General Ed vs specialty/immersion program where applicable

## Map Design
- **Colors by level**: purple=preschool, green=elementary, teal=K-8, orange=middle, red=high, dark=K-12
- **Shape by type**: circle=public, diamond=charter, square=independent, triangle=parochial
- **Marker size**: 16px with thick white border and drop shadow
- **Popups**: name, address, level, grades, type, tuition (private), CA Dashboard rating (public/charter), SFUSD enrollment demand + attendance area odds (public/charter), website link

## Features Implemented
- [x] Search by name or address
- [x] Type filter: All / Public / Charter / Private (all) / Independent / Parochial
- [x] Level filter: multiselect — Pre-K (button label), Elementary, K-8, Middle, High
- [x] Rating filter: multiselect — CA Dashboard colors 1-5 (public/charter only)
- [x] Tuition range slider: dual-handle, $0–$80K+ (private schools only)
- [x] Attendance area boundaries toggle (SFUSD elementary)
- [x] Near Me radius filter: enter address → geocode via Nominatim (OSM, no API key) → draw circle + filter to schools within 0.5/1/2/5 mi; Clear button removes filter; haversineDistance used for distance calc
- [x] School count bar
- [x] Legend
- [x] Specialty Programs filter (formerly "Language Immersion Program"): multiselect — Spanish, Cantonese, Mandarin, Korean, Japanese (FLES), French, Italian, German Immersion + Arts + Montessori; applies to all school types
  - **Public/charter** (20 schools): Alvarado, Bryant, Buena Vista/Horace Mann, Chavez, Flynn, Harte, Revere, Sanchez (Spanish); Chinese Immersion at DeAvila, West Portal, Alice Fong Yu (Cantonese); Ortega, Starr King (Mandarin); Lilienthal/Claire (Korean); Clarendon (Japanese); Ruth Asawa SOTA, Creative Arts Charter, New Traditions, Rooftop (Arts); SF Public Montessori (Montessori)
  - **Private** (9 schools): CAIS, Presidio Knolls, Bert Hsu (Mandarin); Lycée Français Ashbury, Lycée Français Ortega, The International School of SF (French); La Scuola (Italian); German International School (German); The Dahlia School (Spanish + Montessori)

## Public Pre-School Schools (prek: true flag)
Filter label: "Public Pre-School" (sidebar row label); level button stays "Pre-K"
Private: Adda Clevenger, Children's Day School, CAIS, Ecole Notre Dame Des Victoires,
Eureka Learning Center, La Scuola, Lycée Français (Ashbury campus), Presidio Hill,
Presidio Knolls, St. Cecilia Elementary, SF Waldorf, Synergy School, The SF School
Public (on-site TK): SF Public Montessori, Alvarado, Buena Vista/Horace Mann K-8, Huerta, Marshall, Ulloa, Sunnyside, Monroe, Peabody, Taylor (Edward R.), Yick Wo, Lilienthal, Mission Education Center
Public (on-site pre-K at elementary): Cobb, Harte, Drew, Grattan, Muir, Guadalupe, Carmichael/FEC, Ortega, Sheridan
Standalone EES (level: preschool): 17 entries — Commodore Stockton, Noriega, San Miguel, Raphael Weill, Tule Elk Park, McLaren, Leola Havard, Theresa Mahler, Las Americas, Rodriguez (Zaida), J. Serra Annex, Mission Bay, Jefferson, Argonne, Tenderloin (+ Presidio Early Ed and Cooper/Sarah B which predate this update)
`tuitionBased: true` field on all prek:true public schools where data is known. Source: sfusd.edu/early-education/pre-kindergarten/list-preschool-choices
Popup shows "Pre-K enrollment: Tuition-based (open to all) / Income-restricted (subsidized/free)" for public schools with prek:true

## SFUSD TK Feeder System (2026-27)
Source: sfusd.edu/TKFeeder — `const tkFeeders = {...}` — maps school name to:
- EES entries: `feedsTo: [{name, type, area}]` — array of target elementary schools with per-target type ("GE" or "Spanish Immersion") and attendance area tiebreaker
- On-site entries: `onsite` ("GE" or "Spanish Immersion"), `area` (tiebreaker or "citywide") — only 3 schools confirmed on-site per table notes
EES → elementary school feeders (source: updated SFUSD table):
- Argonne EES → Jefferson (GE, Jefferson ESAA)
- Commodore Stockton EES → Chin (GE, Chin ESAA)
- McLaren EES → Cleveland (GE), Taylor (GE), Buena Vista/HM (Spanish Immersion, citywide)
- Mission Education Center → Alvarado (Spanish Immersion), Huerta (Spanish Immersion), Marshall (Spanish Immersion) — all citywide
- Noriega EES → Sunset (GE), Ulloa (GE)
- Rodriguez EES → Webster (Spanish Immersion, citywide)
- San Miguel EES → Sloat (GE), Sunnyside (GE)
- Serra Annex EES → Glen Park (GE), Monroe (GE)
- Tule Elk Park EES → New Traditions (GE), Peabody (GE), Yick Wo (GE), Lilienthal (GE, citywide)
On-site TK schools (per table notes): Alvarado (GE), Huerta (Spanish Immersion, citywide), Taylor (GE)

## Key JS Variables
- `const privateSchools = [...]` — 107 private schools
- `const schools = [...]` — 132 public/charter schools
- `const caRatings = {...}` — name → {ela_color, ela_status, math_color, math_status}
- `const sfusdK = {...}` — name → {ge, immersion, immLabel}; ge/immersion each: {all, esaa, ctip1, notb}; each segment: {req, asgn, pct, succ}; 70 public elementary schools; 3-yr avg K entry; pct=% assigned, succ=% got this or higher choice; immLabel = language name (e.g. "Spanish Immersion"); ge or immersion may be null if school has only one type
- `const tuition = {...}` — name → annual tuition (71 entries)
- `const allSchools = [...privateSchools, ...schools]`
- `let activeFilters = { type, level (Set), rating (Set), program (Set) }`
- Tuition slider: `tuitionMinEl`, `tuitionMaxEl` (id: tuition-min, tuition-max)

## Known Gaps / Possible Future Work
- Golden Bridges School: in Chronicle data but not in CDE private school list
- 32 private schools have no tuition data (not in Chronicle)
- Pre-K offerings for non-Chronicle schools unverified
- Could add clustering for dense areas (Leaflet.markercluster plugin)
- Could add walk score / transit data
- **School tour dates** (revisit Sept–Nov 2026): Sampled in Feb 2026 — data IS findable per school website but the 2025-26 admissions tour season is largely over. Next cycle dates won't be posted until fall 2026. Good candidate for an agent that searches each school individually (~239 schools). SFUSD central tour page: https://www.sfusd.edu/schools/enroll/discover/sfusd-school-tours
