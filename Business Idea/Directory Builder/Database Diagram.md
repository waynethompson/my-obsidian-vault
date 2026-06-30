

# 📘 Database Diagram (Mermaid ERD)

```mermaid
erDiagram

    %% ============================
    %% USERS & ROLES
    %% ============================

    User {
        uuid id PK
        string name
        string email
        string phone
        datetime created_at
    }

    Role {
        uuid id PK
        string name
    }

    UserRole {
        uuid id PK
        uuid user_id FK
        uuid role_id FK
        uuid directory_id FK "nullable"
        uuid listing_id FK "nullable"
    }

    User ||--o{ UserRole : "has roles"
    Role ||--o{ UserRole : "assigned to"
    Directory ||--o{ UserRole : "scopes admin roles"
    Listing ||--o{ UserRole : "scopes listing_owner"


    %% ============================
    %% DIRECTORIES
    %% ============================

    Directory {
        uuid id PK
        uuid owner_id FK
        string name
        json schema
        json theme_settings
        boolean enable_messaging
        boolean enable_email_relay
        boolean enable_sms_relay
        boolean enable_ratings
        boolean enable_reviews
        boolean enable_claiming
        boolean enable_invites
        datetime created_at
    }

    User ||--o{ Directory : "owns"


    %% ============================
    %% LISTINGS
    %% ============================

    Listing {
        uuid id PK
        uuid directory_id FK
        json data
        string email
        string phone
        uuid claimed_by_user_id FK "nullable"
        string claim_status
        datetime created_at
    }

    Directory ||--o{ Listing : "contains"
    User ||--o{ Listing : "claimed_by (optional)"


    %% ============================
    %% INVITES
    %% ============================

    Invite {
        uuid id PK
        uuid listing_id FK
        string sent_to_email
        string sent_to_phone
        string invite_token
        string status
        datetime created_at
    }

    Listing ||--o{ Invite : "has invites"


    %% ============================
    %% MESSAGES (future-proof)
    %% ============================

    Message {
        uuid id PK
        uuid listing_id FK
        uuid directory_id FK
        string from_user_name
        string from_user_email
        string from_user_phone
        string channel
        string direction
        string message_body
        string status
        datetime created_at
    }

    Listing ||--o{ Message : "receives"
    Directory ||--o{ Message : "context"


    %% ============================
    %% REVIEWS (optional)
    %% ============================

    Review {
        uuid id PK
        uuid listing_id FK
        uuid user_id FK "nullable"
        int rating
        string review_body
        datetime created_at
    }

    Listing ||--o{ Review : "has reviews"
    User ||--o{ Review : "written_by (optional)"
```


---

# 🧩 Explanation of the Diagram

## 1. **Users & Roles**

You have a flexible RBAC system:

- `platform_admin`
- `directory_admin` (scoped to a directory)
- `listing_owner` (scoped to a listing)

`UserRole` allows scoping roles to directories or listings without hardcoding logic.

---

## 2. **Directories**

Each directory has:

- its own schema
- its own theme
- its own feature flags
- its own owner

This makes each directory feel like its own product.

---

## 3. **Listings**

Listings belong to directories and store:

- schema‑driven data
- email/phone for messaging
- claim status
- optional `claimed_by_user_id`

---

## 4. **Invites**

Directory owners can invite businesses to claim listings.

Tracks:

- email/phone
- token
- status

---

## 5. **Messages**

Even in v1 (simple relay), messages are stored as **outbound events**.

This future‑proofs:

- masked email
- SMS proxy
- full inbox
- threading
- analytics

---

## 6. **Reviews**

Optional, controlled by directory feature flags.

---

# 🧭 Next Step Options

If you want, I can generate:

- A **Prisma schema**
- A **Postgres SQL schema**
- A **Next.js folder structure**
- A **full API design**
- A **claiming flow sequence diagram**
- A **messaging relay sequence diagram**

Just tell me which direction you want to go.