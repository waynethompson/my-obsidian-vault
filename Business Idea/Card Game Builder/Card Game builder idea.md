
# Card Game Builder Platform

## The Core Idea

A platform where anyone can design, playtest, and publish their own collectible card games — similar in style to Pokémon or Magic: The Gathering. The community plays and votes on the best games, and the top-ranked ones graduate to physical print runs that are sold in an online store. Creators earn a revenue share from sales.

---

## Why This Could Work

- TCG/CCG is a ~$15B+ global market dominated by a handful of gatekeepers (Wizards of the Coast, Nintendo)
- No accessible platform exists for indie creators to design and distribute card games at scale
- Print-on-demand technology has matured (MakePlayingCards, PrintNinja, DriveThruCards) — physical printing is no longer a capital-intensive barrier
- The creator economy model (YouTube, Roblox, itch.io) has proven that community curation + creator monetisation is a viable business
- Tabletop RPG crowdfunding (Kickstarter) proves there is pent-up demand for indie game content

---

## Target Users

| Persona | Who they are | What they want |
|---|---|---|
| **The Designer** | Hobbyist or indie game designer | Tools to build a game without coding; a way to monetise their creativity |
| **The Player** | TCG fan looking for new games | Discover fresh card games; play free online before buying physical |
| **The Collector** | Buys physical sets | Owns a limited-run set from a creator they backed early |
| **The Voter** | Community member | Influence which games get made; early access or discount for voting |

---

## Core Feature Set

### Phase 1 — Creator Studio (MVP)
- **Card editor**: drag-and-drop canvas to design card layouts (art upload, text fields, stat blocks, ability descriptions)
- **Card set builder**: group cards into sets; define rarity distributions (common / uncommon / rare / foil)
- **Rules engine configurator**: define win conditions, turn structure, deck rules (deck size, max copies per card)
- **Basic game templates**: pre-built rule skeletons inspired by common TCG patterns (attack/defence, resource management, lane combat)
- **Export**: download cards as print-ready PDF or PNG sheet (for personal printing)

### Phase 2 — Online Play
- **Digital playmat**: drag-and-drop card play; shared game state via websocket
- **Matchmaking**: quick match against other players of the same game
- **Spectator mode**: watch live games
- **Draft mode**: simulate booster pack opening and draft picks
- **Replay viewer**: review finished games

### Phase 3 — Community & Discovery
- **Game library**: browse all published games; filter by genre, mechanics, player count, complexity
- **Rating system**: upvote/downvote + written reviews
- **Trending / rising**: surface new games gaining traction fast
- **Playtesting queue**: creators can request public playtest sessions; community earns XP for providing feedback
- **Creator profiles**: showcase all published games, follower count, total plays

### Phase 4 — Physical Store & Print Pipeline
- **Print eligibility threshold**: games that hit a vote/play threshold enter "print consideration"
- **Community pre-order campaign**: if a game hits a pre-order target within 30 days, it prints
- **Print partner integration**: automated order flow with print-on-demand partner (MakePlayingCards API or similar)
- **Limited run model**: first print run is numbered/limited — drives collector scarcity
- **Online store**: purchase physical sets directly; creator earns 20–30% of net revenue
- **Booster pack randomisation**: buyers receive randomised booster packs (defined by creator's rarity distribution)

---

## Monetisation Model

| Revenue Stream | Mechanism |
|---|---|
| **Platform subscription (Creator Pro)** | Creators pay $9–$19/month for higher card limits, premium templates, analytics, custom card backs |
| **Print margin** | Platform marks up print cost; split remainder with creator |
| **Digital booster packs** | Players purchase digital packs to unlock full card sets online ($1–$3/pack) |
| **Featured placement** | Creators pay to feature their game in discovery |
| **Marketplace fees** | Secondary market for digital cards (optional later phase) |

---

## Creator Revenue Share

- Physical sales: creator earns **25% of net revenue** (after print + fulfilment cost)
- Digital pack sales: creator earns **50% of net revenue**
- Referral bonus: creator earns extra % for first 90 days on any player they refer

---

## AI-Assisted Design Features

- **Card art generation**: integrate image generation (Flux / SDXL) — creator prompts a scene, gets card art without needing a graphic designer
- **Balance analyser**: AI reviews card stats and flags potential broken combos or power outliers before publishing
- **Rules assistant**: AI helps write clear card text ("tap to deal 2 damage" → formats into standard templated language)
- **Flavour text generator**: AI writes lore snippets for each card based on creator's world description

---

## Competitive Landscape

| Product | Gap |
|---|---|
| **Tabletop Simulator** | Sandbox, no structured creator tools, no print-to-physical pipeline |
| **CardMaker / Card Creator apps** | Design only — no play engine, no community, no monetisation |
| **DriveThruCards** | Print-on-demand only — no design tools, no digital play |
| **Roblox (game analogy)** | Closest model — UGC + community + monetisation — but not card games |
| **itch.io** | Distribution only — no in-browser play, no physical print pipeline |

**Our moat**: the full loop — design → playtest → vote → print → sell — in a single platform.

---

## Key Risks & Mitigations

| Risk | Mitigation |
|---|---|
| IP / copyright (art looks like Pokémon) | Enforce original IP only via ToS; AI art generation reduces the temptation to copy; DMCA process |
| Rules engine complexity | Start with 2–3 battle-tested templates; don't try to build a universal TCG engine on day one |
| Print quality complaints | Partner with established print vendor; send creator a proof copy before public sale |
| Low creator adoption | Offer free tier; seed platform with 5–10 showcase games built by in-house team |
| Game never reaches print threshold | Make digital-only a good outcome too — not every game needs to print |

---

## Go-to-Market

1. **Launch with 5 seed games** — built by the team to demonstrate the platform and attract early players
2. **Target TCG Reddit/Discord communities** — r/custommagic, r/PokemonTCG fan-made communities have active creators
3. **Creator-first growth** — every creator who publishes tells their existing audience; word-of-mouth
4. **"First game to 1,000 votes gets a free print run"** — launch contest drives initial engagement
5. **Kickstarter for physical launch** — use crowdfunding for first major print batch; validates demand before scaling

---

## Technical Architecture

Following vault architecture defaults:

- **Backend**: C# / ASP.NET Core — REST API, game rules engine, user management, order pipeline
- **Frontend**: TypeScript / React SPA — card editor canvas (Fabric.js or Konva), game playmat UI
- **Realtime**: SignalR (ASP.NET) — shared game state for online play
- **Database**: PostgreSQL — cards, sets, users, votes, orders
- **AI layer**: Python — image generation integration, balance analyser, rules text formatter
- **Print pipeline**: Webhook-based integration with print partner API
- **File storage**: Azure Blob / S3 — card art images, print-ready PDF exports
- **Deployment**: Single modular monolith; deploy to Railway or Render to keep costs low early

---

## Milestone Roadmap

| Milestone | Target | Success Metric |
|---|---|---|
| M1 — Card Editor | Month 2 | Creator can design a 20-card set and export PDF |
| M2 — Online Play | Month 4 | Two players can play a full game in browser |
| M3 — Community Launch | Month 5 | 50 published games; 500 registered users |
| M4 — Physical Store | Month 8 | First game ships physical copies; creator earns first payout |
| M5 — AI Features | Month 10 | AI art gen + balance analyser live |

---

## Open Questions

- What rule templates to launch with? (attack/defence only vs. resource-based games)
- Should the vote threshold be transparent or algorithmic?
- Physical-only vs. also supporting digital trading/secondary market?
- Moderation: who reviews games before they go public?
- Should we allow co-creators (e.g., one person does art, another does rules)?
