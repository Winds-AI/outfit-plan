# Logic Plan: Orbit Style

## 1. Project Overview
**Product Name:** Orbit Style

**Description:** A mobile-first, Progressive Web Application (PWA) that allows users to perform virtual clothing try-ons. Users upload photos of themselves and clothing items, utilizing Google's Gemini GenAI API to merge them realistically.

**Architecture Pattern:** Cloud-Enabled (Supabase Backend). Uses Supabase for Authentication, Database (PostgreSQL), and Object Storage. Connects to Gemini API for AI processing.

## 2. User Roles
**End User:** The primary user who uploads photos, manages their wardrobe, and generates try-on images.
**System:** The automated processes (AI generation, local database management, cost calculation).

## 3. System Architecture & Tech Stack (Logic Focus)
- **Backend & Database:** Supabase (PostgreSQL) for structured data.
- **Authentication:** Supabase Auth.
- **AI Engine:** Google Gemini API (@google/genai SDK).
    - **Image Generation:** gemini-2.5-flash-image
    - **Image Analysis/Description:** gemini-2.5-flash

## 4. Functional Modules

### 4.1. Onboarding & Authentication
**Goal:** Secure user identity and data synchronization.

**FR 1.1 - User Authentication:**
- Implement Supabase Auth for user signup/login.
- Support Email/Password and potentially Social Providers (Google).
- Secure session management.

**FR 1.2 - Profile Setup:**
- Create user profile record. [[Ref: Database_Plan.md#profiles]]
- Allow user to input Gemini API Key.

### 4.2. User Profile (Base Models)
**Goal:** Manage the "body" images onto which clothes will be overlaid.

**FR 2.1 - Upload Base Photo:**
- Users can upload an image via the device camera or file picker.
- **Requirement:** Images must be resized client-side (max-width: 800px) before storage.

**FR 2.2 - Categorization:**
- Users must assign a category to the photo (e.g., "Casual", "Formal").

**FR 2.3 - Storage Logic:**
- Photos are uploaded to storage buckets. [[Ref: Database_Plan.md#storage-buckets]]
- Metadata stored in database. [[Ref: Database_Plan.md#user_photos]]

### 4.3. Wardrobe Management
**Goal:** A digital closet for users to store clothing items they want to try on.

**FR 3.1 - Add Item:**
- Upload clothing image via camera/file.

**FR 3.2 - AI Analysis (Auto-Captioning):**
- When an image is uploaded, the system must send it to `gemini-2.5-flash` to generate a short visual description.
- User can edit this description manually.

**FR 3.3 - Favorites:**
- Users can mark items as "Favorites".

**FR 3.4 - CRUD Operations:**
- View grid of items. [[Ref: UI_Plan.md#wardrobe-view]]
- Delete items.

### 4.4. The Try-On Engine (Core Feature)
**Goal:** Generate the synthetic image.

**FR 4.1 - Selection Flow:**
- Select Base User Photo.
- Select Clothing Item.

**FR 4.2 - Prompt Engineering:**
- The system must maintain a registry of prompts based on clothing categories.
- **Example Prompt Logic:** "Image 1 is the user. Image 2 is [Clothing]. Replace user's clothing. Maintain pose/lighting."

**FR 4.3 - Generation Execution:**
- Call `ai.models.generateContent` using the `gemini-2.5-flash-image` model.
- Pass both images (Base64) and the constructed prompt.

**FR 4.4 - Cost Analysis:**
- Calculate estimated cost based on input/output tokens.

**FR 4.5 - Output Handling:**
- Receive generated image.
- Allow download.

### 4.5. History & Gallery
**Goal:** Review past generations.

**FR 5.1 - Archives:**
- Automatically save every successful generation. [[Ref: Database_Plan.md#try_on_results]]

**FR 5.2 - Feedback System:**
- Users can mark a result as "Thumbs Up" or "Thumbs Down".

**FR 5.3 - Filtering:**
- Filter gallery by All, Positive, or Negative.

### 4.6. Settings & Configuration
**Goal:** User control over the application environment.

**FR 6.1 - Category Management:**
- Users can Create, Edit, and Delete clothing categories.
- Users can edit the specific System Prompt associated with a category.

**FR 6.2 - Data Management:**
- "Delete Account": Option to delete user account and all associated data.

## 5. Non-Functional Requirements (NFRs)

**NFR 1 - Performance:**
- Image resizing on Web Worker.
- App load time under 2 seconds.

**NFR 2 - Privacy & Security:**
- User data stored securely with RLS policies.
- Private storage buckets.

**NFR 3 - Offline Capability:**
- App shell loads offline.
- Wardrobe/User photos viewable offline.
- Generation requires internet.
