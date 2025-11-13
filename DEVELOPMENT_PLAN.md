# Social Flow - Development Plan

## Project Overview
A comprehensive SaaS platform with three distinct user types: Content Creators, Business Owners, and Admins. Built with React, Vite, Supabase, and OpenAI integration.

---

## Phase 1: Foundation & Setup (Week 1-2)

### 1.1 Project Setup
- [x] Initialize React + Vite project
- [ ] Install core dependencies
- [ ] Configure environment variables
- [ ] Set up project structure with folders for each module

### 1.2 Supabase Configuration
- [ ] Create Supabase project
- [ ] Set up Supabase client configuration
- [ ] Configure authentication providers
- [ ] Set up environment variables for Supabase keys

### 1.3 Database Schema Design
**Tables to Create:**

```sql
-- Users & Auth
- users (managed by Supabase Auth)
- user_profiles (id, user_id, role, full_name, avatar_url, created_at, updated_at)

-- Content Creators
- creators (id, user_id, niche_tags[], portfolio_url, rate_card, bio, stats, created_at)
- scheduled_posts (id, creator_id, platform, content, media_urls[], scheduled_time, status, published_at)
- performance_data (id, creator_id, platform, views, engagement, revenue, date)
- ai_generated_content (id, creator_id, type, prompt, result, created_at)

-- Business Owners & Projects
- projects (id, business_owner_id, creator_id, title, description, status, budget, created_at, updated_at)
- project_deliverables (id, project_id, file_url, status, submitted_at, approved_at)
- messages (id, project_id, sender_id, content, created_at)

-- Admin
- audit_logs (id, admin_id, action, target_table, target_id, details, created_at)
```

### 1.4 Row Level Security (RLS) Policies
- [ ] Enable RLS on all tables
- [ ] Creator access policies (own data only)
- [ ] Business owner access policies (public creator data + own projects)
- [ ] Admin access policies (full access with role check)

### 1.5 Essential Dependencies
```bash
npm install @supabase/supabase-js
npm install react-router-dom
npm install @reduxjs/toolkit react-redux # state management
npm install react-hook-form
npm install lucide-react # icons
npm install tailwindcss postcss autoprefixer
npm install chart.js react-chartjs-2 # for charts/analytics
npm install date-fns # date utilities
npm install react-datepicker # scheduling
```

---

## Phase 2: Authentication & Core Layout (Week 3)

### 2.1 Authentication System
- [ ] Create auth context/provider
- [ ] Login page component
- [ ] Signup page component with role selection
- [ ] Email verification flow
- [ ] Password reset functionality
- [ ] Protected route wrapper
- [ ] Role-based route guards

### 2.2 Core Application Layout
- [ ] Main navigation component
- [ ] Sidebar navigation (role-specific)
- [ ] Header with user menu
- [ ] Footer component
- [ ] Loading states & error boundaries

### 2.3 Routing Structure
```
/
├── /login
├── /signup
├── /reset-password
├── /creator (Content Creator Hub)
│   ├── /dashboard
│   ├── /post-editor
│   ├── /ai-assistant
│   ├── /analytics
│   └── /profile
├── /business (Ad Agency Model)
│   ├── /discover
│   ├── /creator/:id
│   ├── /projects
│   └── /project/:id
└── /admin (Admin Dashboard)
    ├── /users
    ├── /creators
    └── /content-audit
```

---

## Phase 3: Content Creator Hub (Week 4-6)

### 3.1 Universal Posting Feature
**Components:**
- [ ] `PostEditor` - Main editor component
- [ ] `PlatformSelector` - Choose platform (YouTube, X, Instagram, etc.)
- [ ] `CharacterCounter` - Platform-specific limits
- [ ] `MediaUploader` - Image/video uploads to Supabase Storage
- [ ] `SchedulePicker` - Date/time selector
- [ ] `PostPreview` - Platform-specific preview

**Backend:**
- [ ] Create Edge Function: `schedule-post`
- [ ] Create Edge Function: `publish-post` (cron trigger)
- [ ] Set up Supabase Storage bucket for media
- [ ] Integrate social media APIs (YouTube Data API, X API, etc.)

