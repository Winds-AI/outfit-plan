# Database Plan: Orbit Style

## 1. Data Models (Supabase Schema)

### 1.1 Tables
The following tables are required in PostgreSQL.

**profiles**
- `id` (uuid, PK, references auth.users)
- `email` (text)
- `created_at` (timestamp)

**user_photos** [[Ref: Logic_Plan.md#FR-2.3]]
- `id` (uuid, PK)
- `user_id` (uuid, FK profiles.id)
- `storage_path` (text)
- `category_id` (uuid, FK categories.id)
- `created_at` (timestamp)

**wardrobe_items** [[Ref: Logic_Plan.md#FR-3.4]]
- `id` (uuid, PK)
- `user_id` (uuid, FK profiles.id)
- `storage_path` (text)
- `description` (text)
- `category_id` (uuid, FK categories.id)
- `is_favorite` (boolean)
- `created_at` (timestamp)

**categories** [[Ref: Logic_Plan.md#FR-6.1]]
- `id` (uuid, PK)
- `label` (text)
- `prompt` (text)
- `user_id` (uuid, optional for custom categories)

**try_on_results** [[Ref: Logic_Plan.md#FR-5.1]]
- `id` (uuid, PK)
- `user_id` (uuid, FK profiles.id)
- `original_photo_id` (uuid, FK user_photos.id)
- `clothing_item_id` (uuid, FK wardrobe_items.id)
- `result_storage_path` (text)
- `timestamp` (timestamp)
- `cost_analysis` (jsonb)
- `feedback` (text/enum: 'positive', 'negative')

**settings** [[Ref: Logic_Plan.md#FR-6.2]]
- `user_id` (uuid, PK, references profiles.id)
- `api_key` (text, encrypted)
- `theme_preference` (text)

## 2. Storage (Buckets)
Supabase Storage buckets required for the application.

**buckets**
- `user_uploads`: Private bucket for user base photos. [[Ref: Logic_Plan.md#FR-2.3]]
- `wardrobe_items`: Private bucket for clothing items.
- `generated_results`: Private bucket for AI generated images. [[Ref: Logic_Plan.md#FR-5.1]]

## 3. Security Policies (RLS)
- **General Rule:** Users can only select, insert, update, and delete rows where `user_id` matches their authenticated ID.
- **Storage Rule:** Users can only access objects in buckets that belong to their folder/path (e.g., `/{user_id}/*`).
