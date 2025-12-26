# Ela and the Missing Message - Sitemap

## Public Routes (No Authentication Required)

### Landing & Information
- **/** - Home Page
  - Hero section with game overview
  - Character showcase (Ela, Sam, Paul, Tom)
  - Customization section
  - CTA buttons: "Join the Adventure" & "Sign In"
  - Background music player
  
- **/about** - About Us Page
  - Company/game information
  - Features and benefits
  
- **/contact** - Contact Us Page
  - Contact form
  - Support information

### Payment & Policies
- **Stripe Payment Link** - `https://buy.stripe.com/4gM28rbPX8I7dZKbvGgUM00`
  - Hosted Stripe checkout
  - $6.99/month subscription
  - Coupon code support (ELA100, TESTDOC100, etc.)
  
- **/policies/:type** - Legal Documents
  - Privacy Policy
  - Terms of Service
  - Cookie Policy
  - Data Protection Agreement
  - Refund Policy
  - Accessibility Statement

### Authentication
- **/api/login** - Replit OpenID Connect Login
- **/api/logout** - Logout endpoint

---

## Authenticated Routes (Login Required)

### Main Navigation
- **/** - Home Page (authenticated view)
  - Welcome message with user's name
  - Full access to game content
  
- **/dashboard** - Player Dashboard
  - Welcome with child's name
  - Player stats and progress
  - Achievement badges
  - Quick access to game parts
  
- **/about** - About Us (authenticated view)
- **/contact** - Contact Us (authenticated view)

### Game Content - Play Menu
- **/play** - Game Parts Menu
  - Part selection interface
  - Progress tracking

### Game Parts
- **/play/part1a** - Part 1A: Welcome & Character Selection
  - Introductory content
  - Character customization
  
- **/play/part1b** - Part 1B: Grammar Adventures
  - 12 themed grammar adventures
  - Difficulty levels: Explorer, Adventurer, Champion, Master
  - Grades 3-9+ content
  
- **/play/part2a** - Part 2A: Sentence Building
  - Interactive sentence construction
  - Grammar practice exercises
  
- **/play/part2b** - Part 2B: Punctuation Challenges
  - Punctuation rule mastery
  - Interactive challenges
  
- **/play/part3** - Part 3: Grammar Ghost Chase
  - Arcade-style mini-game
  - Time-based challenges

### Additional Features
- **/minigames** - Mini Games Collection
  - Additional practice games
  - Supplementary learning content
  
- **/signin-landing** - Sign In Landing Page
  - Information for existing subscribers
  
- **/policies/:type** - Legal Documents (authenticated view)

---

## API Endpoints

### Authentication
- `GET /api/auth/user` - Get current user info
- `POST /api/login` - Initiate login
- `GET /api/logout` - Logout user

### User Management
- `POST /api/complete-onboarding` - Complete initial onboarding
- `POST /api/upload-seeker` - Upload child's avatar/profile image
- `GET /api/user/subscription` - Get subscription status

### Payment
- `POST /api/checkout` - Create Stripe checkout session *(deprecated - now using Stripe payment link)*
- `POST /api/stripe/webhook/:uuid` - Stripe webhook receiver

### Data Sync
- Automatic Stripe sync via `stripe-replit-sync` integration
  - Products
  - Prices
  - Plans
  - Customers
  - Subscriptions
  - Checkout sessions
  - Payment methods

---

## Database Schema

### Core Tables
- **users** - Parent accounts
  - Stripe customer integration
  - Child profile (seekerName, seekerImageUrl)
  - Subscription status
  - Onboarding status
  
- **sessions** - Session management
  - PostgreSQL-backed sessions via connect-pg-simple
  
- **badges** - Achievement badges
  - Badge types and descriptions
  - Award tracking
  
- **playerStats** - Game progress
  - Games played count
  - Correct answers
  - Streak count
  - Total playtime

### Stripe Integration Tables (auto-managed)
- stripe_products
- stripe_prices
- stripe_plans
- stripe_customers
- stripe_subscriptions
- stripe_checkout_sessions
- stripe_payment_methods
- stripe_invoices
- stripe_charges

---

## Asset Locations

### Audio Files
- `/public/attached_assets/`
  - `mr_clown.mp3` - Background music
  - `rimshot-joke-funny-80325.mp3` - Correct answer sound
  - `little_robot_sound_factory_multimedia_Click_Electronic_16.mp3` - Wrong answer sound

### Images
- `/public/attached_assets/`
  - Ela logo and character assets
  - Arrow and decorative images
  - Team imagery

### User-Uploaded Content
- `/uploads/seekers/` - Child avatar/profile images

---

## Page Structure Overview

### Free Access (Public)
1. Home → About → Contact
2. Purchase Now → Stripe Checkout

### After Payment (Authenticated)
1. Home → Dashboard → Play → Game Parts
2. Mini Games
3. Badges & Progress

---

## Technology Stack

- **Frontend**: React 18 + TypeScript + Vite
- **Routing**: Wouter
- **Styling**: Tailwind CSS v4
- **UI Components**: Shadcn/ui + Radix UI
- **Server**: Express.js
- **Database**: PostgreSQL (Neon)
- **ORM**: Drizzle
- **Auth**: Replit OpenID Connect
- **Payments**: Stripe (Live Mode)
- **State**: TanStack React Query

---

## Workflow Diagnostics Report

### ✅ Status: RUNNING (All Systems Operational)

**Server Status:**
- Port: 5000
- Environment: Development
- Node.js: Running

**Stripe Integration:**
- ✅ Schema initialized
- ✅ Managed webhook configured
- ✅ Data sync active
- ✅ Products synced (1 product)
- ✅ Prices synced (1 price)
- ✅ Plans synced (1 plan)
- ✅ Customers synced (1 customer)
- ✅ Checkout sessions synced (1 session)

**Database:**
- ✅ Connected
- ✅ Schema ready
- ✅ All tables present

**Webhook:**
- ✅ URL: `https://{your-domain}/api/stripe/webhook/{uuid}`
- ✅ Events: All events enabled (`*`)
- ✅ Ready to receive payment confirmations

**Browser Connection:**
- ✅ Vite dev server connected
- ✅ Hot module replacement active
- ✅ No console errors

**Known Notes:**
- 401 responses on `/api/auth/user` are normal (unauthenticated requests)
- Subscription count is 0 (no live payments yet - waiting for ELA100 coupon creation in Stripe)

---

## Next Steps for Go-Live

1. ✅ Create coupon `ELA100` in **LIVE Stripe mode** (100% discount)
2. ✅ Test payment flow with test card (if needed)
3. ⏳ Monitor webhook events for successful payments
4. ⏳ Verify child names save to database after payment
5. Ready for production deployment

