# SF Schools Map

An interactive map of San Francisco public and private K-12 schools to help families with SFUSD lottery decisions and neighborhood selection.

**Live site: https://michellegeng.github.io/sf-schools-map**

## What It Does

- Browse all 240+ SF public, charter, and private schools on an interactive map
- Filter by school type, level, language program, CA Dashboard rating, and tuition range
- Search by school name or address
- Use "Near Me" to find schools within a set radius of any address
- View SFUSD K lottery demand data — requests, assignments, and success rates by tiebreaker category
- Toggle attendance area boundaries for SFUSD elementary schools
- See TK feeder arrows showing which preschools feed into which elementary schools
- See ES→MS feeder arrows showing which elementary schools feed into which middle schools
- View after-school / CBO program availability per school
- View CA Dashboard ELA and Math ratings for public and charter schools
- View tuition for 71 private schools

## Data Sources

| Data | Source |
|------|--------|
| Public schools (132) | CA Dept of Education |
| Private schools (108) | CA Dept of Education |
| CA Dashboard ratings | CDE 2024 ELA/Math data |
| Private school tuition | SF Chronicle 2025 |
| Attendance boundaries | SF Open Data API (live) |
| SFUSD K lottery rates | SFUSD Main Round Success Rates 2023–2026 |
| After-school programs | SFUSD ExCEL / ELO-P SY25-26 |
| TK feeder assignments | sfusd.edu/TKFeeder |
| ES→MS feeder assignments | SFUSD feeder pattern |
| Catholic school enrichment | SF Catholic Schools |

## Map Legend

**Color = school level**
- Purple: Preschool
- Green: Elementary
- Teal: K-8
- Orange: Middle
- Red: High School
- Dark: K-12

**Shape = school type**
- Circle: Public
- Diamond: Charter
- Square: Independent (private)
- Triangle: Parochial

## Technical Notes

- Single self-contained HTML file — no backend, no build step, just open in browser
- Built with Leaflet.js, CartoDB Positron basemap, vanilla JavaScript
- No API keys required
- Geocoding via Nominatim (OpenStreetMap)