### 3.2 AI Content Suggestions
**Components:**
- [ ] `AIAssistantPanel` - Main AI interface
- [ ] `PromptInput` - Text input with tone/length selectors
- [ ] `SuggestionDisplay` - Editable text area for results
- [ ] `ToneSelector` - Professional, casual, funny, etc.
- [ ] `ContentTypeSelector` - Caption, title, hashtags, blog outline

**Backend:**
- [ ] Create Edge Function: `generate-content`
- [ ] Set up OpenAI API integration
- [ ] Implement prompt engineering for different content types
- [ ] Add OpenAI API key to Supabase secrets

### 3.3 AI Image Generation
**Components:**
- [ ] `ImageGenerator` - Main component
- [ ] `PromptInput` - Detailed text prompt
- [ ] `ImageGallery` - Display generated images
- [ ] `ImageEditor` - Basic editing tools (optional)
- [ ] `LoadingSpinner` - Generation progress

**Backend:**
- [ ] Create Edge Function: `generate-image`
- [ ] Gemini Nano Banana model integration for image generation
- [ ] Save generated images to Supabase Storage
- [ ] Return public URLs to client

### 3.4 Performance Analytics Dashboard
**Components:**
- [ ] `Dashboard` - Main analytics view
- [ ] `MetricsCards` - Views, engagement, revenue summary
- [ ] `ChartsContainer` - Time-series graphs (Chart.js)
- [ ] `PlatformFilter` - Filter by social platform
- [ ] `DateRangePicker` - Custom date ranges

**Backend:**
- [ ] Create Edge Function: `sync-analytics` (scheduled via cron)
- [ ] Integrate platform analytics APIs
- [ ] Data transformation and storage logic
- [ ] Query optimization for large datasets

---

## Phase 4: Ad Agency Model (Week 7-9)

### 4.1 Creator Discovery
**Components:**
- [ ] `CreatorSearch` - Main search interface
- [ ] `FilterPanel` - Niche, follower count, rates
- [ ] `CreatorCard` - Individual creator preview
- [ ] `CreatorList` - Paginated grid/list view
- [ ] `SortOptions` - Sort by relevance, rating, price

**Backend:**
- [ ] Implement full-text search on creator bios
- [ ] Set up filters using Postgres queries
- [ ] (Optional) Implement pg_vector for semantic search
- [ ] Optimize queries with indexes

### 4.2 Creator Profiles
**Components:**
- [ ] `CreatorProfile` - Public profile view
- [ ] `PortfolioSection` - Past work showcase
- [ ] `RateCard` - Pricing information
- [ ] `StatsSection` - Performance metrics
- [ ] `ContactButton` - Initiate project

**Backend:**
- [ ] RLS policies for public/private data separation
- [ ] Join queries for creator + performance data

### 4.3 Project Management
**Components:**
- [ ] `ProjectTracker` - Main project view
- [ ] `ProjectStages` - Draft → Review → Approved
- [ ] `FileUploader` - Deliverable uploads
- [ ] `ChatInterface` - Real-time messaging
- [ ] `ContractManager` - Contract documents

**Backend:**
- [ ] Supabase Realtime subscriptions for messages
- [ ] Storage bucket for contracts/deliverables
- [ ] Notification system (email/in-app)
- [ ] Project status workflow functions

---

## Phase 5: Admin Dashboard (Week 10-11)

### 5.1 User & Role Management
**Components:**
- [ ] `UsersTable` - All users with filters
- [ ] `RoleEditor` - Inline role modification
- [ ] `AccountStatusToggle` - Activate/deactivate users
- [ ] `UserDetails` - Detailed user view

**Backend:**
- [ ] Admin-only RLS policies
- [ ] Postgres functions for bulk operations
- [ ] Audit logging for all admin actions

### 5.2 Creator Database Management
**Components:**
- [ ] `CreatorManager` - CRUD interface
- [ ] `NicheTagEditor` - Manage creator niches
- [ ] `RateCardEditor` - Update pricing
- [ ] `CreatorForm` - Complex data editing

**Backend:**
- [ ] Validation functions for data integrity
- [ ] Postgres triggers for automated updates

### 5.3 Content Audit System
**Components:**
- [ ] `ContentAuditLog` - Paginated content list
- [ ] `ContentPreview` - View uploaded content
- [ ] `ModerationTools` - Approve/reject content
- [ ] `FilterPanel` - Status, type, date filters

