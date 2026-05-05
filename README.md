# Geography Drill

An interactive geography quiz site built with vanilla HTML/CSS/JS and D3. Name countries, capitals, US cities, metros, and every city/town in South Carolina — with a live map that fills in as you type.

## Quizzes

- **Countries of the World** — All 197 sovereign countries
- **World Capitals** — Every capital, by continent or all at once
- **Top 200 US Cities** — By 2024 Census estimates
- **Top 3 Cities per State** — 150 answers across all 50 states
- **Top 100 US Metros** — OMB Bulletin 23-01 delineations
- **Every SC City & Town** — 374 places, down to towns of 38 residents

Most quizzes offer easy mode (map shown while playing) and hard mode (map hidden until you finish).

## Project Structure

```
.
├── index.html          # All HTML, CSS, and quiz logic
└── data/               # Quiz data (loaded as window.* globals)
    ├── top200.js       # Top 200 US cities
    ├── sc200.js        # All SC cities/towns
    ├── top3.js         # Top 3 per state
    ├── countries.js    # Countries + continent groupings
    ├── metros100.js    # Top 100 US metros (with FIPS county arrays)
    └── capitals.js     # World capitals
```

## How It Works

Data files are loaded via `<script>` tags before the main inline script and exposed as `window.TOP200`, `window.COUNTRIES`, etc. The main script in `index.html` binds them to local `const`s and runs the quiz logic.

**Script load order matters** — data files must come before the main script. Don't add `defer` or `type="module"` to those tags or it will break.

Maps are rendered with [D3](https://d3js.org/) and [TopoJSON](https://github.com/topojson/topojson). Country/state boundaries fill in as the user names answers.

## Data Format

Each data file exports one or more globals on `window`. Most use a simple array-of-arrays format:

```js
// Cities: [name, state, population, lat, lon]
["Charleston", "SC", 157665, 32.83, -79.97]

// Countries: [name, isoCode, [aliases], population]
["United States", "840", ["USA", "US", "America"], 343477335]

// Metros: [displayName, population, [aliases], [5-digit county FIPS codes]]
["New York–Newark–Jersey City, NY-NJ", 19940274, ["New York","NYC"], ["36005", "36047", ...]]
```

## Adding a New Quiz

1. Create a new data file in `data/` and assign your data to `window.YOURDATA`.
2. Add a `<script src="data/yourdata.js"></script>` tag in `index.html` (with the other data tags).
3. Bind it: `const YOURDATA = window.YOURDATA;` in the local consts block.
4. Add an entry to the `MODES` array.
5. Write a `renderYourQuiz()` function following the pattern of the existing quizzes.

## Data Sources

- US cities: 2024 US Census Bureau population estimates
- SC cities: 2024 ACS 5-Year estimates
- US metros: OMB Bulletin 23-01 (July 2023 delineations), 2024 Census estimates
- Countries: ISO 3166-1 numeric codes; populations from 2024 UN/World Bank estimates
- World map: [world-atlas](https://github.com/topojson/world-atlas)
- US map: [us-atlas](https://github.com/topojson/us-atlas)

## Running Locally

It's a static site — no build step. Just open `index.html` in a browser, or serve the folder with any static file server:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deployment

Hosted via GitHub Pages from the `main` branch.
