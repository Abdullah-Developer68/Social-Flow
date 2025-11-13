# Social Flow - Product Requirements


## Feature Summary: Content Creator Hub

This is the main client application for creators, focusing on content management and AI assistance.

| Feature Area | Description | React Implementation | Supabase/OpenAI Implementation |
| :--- | :--- | :--- | :--- |
| **1. Universal Posting** | **One-stop scheduling and publishing** for various social media platforms (e.g., YouTube, X/Twitter, Instagram/Facebook). | **Component:** A unified `PostEditor` component with character/media counters tailored to the selected platform. **Library:** Use an external library for date/time picking and scheduling logic. | **Supabase Edge Function (Webhook):** An Edge Function securely stores and handles the scheduled posts. When the time comes, it calls the respective social media APIs. **Database:** `scheduled_posts` table for content, status, and publish time. |
| **2. AI Content Suggestions** | Generate captions, titles, hashtags, or blog post outlines based on a topic and tone selected by the user. | **Component:** A dedicated **"AI Assistant Panel"** that sends input text/keywords and displays the returned suggestions in an editable field. | **OpenAI Integration (Edge Function):** An Edge Function receives the user's prompt and desired tone/length, calls the OpenAI Chat Completion API (GPT-4), and returns the generated text. |
| **3. AI Image Generation** | Generate custom images (e.g., YouTube thumbnails, social post images) based on a text prompt. | **Component:** An `ImageGenerator` component with a large text input and a gallery view to display generated images. **UI:** Add loading/spinner states while the generation is running. | **OpenAI DALL-E (Edge Function/Storage):** The Edge Function calls the DALL-E API. The resulting image URL is fetched, saved securely to **Supabase Storage**, and the public URL is returned to the React client. |
| **4. Performance Stats** | View key metrics (views, engagement, revenue) for published videos/posts across platforms. | **Component:** A dedicated `Dashboard` and `Charts` component using a library like **Recharts** or **D3.js** to visualize time-series data. | **Supabase Database/Edge Function:** An Edge Function is scheduled (via cron or daily job) to pull data from social media analytics APIs (YouTube, etc.). The raw data is cleaned and stored in a `performance_data` table. |

---

## Feature Summary: Ad Agency Model

This is the dedicated platform for **Business Owners** to find and manage creators.

| Feature Area | Description | React Implementation | Supabase/Postgres Implementation |
| :--- | :--- | :--- | :--- |
| **1. Creator Discovery** | Ability for business owners to search, filter, and view creator profiles based on niche, product experience, and performance stats. | **Component:** A detailed `CreatorSearch` interface with multiple filtering options (niche tags, follower count slider). **State:** Efficient data fetching with pagination from Supabase. | **Supabase Database (Advanced Search):** Creators are tagged with niche categories. **Vector Embeddings (Optional but advanced):** Store vector embeddings of creator bios/past videos in Postgres (`pg_vector`) for semantic search (e.g., searching for "sustainability products" finds creators who used terms like "eco-friendly"). |
| **2. Creator Profiles** | Detailed, public-facing profiles showcasing a creator's portfolio, rate card, and stats (pulled from the Creator Hub's database). | **Component:** A read-only `CreatorProfile` component structured for clarity (rates, portfolio, contact button). | **Supabase RLS (Row-Level Security):** Crucial to ensure business owners **only see public** creator data, and cannot access private content or other business owners' data. |
| **3. Hiring/Project Management** | A system for initiating a contract, managing deliverables, and approving content. | **Component:** A `ProjectTracker` with stages (Draft, Review, Approved). Components for file uploads and internal chat/comments. | **Supabase Realtime:** Use Realtime subscriptions on the `projects` or `messages` table to provide instant updates on content status changes or new messages between the Business Owner and Creator. **Supabase Storage:** Handles secure contract and content deliverable file storage. |

---

## Feature Summary: Internal Tool Admin Dashboard

This is a secure, internal-use-only dashboard for managing the core system data.

| Feature Area | Description | React Implementation | Supabase/Auth Implementation |
| :--- | :--- | :--- | :--- |
| **1. User & Role Management** | View all users (Creators, Business Owners, Admins) and modify their roles or activate/deactivate accounts. | **Component:** A central `UsersTable` with sort, filter, and inline editing capabilities. **Conditional Rendering:** Ensures only **Admin** users see this entire dashboard. | **Supabase Auth & RLS:** The RLS policies must be structured to check if the authenticated user has the "Admin" role before allowing any database modifications. |
| **2. Creator Database Management** | CRUD (Create, Read, Update, Delete) operations on the core `creators` table, including manual niche tagging and rate adjustments. | **Component:** A highly functional `CreatorManager` with large forms for editing complex data fields. | **Supabase Studio (Alternative/Starter):** Initially, you could use the built-in Supabase Studio for basic data management. However, building a custom React tool with **Postgres Functions** to validate data on complex updates is the superior, customized solution. |
| **3. Video/Content Audit** | Review all content entries (AI-generated, scheduled, or published) for compliance, quality checks, and moderation. | **Component:** A paginated `ContentAuditLog` showing all items, with filter options for status (e.g., "Pending Review," "AI Generated"). | **Supabase Database/Postgres Functions:** Use database functions for mass operations (e.g., a "Deactivate Creator's Content" function). Use **Storage** for previewing uploaded content files. |
