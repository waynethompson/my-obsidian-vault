
Created: 21-04-2026
# Multi‑Tenant Directory Platform — Concept & Architecture Overview

## 1. Vision

A reusable, multi‑tenant **directory‑builder platform** built with Next.js.  
Users can create their own directories, import listings, allow businesses to claim their listings, and optionally enable messaging and ratings.

The platform starts simple but is architected for future expansion into a full structured‑data SaaS with communication rails.

---

## 2. Core Use Cases

### 2.1 For the Platform Owner

- Operate the SaaS
- Manage global settings
- Handle billing (future)
- Oversee abuse reports
- Seed initial directories (e.g., offshoring companies, suit shops)

### 2.2 For Directory Owners

- Create and manage directories
- Define schema (fields, categories, filters)
- Import listings via CSV
- Invite businesses to claim listings
- Enable or disable directory‑level features
- Customize directory branding
- View analytics (future)

### 2.3 For Listing Owners (Businesses)

- Claim their listing
- Verify ownership via email or SMS
- Edit their listing details
- Upload photos
- Respond to reviews (future)
- View message counts (future analytics)

### 2.4 For End Users

- Browse directories
- Filter and search listings
- Contact businesses via platform relay (if enabled)
- Leave ratings/reviews (if enabled)

---

## 3. Feature Overview

### 3.1 Directory‑Level Feature Flags (v1)

Each directory can toggle:

- `enable_messaging`
- `enable_email_relay`
- `enable_sms_relay`
- `enable_ratings`
- `enable_reviews`
- `enable_claiming`
- `enable_invites`

These apply to the entire directory.  
**No per‑listing overrides in v1.**

### 3.2 CSV Import

Directory owners can bulk import listings with fields such as:

- name
- address
- email
- phone
- website
- category
- description
- tags
- images (optional URLs)

Imported listings store email/phone for future invites and claiming.

### 3.3 Claiming System

Businesses can claim their listing by:

1. Searching for their listing
2. Clicking “Claim this listing”
3. Verifying via email or SMS
4. Becoming the `listing_owner` for that listing

Fallback: manual approval by directory owner.

### 3.4 Invite System

Directory owners can:

- Select listings
- Send invites via email or SMS
- Track invite status (sent, opened, accepted)

### 3.5 Messaging System (v1: Simple Relay)

If messaging is enabled:

- End users fill a contact form
- Platform sends message to business via email or SMS
- Business receives message directly
- No reply routing
- No inbox
- No threading

Messages are stored as **outbound message events** for future analytics.

### 3.6 Ratings & Reviews (Optional)

If enabled:

- Listings show star ratings
- Users can submit reviews
- Directory owner can moderate (future)

---

## 4. Architecture Overview

### 4.1 Roles

- `platform_admin`
- `directory_admin`
- `listing_owner`
- `public_user`

### 4.2 Data Model (Simplified)

#### Directories

- id
- name
- schema (JSON)
- theme settings
- feature flags
- owner_id

#### Listings

- id
- directory_id
- data (JSON based on schema)
- email
- phone
- claimed_by_user_id (nullable)
- claim_status

#### Messages (future‑proof schema)

- id
- listing_id
- directory_id
- from_user_name
- from_user_email
- from_user_phone
- message_body
- channel (email or sms)
- direction (“outbound” for v1)
- status
- created_at

#### Invites

- id
- listing_id
- invite_token
- sent_to_email
- sent_to_phone
- status
- created_at

---

## 5. Messaging System Roadmap

### v1 — Simple Relay (MVP)

- User → Business only
- No reply routing
- No inbox
- Store outbound events only

### v2 — Masked Relay

- Masked email addresses
- Masked SMS numbers
- Basic reply routing

### v3 — Full Inbox

- Two‑way messaging
- Threading
- Message history
- Notifications
- Analytics for listing owners

---

## 6. Directory Settings Panel (v1)

A simple UI where directory owners can toggle:

- Messaging
- Email relay
- SMS relay
- Ratings
- Reviews
- Claiming
- Invites

Defaults apply to new directories.

---

## 7. Development Roadmap

### Phase 1 — Core Platform

- Multi‑tenant directories
- Schema builder
- Listing CRUD
- Directory settings with feature flags
- CSV import
- Claim listing flow
- Invite system
- Simple messaging relay
- Ratings toggle (no reviews yet)

### Phase 2 — Enhancements

- Reviews
- Analytics
- AI‑generated descriptions
- AI‑generated SEO
- Map view
- Embeddable widgets
- Custom domains
- Premium tiers

### Phase 3 — Communication Layer

- Masked email/SMS
- Full inbox
- Threading
- Notifications
- Spam filtering
- Rate limiting

---

## 8. Guiding Principles

- **Start simple**: directory‑level toggles only
- **Schema‑driven**: listings adapt to directory schema
- **Future‑proof**: message events stored even in v1
- **Modular**: features can be added without refactoring
- **Multi‑tenant**: each directory feels like its own product

---

If you want, I can also generate:

- a **folder structure** for the repo
- a **database schema diagram**
- a **Next.js routing plan**
- a **spec document** for v1
- a **pitch deck style overview** for the SaaS

Just tell me what direction you want to take next.