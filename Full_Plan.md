Functional Requirements Document: Orbit Style
1. Project Overview
Product Name: Orbit Style

Description: A mobile-first, Progressive Web Application (PWA) that allows users to perform virtual clothing try-ons. Users upload photos of themselves and clothing items, utilizing Google's Gemini GenAI API to merge them realistically.
Architecture Pattern: Cloud-Enabled (Supabase Backend). Uses Supabase for Authentication, Database (PostgreSQL), and Object Storage. Connects to Gemini API for AI processing.
2. User Roles
End User: The primary user who uploads photos, manages their wardrobe, and generates try-on images.
System: The automated processes (AI generation, local database management, cost calculation).
3. System Architecture & Tech Stack
Frontend Framework: React 19+ (Vite recommended).
Styling: Tailwind CSS (Theme: Space/Dark Mode, Glassmorphism).
Backend & Database: Supabase (PostgreSQL) for structured data.
Authentication: Supabase Auth.
Storage: Supabase Storage (Buckets) for images.
AI Engine: Google Gemini API (@google/genai SDK).
Image Generation: gemini-2.5-flash-image
Image Analysis/Description: gemini-2.5-flash


Hosting: Static hosting (Vercel/Netlify/GitHub Pages).

4. Functional Modules
4.1. Onboarding & Authentication
Goal: Secure user identity and data synchronization.
FR 1.1 - User Authentication:
Implement Supabase Auth for user signup/login.
Support Email/Password and potentially Social Providers (Google).
Secure session management.

FR 1.2 - Profile Setup:
Create user profile record in Supabase upon registration.
Allow user to input Gemini API Key (stored securely in user settings/profile if required, or managed via env vars if centralized).


4.2. User Profile (Base Models)
Goal: Manage the "body" images onto which clothes will be overlaid.
FR 2.1 - Upload Base Photo:
Users can upload an image via the device camera or file picker.
Requirement: Images must be resized client-side (max-width: 800px) before storage to prevent database bloat.


FR 2.2 - Categorization:
Users must assign a category to the photo (e.g., "Casual", "Formal") based on the pose or intended use.


FR 2.3 - Storage:
FR 2.3 - Storage:
Photos are uploaded to Supabase Storage buckets.
Metadata (paths, category) stored in Supabase Database.


4.3. Wardrobe Management
Goal: A digital closet for users to store clothing items they want to try on.
FR 3.1 - Add Item:
Upload clothing image via camera/file.


FR 3.2 - AI Analysis (Auto-Captioning):
When an image is uploaded, the system must send it to gemini-2.5-flash to generate a short visual description (e.g., "Red velvet bomber jacket with gold zipper").
User can edit this description manually.


FR 3.3 - Favorites:
Users can mark items as "Favorites".
Favorites appear in the "Quick Pick" section on the Home screen.


FR 3.4 - CRUD Operations:
View grid of items.
Delete items.


4.4. The Try-On Engine (Core Feature)
Goal: Generate the synthetic image.
FR 4.1 - Selection Flow:
Select Base User Photo.


Select Clothing Item (from Wardrobe OR direct upload).




FR 4.2 - Prompt Engineering:
The system must maintain a registry of prompts based on clothing categories (e.g., Logic for "T-Shirt" differs from "Dress").
Example Prompt Logic: "Image 1 is the user. Image 2 is [Clothing]. Replace user's clothing. Maintain pose/lighting."


FR 4.3 - Generation Execution:
Call ai.models.generateContent using the gemini-2.5-flash-image model.
Pass both images (Base64) and the constructed prompt.


FR 4.4 - Cost Analysis:
The system must calculate the estimated cost of the generation based on input/output tokens.
Display cost in a localized currency (e.g., INR or USD) based on a defined exchange rate.


FR 4.5 - Output Display:
Show the generated image.
Allow side-by-side comparison (implied by context).
Download button to save to device.


4.5. History & Gallery
Goal: Review past generations.
FR 5.1 - Archives:
FR 5.1 - Archives:
Automatically save every successful generation to Supabase Database and Storage.


FR 5.2 - Feedback System:
Users can mark a result as "Thumbs Up" (Positive) or "Thumbs Down" (Negative).


FR 5.3 - Filtering:
Filter gallery by All, Positive, or Negative.


FR 5.4 - Metadata:
Display token usage and cost for historical items.


4.6. Settings & Configuration
Goal: User control over the application environment.
FR 6.1 - Category Management:
Users can Create, Edit, and Delete clothing categories.
Users can edit the specific System Prompt associated with a category.


FR 6.2 - Data Management:
FR 6.2 - Data Management:
"Delete Account": Option to delete user account and all associated data from Supabase.


FR 6.3 - PWA Installation:
If the browser supports it, show a prompt to install the app to the home screen.



5. Non-Functional Requirements (NFRs)
NFR 1 - Performance:
Image resizing must happen on a Web Worker or main thread efficiently to ensure the UI doesn't freeze during upload.
App load time should be under 2 seconds (using Service Workers).


NFR 2 - Privacy:
NFR 2 - Privacy & Security:
User data is stored securely in Supabase with Row Level Security (RLS) policies.
Images are stored in private buckets accessible only to the authenticated user.


NFR 3 - Offline Capability:
The app shell (UI) must load offline via Service Worker.
Wardrobe and User photos should be viewable offline.
Note: Generation requires internet connection.


NFR 4 - Aesthetics:
Theme: "Space/Cosmic". Dark backgrounds (#050507), neon violet accents (#8b5cf6), glassmorphism cards.
Animations: Twinkling stars background, floating loading states, smooth transitions.



6. UI/UX Specifications
6.1 Navigation (Bottom Bar)
Try On (Home): Main action area. Quick pick wardrobe + Body selector.
Wardrobe: Grid view of clothing items. Upload new items.
History: Grid view of past results.
Settings: Configuration and keys.
6.2 Loading States
Visual: A customized "Warping Spacetime" loader (spinner with orbital animations) must be displayed during the 5-15 second API call.
Feedback: A progress bar with changing text labels (e.g., "Analyzing Texture" -> "Generating Fit" -> "Finalizing").
6.3 Receipt View
A collapsible "Receipt" card showing the breakdown of Input Tokens + Output Image cost = Total Cost.

7. Data Models (Supabase Schema)
Tables required in PostgreSQL:
- profiles: { id (uuid), email, created_at }
- user_photos: { id, user_id, storage_path, category_id, created_at }
- wardrobe_items: { id, user_id, storage_path, description, category_id, is_favorite, created_at }
- categories: { id, label, prompt, user_id (optional for custom) }
- try_on_results: { id, user_id, original_photo_id, clothing_item_id, result_storage_path, timestamp, cost_analysis, feedback }
- settings: { user_id, api_key (encrypted), theme_preference }