**Backend:**
- [ ] Storage integration for content preview
- [ ] Bulk operation functions
- [ ] Automated compliance checks (optional)

---

## Phase 6: Testing & Polish (Week 12-13)

### 6.1 Testing
- [ ] Unit tests for critical functions
- [ ] Integration tests for API calls
- [ ] E2E tests for key user flows
- [ ] Performance testing & optimization
- [ ] Security audit of RLS policies

### 6.2 UI/UX Polish
- [ ] Consistent design system
- [ ] Responsive design for mobile
- [ ] Loading states & skeletons
- [ ] Error handling & user feedback
- [ ] Accessibility improvements (ARIA labels, keyboard navigation)

### 6.3 Documentation
- [ ] API documentation for Edge Functions
- [ ] Component documentation
- [ ] User guides for each role
- [ ] Developer onboarding docs

---

## Phase 7: Deployment & Monitoring (Week 14)

### 7.1 Production Setup
- [ ] Set up production Supabase project
- [ ] Configure production environment variables
- [ ] Set up custom domain
- [ ] SSL certificate configuration
- [ ] CDN setup for static assets

### 7.2 Deployment
- [ ] Deploy React app (Vercel/Netlify recommended)
- [ ] Deploy Supabase Edge Functions
- [ ] Run database migrations in production
- [ ] Test all functionality in production

### 7.3 Monitoring & Analytics
- [ ] Set up error tracking (Sentry)
- [ ] Configure performance monitoring
- [ ] Set up usage analytics
- [ ] Database performance monitoring
- [ ] Set up alerts for critical errors

---

## Technology Stack Summary

### Frontend
- **Framework:** React 19 + Vite
- **Routing:** React Router v7
- **State Management:** Redux Toolkit
- **Styling:** Tailwind CSS
- **Forms:** React Hook Form
- **Charts:** Chart.js
- **Icons:** Lucide React

### Backend
- **Database:** Supabase (PostgreSQL)
- **Authentication:** Supabase Auth
- **Storage:** Supabase Storage
- **Serverless Functions:** Supabase Edge Functions
- **Real-time:** Supabase Realtime
- **AI:** OpenAI API (GPT-4), Google Gemini Nano Banana (Image Generation)

### DevOps
- **Hosting:** Vercel/Netlify
- **Version Control:** Git + GitHub
- **CI/CD:** GitHub Actions
- **Monitoring:** Sentry
- **Testing:** Vitest + React Testing Library

---

## Key Implementation Notes

### Security Best Practices
1. Never expose API keys in client-side code
2. Use Supabase Edge Functions for all OpenAI API calls
3. Implement strict RLS policies for all tables
4. Validate all user inputs on both client and server
5. Use environment variables for all sensitive data

### Performance Optimization
1. Implement pagination for all lists
2. Lazy load routes and heavy components
3. Optimize images and media files
4. Use database indexes for frequently queried fields
5. Implement client-side state management with Redux for efficient data handling

### Code Organization
```
src/
├── components/       # Reusable UI components
├── pages/           # Page-level components
├── hooks/           # Custom React hooks
├── lib/             # Utilities & helpers
├── store/           # Redux store & slices
├── services/        # API services
├── types/           # TypeScript types
└── constants/       # Constants & configs
```

---

## Risk Mitigation

### Technical Risks
- **Social Media API Rate Limits:** Implement queuing system, cache responses
- **OpenAI Costs:** Set usage limits, implement caching for common prompts
- **Supabase Free Tier Limits:** Plan for upgrade, optimize database queries

### Business Risks
- **Data Privacy:** Ensure GDPR compliance, clear data policies
- **Content Moderation:** Implement automated + manual review workflows
- **Scalability:** Design with horizontal scaling in mind

---

## Next Steps

1. Review and approve this development plan
2. Set up Supabase project and get credentials
3. Install all dependencies
4. Begin Phase 1.3 - Database schema creation
5. Start building authentication system

**Estimated Timeline:** 14 weeks for MVP
**Team Size:** 1-2 developers
**Budget Considerations:** OpenAI API costs (GPT-4), Google Gemini Nano Banana API costs, Supabase Pro plan (~$25/month), domain/hosting
