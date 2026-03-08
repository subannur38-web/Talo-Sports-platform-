# CLAUDE.md — Talo Sports Platform

## Project Overview

Talo Sports Platform is a **React-based sports sponsorship marketplace** for the African sports ecosystem. The entire application is contained in a single `index.html` file — a self-contained, pre-compiled frontend with no build tooling or backend.

The platform connects sponsors with African sports properties and athletes, featuring:
- A **Sponsorship Marketplace** (47 listings across football, athletics, rugby, etc.)
- An **Athlete Economy** section showcasing verified athletes available for endorsements
- A **Market Intelligence** dashboard with sponsorship metrics
- An **Advisory Studio** for brand strategy services

---

## Repository Structure

```
Talo-Sports-platform-/
├── index.html        # Entire application (React component + styles + data)
└── CLAUDE.md         # This file
```

**Everything lives in `index.html`.** There is no build process, no `node_modules`, no separate JS/CSS files.

---

## Technology Stack

| Layer | Technology |
|---|---|
| UI Framework | React (functional components, hooks via `useState`) |
| Styling | Inline CSS string injected via `<style>` tag |
| Fonts | Google Fonts — Playfair Display (headings), DM Sans (body) |
| Data | Hardcoded mock arrays (`SPONSORSHIPS`, `ATHLETES`) |
| Deployment | Static HTML — serve directly, no build step |

---

## File Structure: `index.html`

The file is structured as follows (approximate line ranges):

| Lines | Content |
|---|---|
| 1 | HTML title comment + React import |
| 3–12 | `COLORS` design token object |
| 14–876 | `styles` — full embedded CSS string |
| 877–932 | `SPONSORSHIPS` data array (4 featured + listing counts) |
| 933–938 | `ATHLETES` data array (4 verified athletes) |
| 940–941 | `NAV_ITEMS` and `FILTER_TABS` arrays |
| 943–1330 | `TALOMarketplace` React component (JSX + state) |

---

## Design System

### Color Palette (`COLORS` object)

| Token | Hex | Usage |
|---|---|---|
| `gold` | `#C8881A` | Primary brand / CTAs / accents |
| `goldLight` | `#E8A830` | Hover states, highlights |
| `navy` | `#0D1B3E` | Cards, panels |
| `navyMid` | `#1A2F5E` | Sidebar, secondary panels |
| `dark` | `#080F1E` | Page background |
| `white` | `#F5F0E8` | Primary text |
| `muted` | `#8A95A8` | Secondary text, labels |
| `success` | `#2ECC8A` | Badges, verified status |

### Typography
- **Headings**: `Playfair Display` (700, 900 weight) — used for brand name and section titles
- **Body**: `DM Sans` (300, 400, 500, 600 weight) — used for all other text

### Visual Effects
- `backdrop-filter: blur(16px)` on the sticky navbar
- Noise overlay texture (SVG `feTurbulence` filter, 3% opacity) for premium feel
- CSS Grid for marketplace and athlete card layouts

---

## Component Architecture

There is **one top-level component**: `TALOMarketplace`

### State

```js
const [activeNav, setActiveNav] = useState("marketplace");
const [activeFilter, setActiveFilter] = useState("all");
const [selectedItem, setSelectedItem] = useState(null);      // Sponsorship modal
const [selectedAthlete, setSelectedAthlete] = useState(null); // Athlete modal
const [sidebarFilter, setSidebarFilter] = useState("all");
```

### Navigation Tabs (`NAV_ITEMS`)

| Key | Label | Status |
|---|---|---|
| `marketplace` | Marketplace | Fully implemented |
| `athletes` | Athletes | Fully implemented |
| `investments` | Investments | Coming Q4 2026 |
| `fan-economy` | Fan Economy | Coming Q4 2026 |
| `intelligence` | Intelligence | Coming Q4 2026 |

### Rendered Sections (by `activeNav`)

- **`marketplace`**: Sidebar filter + sponsorship cards grid + market intelligence metrics + advisory studio + summit CTA
- **`athletes`**: Athlete cards grid with modal for endorsement details

---

## Data Models

### Sponsorship Object

```js
{
  id: Number,
  title: String,           // e.g. "League Sponsorships"
  category: String,        // e.g. "Football"
  listings: Number,        // Total listing count
  featured: Boolean,
  packages: Array<{
    name: String,          // e.g. "Title Sponsor"
    price: String,         // e.g. "KES 2.5M - 5M/season"
    reach: String,
    features: Array<String>
  }>
}
```

### Athlete Object

```js
{
  id: Number,
  name: String,
  sport: String,
  event: String,
  followers: String,       // e.g. "45K"
  engagement: String,      // e.g. "8.2%"
  achievements: Array<String>,
  endorsements: Array<String>,  // Available categories
  fee: String,             // TALO management fee
  verified: Boolean
}
```

---

## Development Conventions

Since the app is a single HTML file, follow these conventions when making changes:

### Adding/Modifying Styles
- All CSS lives in the `styles` template literal (lines ~14–876)
- Use existing class names; do not add inline `style` attributes to JSX unless for dynamic values
- Respect the color tokens in the `COLORS` object — never hardcode hex values in JSX; reference `COLORS.gold`, etc.

### Adding Data
- New sponsorship records go in the `SPONSORSHIPS` array
- New athlete records go in the `ATHLETES` array
- Keep mock data realistic for the African sports market (use KES for prices, Kenyan/East African athletes)

### Adding Features
- All new UI sections should be conditional on `activeNav` (follow the existing pattern)
- New navigation tabs require adding to `NAV_ITEMS` and adding a render branch in the component
- Modals follow the pattern: `selectedItem` / `selectedAthlete` state → conditional render at bottom of component

### Styling Conventions
- Dark backgrounds: `#080F1E` (dark) or `#0D1B3E` (navy)
- Gold borders: `rgba(200,136,26,0.2)` for subtle, `#C8881A` for prominent
- Cards use `border-radius: 12px` or `16px`
- Hover effects on interactive elements (opacity changes, gold border highlights)

---

## No-Build Deployment

To serve the app locally:
```bash
# Python
python3 -m http.server 8080

# Node
npx serve .

# Or simply open index.html in a browser
```

There is no `npm install`, no compilation, no environment variables.

---

## Testing

There are currently **no automated tests**. When adding tests in the future:
- Use React Testing Library + Vitest (recommended for this React stack)
- Test state transitions (modal open/close, nav switching, filter application)

---

## Git Workflow

- Main production branch: `main`
- Feature branches follow the pattern: `claude/<description>-<id>`
- Commits should be descriptive and reference what section of `index.html` was changed

---

## Future Roadmap (per app UI)

- **Q4 2026**: Investments tab, Fan Economy tab, Intelligence tab
- **TALO Sports Data & Economy Summit** — Nairobi, Q4 2026
- Backend API integration (currently all data is hardcoded)
- Authentication for sponsors and athletes

---

## Key Contacts / Context

- **Target Market**: East African sports ecosystem (Kenya-centric)
- **Currency**: KES (Kenyan Shilling) for all pricing
- **Brand Voice**: Premium, data-driven, empowering African athletes
