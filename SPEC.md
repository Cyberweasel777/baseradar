# BaseRadar — Build Spec

## Product
Consumer-facing Base chain intelligence dashboard. Standalone HTML, no build step, dark theme, mobile-first.

## API Base
All data comes from `https://king-backend.fly.dev/api/botindex/`

### Existing Endpoints (confirmed working):
- `GET /api/botindex/zora/trending` — Top volume Zora coins on Base
- `GET /api/botindex/zora/new-coins` — Recently launched coins
- `GET /api/botindex/zora/coin/:address` — Individual coin detail
- `GET /api/botindex/tokens` — DexScreener token universe
- `GET /api/botindex/correlation-leaders` — Cross-token correlation
- `GET /api/botindex/whale-trades` — Whale activity feed
- `GET /api/botindex/intel/market-brief` — DeepSeek AI market brief (premium)
- `GET /api/botindex/intel/deepseek-signals` — AI-generated signals (premium)

## Pages to Build

### 1. index.html — Main Dashboard
Dark theme (#0a0a0f background), neon blue (#00d4ff) + green (#00ff88) accents.
Mobile-first responsive grid.

#### Layout:
- **Header:** BaseRadar logo/text, tagline "Base Chain Intelligence", [Get Pro] CTA button
- **Stats Bar:** Total coins tracked, 24h volume, active wallets (pull from trending endpoint)

#### Widgets (4 panels):

**Panel 1: 🔥 Trending Coins (FREE)**
- Table: Rank, Name, Symbol, Price, 24h Change %, Market Cap, Volume
- Data from `/zora/trending`
- Free users see top 5, Pro sees all
- Auto-refresh every 60s
- Click row to expand detail

**Panel 2: 🆕 New Launches (FREE — limited)**
- Card layout: coin name, time since launch, initial volume, creator
- Data from `/zora/new-coins`
- Free: last 3 coins only, 15-min delay
- Pro: real-time, full history, alert setup
- "🔒 See all launches in real-time" CTA for free users

**Panel 3: 🐋 Whale Activity (PAYWALLED)**
- Live feed: wallet address (truncated), amount, token, direction (buy/sell), timestamp
- Data from `/whale-trades`
- Free: show 2 entries with blur overlay on rest
- Pro: full feed, filtering, export
- "🔒 Unlock whale tracking" overlay for free users

**Panel 4: 📊 Correlation Heatmap (PAYWALLED)**
- Grid/matrix of top 10 tokens, color-coded correlation values
- Data from `/correlation-leaders`
- Free: blurred preview image
- Pro: interactive heatmap, click for pair detail
- "🔒 Unlock correlation analysis" overlay

#### Footer:
- "Powered by BaseRadar" + link to landing
- Links: Pro | API | Telegram (@MemeRadarBot cross-sell) | Twitter

### 2. landing.html — PPC Landing Page
Conversion-optimized for Google Ads traffic.

#### Structure:
- Hero: "Stop Missing Base Chain Alpha" + subhead + CTA
- Social proof: "X coins tracked", "Y in whale volume detected", "Z new launches per day"
- 3 feature cards: Trending Intelligence, Whale Tracking, New Launch Alerts
- Pricing: $9.99/mo with 7-day free trial, feature comparison (free vs pro)
- FAQ: 4-5 questions
- Final CTA: "Start Your Free Trial"

#### Tracking:
- GA4: G-W0MNKBJC2E
- Google Ads: AW-18000142406
- Conversion event on CTA click

### 3. success.html — Post-Checkout Thank You
- "Welcome to BaseRadar Pro!"
- Quick start guide (what to do first)
- Link back to dashboard

## Checkout Flow
- CTA buttons → `POST https://king-backend.fly.dev/api/baseradar/checkout`
  - Body: `{ "email": "optional" }`
  - Returns: `{ "url": "https://checkout.stripe.com/..." }`
  - Redirect to Stripe Checkout
- Success redirect: `https://baseradar.polyhacks.app/success.html`
- Cancel redirect: `https://baseradar.polyhacks.app/landing.html`

## Design Requirements
- Dark theme: bg #0a0a0f, cards #12121a, borders #1a1a2e
- Primary: #00d4ff (cyan/neon blue)
- Success: #00ff88 (neon green)
- Danger: #ff4444
- Font: system-ui, -apple-system, sans-serif
- All animations subtle (no flashy crypto bro vibes)
- Mobile: single column stack, touch-friendly
- Skeleton loading states for API data
- Error states for failed API calls

## Technical
- Vanilla HTML/CSS/JS, no frameworks, no build step
- Fetch API for all data calls
- LocalStorage for subscription state cache
- CSS Grid for dashboard layout
- No cookies banner needed (no cookies used)
