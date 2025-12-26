# Ela and the Missing Message

## Overview

Ela and the Missing Message is an interactive educational web game that teaches English Language Arts (ELA) to students ages 8-13. The game features time-traveling adventures where students learn grammar, sentence structure, and punctuation through engaging gameplay with characters Ela, Sam, Paul, and Tom.

The application is a full-stack TypeScript project with a React frontend and Express backend, using PostgreSQL for data persistence and Stripe for subscription payments.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Build Tool**: Vite with custom plugins for Replit integration
- **Routing**: Wouter (lightweight React router)
- **State Management**: TanStack React Query for server state
- **Styling**: Tailwind CSS v4 with custom theme variables (vibrant color palette including amber-gold, neon-pink, blue-violet, azure-blue)
- **UI Components**: Shadcn/ui component library with Radix UI primitives
- **Fonts**: Fredoka (display), Quicksand (body), Creepster (spooky effects)

### Backend Architecture
- **Framework**: Express.js with TypeScript
- **Authentication**: Replit OpenID Connect (OAuth) with Passport.js
- **Session Management**: PostgreSQL-backed sessions via connect-pg-simple
- **API Pattern**: RESTful endpoints under `/api/` prefix

### Database
- **Database**: PostgreSQL (Neon serverless)
- **ORM**: Drizzle ORM with drizzle-kit for migrations
- **Schema Location**: `shared/schema.ts`
- **Key Tables**: users (with Stripe integration), sessions, badges, playerStats

### Game Structure
The game consists of multiple parts:
- **Part 1A**: Welcome & Character Selection
- **Part 1B**: Grammar Adventures (12 themed adventures with difficulty levels)
- **Part 2A**: Sentence Building exercises
- **Part 2B**: Punctuation Challenges
- **Part 3**: Grammar Ghost Chase (arcade-style game)

Game data and questions are embedded directly in component files with difficulty-based organization (Explorer/Adventurer/Champion/Master levels for grades 3-9+).

### Audio System
Sound effects stored in `/public/` folder:
- Correct answer: rimshot sound (`rimshot-joke-funny-80325.mp3`)
- Wrong answer: robot click sound (`little_robot_sound_factory_multimedia_Click_Electronic_16.mp3`)
- Background music: Mr Clown loop (`mr_clown.mp3`)

### Footer Components
- **FooterContent.tsx**: New section component above footer with all content (logo, links, contact info) with yellow (#FBBF24) background and dark brown text (#1F2937) for visibility
- **Footer.tsx**: Minimal footer with just copyright text and dark background
- Used in Home.tsx with FooterContent appearing before minimal Footer

## External Dependencies

### Payment Processing
- **Stripe**: Subscription management with hosted checkout (LIVE MODE ACTIVE)
- **stripe-replit-sync**: Replit's Stripe integration helper for webhooks and data sync
- **Pricing**: Monthly subscription model ($6.99/month)
- **Live Product ID**: `prod_Tav2EvHiegatB9`
- **Live Price ID**: `price_1SdjC0B865KSFqIBF7GhcYjC`
- **Payment Link**: `https://buy.stripe.com/4gM28rbPX8I7dZKbvGgUM00`
- **Stripe Account**: Production/Live (synced to PostgreSQL stripe schema)

### Authentication
- **Google OAuth 2.0**: OpenID authentication via Google
- **Passport.js**: Used for OAuth strategy management
- **Required Secrets**: 
  - `GOOGLE_CLIENT_ID`: OAuth app client ID
  - `GOOGLE_CLIENT_SECRET`: OAuth app client secret
  - `SESSION_SECRET`: Express session encryption key

### Database
- **Neon Database**: Serverless PostgreSQL
- **Required**: `DATABASE_URL` environment variable

### Third-Party Services
- **Google Fonts**: Fredoka, Quicksand, Creepster font families
- **Lucide React**: Icon library

### Key Environment Variables
- `DATABASE_URL`: PostgreSQL connection string (required)
- `SESSION_SECRET`: Express session encryption key (required)
- `REPLIT_DOMAINS`: For webhook URL configuration
- `ISSUER_URL`: Replit OIDC issuer (defaults to https://replit.com/oidc)

## Recent Changes (Dec 19, 2025 - GOOGLE OAUTH INTEGRATION)

### AUTHENTICATION SYSTEM REPLACED WITH GOOGLE OAUTH

**Changes Made:**
1. ✅ Replaced Replit OpenID Connect with Google OAuth 2.0
2. ✅ Created new `server/googleAuth.ts` with passport-google-oauth20 strategy
3. ✅ Updated `server/index.ts` to use googleAuth instead of replitAuth
4. ✅ Google OAuth credentials stored securely as environment secrets:
   - `GOOGLE_CLIENT_ID`: `774943168416-eftc27muf3b9u54aa6kdu5viuj4r8ob9.apps.googleusercontent.com`
   - `GOOGLE_CLIENT_SECRET`: Stored securely in Replit secrets
5. ✅ Auth callback redirects properly handle post-login checkout flow
6. ✅ Login scopes: `profile`, `email` (simplified from OIDC)
7. ✅ Session management and user upsert logic preserved

### Full Payment Flow:
```
User clicks "Purchase Now"
        ↓
   If not logged in → Redirect to login
        ↓
   POST /api/checkout creates Stripe checkout session
        ↓
   User redirected to Stripe checkout page
        ↓
   User completes payment on Stripe
        ↓
   Stripe redirects to /success?session_id={CHECKOUT_SESSION_ID}
        ↓
   SuccessPage calls /api/payment-success to sync subscription
        ↓
   Auth data refreshed, subscription status updated
        ↓
   Auto-redirects to /dashboard (5 second countdown)
        ↓
   Onboarding shown if not completed
        ↓
   User can start playing games at /play
```

### Key Files Created/Modified:
- `server/googleAuth.ts` - NEW: Google OAuth authentication with Passport.js
- `server/index.ts` - Updated import to use googleAuth instead of replitAuth
- `client/src/components/Navigation.tsx` - Redirect to `/api/login?redirect=checkout` for authenticated checkout
- Auth endpoints preserved: `/api/login`, `/api/callback`, `/api/logout`
- User upsert logic maps Google profile to app user schema

## Complete Sitemap

| Route | Status | Description |
|-------|--------|-------------|
| `/` | ✅ | Home page with purchase buttons |
| `/about` | ✅ | About page |
| `/contact` | ✅ | Contact page |
| `/success?session_id=...` | ✅ FIXED | Payment success redirect |
| `/dashboard` | ✅ | Main user dashboard |
| `/play` | ✅ | Game parts menu |
| `/play/part1a` | ✅ | Grammar Adventures |
| `/play/part1b` | ✅ | Advanced Grammar |
| `/play/part2a` | ✅ | Sentence Building |
| `/play/part2b` | ✅ | Punctuation |
| `/play/part3` | ✅ | Grammar Ghost Chase |
| `/minigames` | ✅ | Practice games |
| `/policies/*` | ✅ | Legal documents |

## Current Status: READY FOR TESTING & PUBLISHING

✅ Stripe integration fixed - now using proper credential system  
✅ Payment flow complete and routed correctly  
✅ Success page properly accessible  
✅ Dashboard redirects working  
✅ All routes functional  
✅ App ready to test full payment flow and publish
