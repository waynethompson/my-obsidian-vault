# Multi-Tenant Directory Builder Platform — Technical Specification

**Version:** 2.0.0  
**Last Updated:** April 2026  


---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Use Cases](#use-cases)
3. [Technology Stack](#technology-stack)
4. [Architecture](#architecture)
5. [Core Features](#core-features)
6. [Component Structure](#component-structure)
7. [Data Models](#data-models)
8. [User Interface Specifications](#user-interface-specifications)
9. [API Design](#api-design)
10. [SEO, CDN & Custom Domains](#seo-cdn--custom-domains)
11. [Setup and Installation](#setup-and-installation)
12. [Development Roadmap](#development-roadmap)
13. [Performance Requirements](#performance-requirements)
14. [Security Considerations](#security-considerations)

---

## Project Overview

The Multi-Tenant Directory Builder is a SaaS platform that lets users create and manage their own customizable business directories. It is built with Next.js and architected for multi-tenancy from the start — each directory behaves as its own product with its own schema, branding, and feature flags.

The platform starts as a simple relay-and-listing tool but is architected for future expansion into a full structured-data SaaS with communication rails.

### Goals

- Enable anyone to launch a fully branded business directory without code
- Support any industry vertical through schema-driven listings
- Allow businesses to claim and manage their own listings
- Provide messaging, ratings, and reviews as optional, per-directory features
- Scale to millions of listings with excellent SEO performance

### Target Audiences

- **Platform Owners**: Operators of the SaaS, managing global settings and billing
- **Directory Owners**: Users who create and configure directories (e.g., suit shops, dive stores)
- **Listing Owners (Businesses)**: Businesses that claim and manage their own listing
- **End Users**: People browsing directories to find and contact businesses

---

## Use Cases

### 2.1 For the Platform Owner

- Operate the SaaS and manage global settings
- Handle billing (future)
- Oversee abuse reports
- Seed initial directories (e.g., offshoring companies, suit shops, dive stores)

### 2.2 For Directory Owners

- Create and manage directories
- Define listing schema (fields, categories, filters)
- Import listings via CSV
- Invite businesses to claim listings
- Enable or disable per-directory feature flags
- Customize directory branding and theme
- View analytics (future)

### 2.3 For Listing Owners (Businesses)

- Claim their listing via email or SMS verification
- Edit listing details
- Upload photos
- Respond to reviews (future)
- View message counts (future analytics)

### 2.4 For End Users

- Browse and search directories
- Filter and sort listings
- Contact businesses via platform messaging relay (if enabled)
- Leave ratings and reviews (if enabled)

### Example Verticals

- Suit shop and custom tailor directories
- Scuba dive shop and equipment retailer directories
- Offshore software development agency directories
- Restaurant, healthcare, real estate, and professional service directories

---

## Technology Stack

### Frontend

| Technology | Version | Purpose |
|------------|---------|---------|
| **Next.js** | 14.x | React framework with SSR/SSG |
| **React** | 18.x | UI component library |
| **TypeScript** | 5.x | Type safety and developer experience |
| **Tailwind CSS** | 3.x | Utility-first CSS framework |
| **Heroicons** | 2.x | Icon library |
| **Lucide React** | 0.263.x | Additional icon set |

### Backend

| Technology | Purpose |
|------------|---------|
| **Prisma** | ORM and database toolkit |
| **PostgreSQL** | Primary database |
| **NextAuth.js** | Authentication solution |

### Infrastructure (Recommended)

| Service | Purpose |
|---------|---------|
| **Vercel** | Hosting and deployment |
| **Cloudflare** | CDN, caching, and custom domain routing |
| **AWS S3** | File and image storage |
| **Stripe** | Payment processing (future) |

---

## Architecture

### Multi-Tenancy Model

Each directory is a tenant. Routing is resolved by subdomain or custom domain:

- **Free plan**: `directoryname.yourplatform.com`
- **Paid plan**: `suitshopsbangkok.com` (custom domain via Cloudflare)

The Next.js app reads the `Host` header, resolves the `directory_id`, and renders the correct directory using SSG/ISR.

### Application Structure

```
saas-directory/
├── app/                          # Next.js app directory
│   ├── layout.tsx
│   ├── page.tsx                  # Platform homepage
│   ├── [directory]/              # Per-directory pages
│   │   ├── page.tsx              # Directory listing page
│   │   └── [listing]/
│   │       └── page.tsx          # Individual listing page
│   ├── dashboard/                # Directory owner dashboard
│   │   ├── directories/
│   │   ├── listings/
│   │   ├── invites/
│   │   └── settings/
│   ├── claim/                    # Listing claim flow
│   │   └── [token]/
│   │       └── page.tsx
│   ├── components/               # Shared components
│   │   ├── Header.tsx
│   │   ├── Hero.tsx
│   │   ├── SearchFilters.tsx
│   │   ├── DirectoryGrid.tsx
│   │   ├── DirectoryCard.tsx
│   │   ├── ClaimBanner.tsx
│   │   ├── ContactForm.tsx
│   │   └── Footer.tsx
│   └── api/                      # API routes
│       ├── directories/
│       ├── listings/
│       ├── invites/
│       ├── messages/
│       ├── reviews/
│       └── auth/
├── prisma/                       # Database schema
│   └── schema.prisma
├── lib/                          # Utility functions
│   ├── db.ts
│   ├── tenant.ts                 # Tenant resolution
│   ├── utils.ts
│   └── validation.ts
├── public/
├── types/
├── package.json
├── next.config.js
├── tailwind.config.js
└── tsconfig.json
```

### Design Patterns

- **Multi-Tenant Routing**: Tenant resolved from `Host` header at request time
- **Schema-Driven Listings**: Listing fields are defined per-directory via JSON schema
- **Feature Flags**: Each directory toggles features independently
- **Server Components**: Leverage Next.js server components for SEO performance
- **ISR**: Listing pages revalidated on-demand after edits
- **Type Safety**: End-to-end TypeScript

---

## Core Features

### 1. Multi-Tenant Directory Management

- Create and configure multiple directories from one platform account
- Each directory has its own schema, theme, feature flags, and owner
- Directories are served via subdomain (free) or custom domain (paid)

### 2. Directory-Level Feature Flags

Each directory independently toggles these features (no per-listing overrides in v1):

| Flag | Description |
|------|-------------|
| `enable_messaging` | Show contact form on listings |
| `enable_email_relay` | Relay messages via email |
| `enable_sms_relay` | Relay messages via SMS |
| `enable_ratings` | Show star ratings on listings |
| `enable_reviews` | Allow users to submit reviews |
| `enable_claiming` | Allow businesses to claim listings |
| `enable_invites` | Allow directory owner to send invite emails/SMS |

### 3. Schema-Driven Listings

Directory owners define the listing schema (fields, categories, filters) via a JSON schema. Listings store their data as a JSON blob validated against that schema. This allows any industry vertical without code changes.

### 4. CSV Import

Directory owners can bulk-import listings with fields such as:
- name, address, email, phone, website
- category, description, tags
- image URLs (optional)

Imported listings store email and phone for future invites and claiming.

### 5. Claiming System

Businesses can claim their listing:

1. Search for their listing
2. Click "Claim this listing"
3. Verify via email or SMS token
4. Become the `listing_owner` for that listing

Fallback: manual approval by directory owner.

`claim_status` values: `unclaimed`, `pending`, `claimed`

### 6. Invite System

Directory owners can:
- Select listings and send invites via email or SMS
- Track invite status: `sent`, `opened`, `accepted`, `expired`

### 7. Messaging System (v1 — Simple Relay)

If messaging is enabled on a directory:
- End users fill a contact form on a listing
- Platform sends the message to the business via email or SMS
- No reply routing, no inbox, no threading in v1
- All messages are stored as **outbound events** for future analytics

### 8. Ratings & Reviews (Optional)

- Controlled by per-directory feature flags
- Listings display star ratings when `enable_ratings` is on
- Users can submit text reviews when `enable_reviews` is on
- Moderation by directory owner (future)

### 9. Discovery & Search

- Full-text search across listing names, descriptions, and tags
- Category and schema-field filtering
- Sorting by rating, review count, name, recently added
- Location-based filtering (if location field in schema)

---

## Component Structure

### Component Hierarchy

```
App
├── Header
│   ├── Logo
│   ├── Navigation
│   └── UserMenu
├── Hero
│   └── SearchBar
├── SearchFilters
│   ├── CategoryFilter
│   ├── LocationFilter
│   ├── RatingFilter
│   └── FilterButton (mobile)
├── DirectoryGrid
│   ├── SortingControls
│   ├── DirectoryCard (multiple)
│   │   ├── Image
│   │   ├── BusinessInfo
│   │   ├── RatingDisplay
│   │   ├── Tags
│   │   └── ActionButtons
│   └── Pagination
└── Footer
    ├── FooterLinks
    ├── SocialLinks
    └── Newsletter
```

### Component Specifications

#### Header Component

**Props:**
- `isAuthenticated: boolean`
- `user?: User`

**State:**
- `isMenuOpen: boolean`

**Features:**
- Responsive mobile menu
- Sticky positioning on scroll
- User dropdown menu
- Sign in/out functionality

#### DirectoryCard Component

**Props:**
```typescript
interface DirectoryCardProps {
  listing: {
    id: number
    name: string
    category: string
    location: string
    rating: number
    reviewCount: number
    description: string
    image: string
    tags: string[]
    verified: boolean
  }
}
```

**Features:**
- Image hover effects
- Verification badge display
- Star rating visualization
- Tag display (max 3 visible)
- Click-through to detail page

#### SearchFilters Component

**State:**
```typescript
interface FilterState {
  category: string
  location: string
  rating: number
  priceRange: [number, number]
}
```

**Features:**
- Mobile collapsible filters
- Filter persistence in URL params
- Clear all filters option
- Active filter indicators

---

## Data Models

See [[Business Idea/Directory Builder/Database Diagram]] for the full ERD.

### User

```typescript
interface User {
  id: string          // uuid PK
  name: string
  email: string
  phone?: string
  created_at: Date
}
```

### Role

Roles: `platform_admin`, `directory_admin`, `listing_owner`

```typescript
interface Role {
  id: string          // uuid PK
  name: string        // platform_admin | directory_admin | listing_owner
}
```

### UserRole

Scopes a role to a specific directory or listing.

```typescript
interface UserRole {
  id: string
  user_id: string     // FK → User
  role_id: string     // FK → Role
  directory_id?: string  // FK → Directory (nullable — scopes directory_admin)
  listing_id?: string    // FK → Listing (nullable — scopes listing_owner)
}
```

### Directory

```typescript
interface Directory {
  id: string
  owner_id: string    // FK → User
  name: string
  schema: object      // JSON — defines listing fields for this directory
  theme_settings: object
  enable_messaging: boolean
  enable_email_relay: boolean
  enable_sms_relay: boolean
  enable_ratings: boolean
  enable_reviews: boolean
  enable_claiming: boolean
  enable_invites: boolean
  created_at: Date
}
```

### Listing

```typescript
interface Listing {
  id: string
  directory_id: string          // FK → Directory
  data: object                  // JSON — fields defined by directory schema
  email?: string
  phone?: string
  claimed_by_user_id?: string   // FK → User (nullable)
  claim_status: 'unclaimed' | 'pending' | 'claimed'
  created_at: Date
}
```

### Invite

```typescript
interface Invite {
  id: string
  listing_id: string   // FK → Listing
  sent_to_email?: string
  sent_to_phone?: string
  invite_token: string
  status: 'sent' | 'opened' | 'accepted' | 'expired'
  created_at: Date
}
```

### Message

```typescript
interface Message {
  id: string
  listing_id: string    // FK → Listing
  directory_id: string  // FK → Directory
  from_user_name: string
  from_user_email: string
  from_user_phone: string
  channel: 'email' | 'sms'
  direction: 'outbound' // v1 only; 'inbound' future
  message_body: string
  status: 'sent' | 'failed'
  created_at: Date
}
```

### Review

```typescript
interface Review {
  id: string
  listing_id: string  // FK → Listing
  user_id?: string    // FK → User (nullable — anonymous allowed)
  rating: number      // 1–5
  review_body: string
  created_at: Date
}
```

---

## User Interface Specifications

### Design System

**Color Palette**

```css
Primary Colors:
- primary-50: #eff6ff
- primary-500: #3b82f6
- primary-600: #2563eb
- primary-700: #1d4ed8

Neutral Colors:
- gray-50: #f9fafb
- gray-200: #e5e7eb
- gray-600: #4b5563
- gray-900: #111827

Semantic Colors:
- success: #10b981
- warning: #f59e0b
- error: #ef4444
- info: #3b82f6
```

**Typography**

```css
Font Family: Inter (Google Fonts)

Headings:
- h1: text-4xl md:text-6xl font-bold
- h2: text-2xl font-bold
- h3: text-lg font-semibold
- h4: text-md font-semibold

Body:
- Large: text-xl
- Regular: text-base
- Small: text-sm
- Extra Small: text-xs
```

**Spacing Scale**

```css
- xs: 0.25rem (4px)
- sm: 0.5rem (8px)
- md: 1rem (16px)
- lg: 1.5rem (24px)
- xl: 2rem (32px)
- 2xl: 3rem (48px)
```

**Border Radius**

```css
- sm: 0.25rem
- md: 0.5rem
- lg: 0.75rem
- xl: 1rem
```

### Responsive Breakpoints

```css
- sm: 640px
- md: 768px
- lg: 1024px
- xl: 1280px
- 2xl: 1536px
```

### Component Styling Guidelines

**Cards**
- Background: white
- Border: 1px solid gray-200
- Border radius: xl (1rem)
- Padding: 1.5rem (24px)
- Shadow: sm on default, md on hover
- Transition: all 200ms

**Buttons**

Primary Button:
```css
background: primary-600
hover:background: primary-700
color: white
padding: 0.5rem 1rem
border-radius: 0.5rem
font-weight: medium
```

Secondary Button:
```css
background: gray-200
hover:background: gray-300
color: gray-900
padding: 0.5rem 1rem
border-radius: 0.5rem
font-weight: medium
```

**Forms**
- Input height: 2.5rem (40px)
- Input padding: 0.75rem (12px)
- Border: 1px solid gray-300
- Focus ring: 2px primary-500
- Border radius: 0.5rem

### Accessibility Requirements

- WCAG 2.1 Level AA compliance
- Minimum color contrast ratio: 4.5:1 for text
- Keyboard navigation support
- Screen reader compatibility
- ARIA labels on interactive elements
- Focus indicators on all focusable elements
- Alt text for all images

---

## API Design

### RESTful Endpoints

#### Directories

```
GET    /api/directories              # List directories (platform admin)
POST   /api/directories              # Create directory (auth required)
GET    /api/directories/:id          # Get directory details
PUT    /api/directories/:id          # Update directory settings (auth required)
DELETE /api/directories/:id          # Delete directory (auth required)
```

#### Listings

```
GET    /api/directories/:id/listings            # List listings in directory (with filters)
POST   /api/directories/:id/listings            # Create listing (auth required)
GET    /api/directories/:id/listings/:listingId # Get single listing
PUT    /api/directories/:id/listings/:listingId # Update listing (auth required)
DELETE /api/directories/:id/listings/:listingId # Delete listing (auth required)
POST   /api/listings/:id/claim                  # Initiate claim flow
PATCH  /api/listings/:id/claim/verify           # Verify claim token
```

**Query Parameters for listing search:**
```
?search=keyword
&category=technology
&rating=4
&sort=rating
&page=1
&limit=20
```

#### Invites

```
GET    /api/directories/:id/invites             # List invites
POST   /api/directories/:id/invites             # Send invite to listing owner
PATCH  /api/invites/:token/open                 # Mark invite opened
PATCH  /api/invites/:token/accept               # Accept invite (claim listing)
```

#### Messages

```
POST   /api/listings/:id/messages               # Send message via relay (public)
GET    /api/directories/:id/messages            # List messages (auth required)
```

#### Reviews

```
GET    /api/listings/:id/reviews                # Get reviews for listing
POST   /api/listings/:id/reviews                # Submit review (auth required)
DELETE /api/reviews/:id                         # Delete review (auth required)
```

#### Auth

```
POST   /api/auth/register         # Register new user
POST   /api/auth/login            # Login user
POST   /api/auth/logout           # Logout user
GET    /api/user/profile          # Get user profile
PUT    /api/user/profile          # Update user profile
```

### Response Format

**Success Response:**
```json
{
  "success": true,
  "data": {},
  "meta": {
    "page": 1,
    "limit": 20,
    "total": 150,
    "totalPages": 8
  }
}
```

**Error Response:**
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": {
      "field": "email",
      "issue": "Invalid email format"
    }
  }
}
```

---

## SEO, CDN & Custom Domains

See [[Business Idea/Directory Builder/SEO, CDN, Domains]] for full detail.

### SEO Strategy

SEO is handled entirely at the **Next.js layer**. Key mechanisms:

- **SSG** for stable directory and listing pages (fully cached at CDN edge)
- **ISR** for pages that update after edits (revalidated on demand)
- **SSR** for dashboards and admin (not cached)
- Metadata API for titles, descriptions, and OpenGraph
- JSON-LD structured data for rich search results
- Automatic sitemap generation

### CDN Caching

Cloudflare is the preferred CDN. All directory pages, listing pages, and static assets are cached at the edge for global low-latency delivery.

### Custom Domains

| Plan | Domain |
|------|--------|
| Free | `directoryname.yourplatform.com` (subdomain, instant, no SSL setup) |
| Paid | `yourdomain.com` (custom domain via Cloudflare Custom Hostnames API) |

**Custom domain flow:**
1. Directory owner enters their domain
2. Platform generates a DNS verification token
3. Owner adds TXT record to their DNS
4. Cloudflare verifies ownership and provisions SSL automatically
5. Traffic is routed to the Next.js app with correct `Host` header
6. Platform resolves `directory_id` from `domains` table and renders the directory

### Domains Table

```
domains
  id
  directory_id  FK → Directory
  domain
  verification_status
  ssl_status
  created_at
```

---

## Setup and Installation

### Prerequisites

- Node.js 18.x or higher
- npm or yarn package manager
- Git

### Installation Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/saas-directory.git
   cd saas-directory
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Configure environment variables**
   ```bash
   cp .env.example .env.local
   ```

   Edit `.env.local`:
   ```env
   DATABASE_URL="postgresql://user:password@localhost:5432/directory"
   NEXTAUTH_SECRET="your-secret-key"
   NEXTAUTH_URL="http://localhost:3000"
   CLOUDFLARE_API_TOKEN="..."
   SENDGRID_API_KEY="..."       # or Resend, Postmark
   TWILIO_ACCOUNT_SID="..."     # for SMS relay
   TWILIO_AUTH_TOKEN="..."
   AWS_S3_BUCKET="..."
   ```

4. **Set up database**
   ```bash
   npx prisma migrate dev
   npx prisma db seed
   ```

5. **Run development server**
   ```bash
   npm run dev
   ```

6. **Open application**
   ```
   http://localhost:3000
   ```

### Build for Production

```bash
npm run build
npm run start
```

### Docker Setup (Optional)

```bash
docker-compose up -d
```

---

## Development Roadmap

### Phase 1 — Core Platform (MVP)

- [ ] Multi-tenant directory creation and management
- [ ] Schema builder (JSON-based listing fields)
- [ ] Listing CRUD
- [ ] Directory settings with feature flag toggles
- [ ] CSV import for bulk listing creation
- [ ] Claim listing flow (email + SMS verification)
- [ ] Invite system (send invites, track status)
- [ ] Simple messaging relay (email + SMS, no inbox)
- [ ] Ratings toggle (star ratings, no reviews yet)
- [ ] Subdomain routing per directory
- [ ] SSG/ISR pages for SEO

### Phase 2 — Enhancements

- [ ] Reviews (text + moderation)
- [ ] Analytics dashboard for directory owners
- [ ] AI-generated listing descriptions
- [ ] AI-generated SEO metadata
- [ ] Map view for location-based listings
- [ ] Embeddable widgets (iframe or JS)
- [ ] Custom domain support (Cloudflare Custom Hostnames)
- [ ] Premium subscription tiers (Stripe)

### Phase 3 — Communication Layer

- [ ] Masked email relay (privacy-first messaging)
- [ ] Masked SMS numbers
- [ ] Basic reply routing
- [ ] Full inbox (two-way messaging)
- [ ] Message threading
- [ ] Notifications (email + push)
- [ ] Spam filtering and rate limiting

---

## Performance Requirements

### Page Load Metrics

- **First Contentful Paint (FCP)**: < 1.5s
- **Largest Contentful Paint (LCP)**: < 2.5s
- **Time to Interactive (TTI)**: < 3.5s
- **Cumulative Layout Shift (CLS)**: < 0.1
- **First Input Delay (FID)**: < 100ms

### Optimization Strategies

**Image Optimization**
- Use Next.js Image component
- Lazy loading for off-screen images
- WebP format with fallbacks
- Responsive images with srcset
- Image compression (max 100KB per image)

**Code Splitting**
- Route-based code splitting (automatic with Next.js)
- Dynamic imports for heavy components
- Tree shaking for unused code

**Caching**
- Static page generation where possible
- API response caching (Redis)
- CDN for static assets
- Browser caching headers

**Database**
- Query optimization with indexes
- Connection pooling
- Read replicas for scaling
- Pagination for large datasets

### Scalability Targets

- Support 10,000+ listings
- Handle 100,000+ monthly active users
- Serve 1M+ page views per month
- API response time < 200ms (p95)

---

## Security Considerations

### Authentication & Authorization

- Secure password hashing (bcrypt)
- JWT tokens with short expiration
- Session management
- Multi-tenant RBAC via `UserRole` — roles scoped to platform, directory, or listing
- OAuth2 integration (Google, Facebook)
- Two-factor authentication (2FA)

### Data Protection

- Input validation and sanitization
- SQL injection prevention (Prisma ORM)
- XSS protection
- CSRF tokens
- Rate limiting on API endpoints
- File upload validation

### Privacy

- GDPR compliance
- Cookie consent management
- Privacy policy and terms of service
- Data export functionality
- Right to deletion
- Anonymized analytics

### Infrastructure

- HTTPS/SSL certificates
- Environment variable protection
- API key rotation
- Regular security audits
- Dependency vulnerability scanning
- Error logging (no sensitive data)

---

## Testing Strategy

### Unit Tests

- Component testing with React Testing Library
- Utility function tests
- API route handler tests
- Database model tests

### Integration Tests

- API endpoint integration tests
- Authentication flow tests
- Payment flow tests
- Email delivery tests

### End-to-End Tests

- User registration and login flows
- Listing creation and editing
- Review submission flow
- Search and filter functionality

### Performance Tests

- Load testing with k6
- Stress testing
- Database query performance
- API endpoint benchmarks

---

## Guiding Principles

- **Start simple** — directory-level toggles only; no per-listing overrides in v1
- **Schema-driven** — listings adapt to any directory schema without code changes
- **Future-proof** — message events stored even in v1 to enable analytics and inbox later
- **Modular** — features can be added without refactoring core models
- **Multi-tenant** — each directory feels like its own product with its own domain and brand
- **SEO-first** — all public pages are SSG/ISR; the CDN does the heavy lifting