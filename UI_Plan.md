# UI Plan: Orbit Style

## 1. Design System & Aesthetics
**Theme:** "Space/Cosmic"
- **Backgrounds:** Dark (#050507), Twinkling stars background.
- **Accents:** Neon violet (#8b5cf6).
- **Cards:** Glassmorphism effects.
- **Typography:** Modern, clean sans-serif (e.g., Inter or Orbitron for headers).

**Animations:** [[Ref: Logic_Plan.md#NFR-4]]
- Floating loading states.
- Smooth page transitions.

## 2. Frontend Architecture
- **Framework:** React 19+ (Vite).
- **Styling:** Tailwind CSS.

## 3. Navigation Structure (Bottom Bar)
The app uses a persistent bottom navigation bar with the following tabs:

1.  **Try On (Home):** Main action area.
2.  **Wardrobe:** Manage clothing items.
3.  **History:** View past generations.
4.  **Settings:** App configuration.

## 4. Screen Specifications

### 4.1. Home (Try On) [[Ref: Logic_Plan.md#FR-4.1]]
- **Layout:** Split view or step-by-step wizard.
- **Components:**
    - **Body Selector:** Carousel or grid of user base photos.
    - **Quick Pick:** Horizontal scroll of "Favorite" wardrobe items. [[Ref: Logic_Plan.md#FR-3.3]]
    - **Generate Button:** Prominent CTA.

### 4.2. Wardrobe [[Ref: Logic_Plan.md#FR-3.4]]
- **Layout:** Grid view of clothing items.
- **Actions:**
    - Floating Action Button (FAB) to upload new items.
    - Long-press to delete or edit.
    - Tap to view details/edit description.

### 4.3. History [[Ref: Logic_Plan.md#FR-5.3]]
- **Layout:** Masonry or Grid view of generated images.
- **Filters:** Tabs for "All", "Positive", "Negative".
- **Item View:** Full-screen modal with "Receipt" and "Download" options.

### 4.4. Settings [[Ref: Logic_Plan.md#FR-6.1]]
- **Layout:** List view of options.
- **Sections:**
    - API Key Management.
    - Category Editor.
    - Data Management (Delete Account).

## 5. UI Components

### 5.1. Loading States [[Ref: Logic_Plan.md#FR-6.2]]
- **Visual:** "Warping Spacetime" loader (spinner with orbital animations).
- **Feedback:** Progress bar with changing text labels (e.g., "Analyzing Texture" -> "Generating Fit").

### 5.2. Receipt View [[Ref: Logic_Plan.md#FR-4.4]]
- **Component:** Collapsible card.
- **Content:** Breakdown of Input Tokens + Output Image cost = Total Cost.
