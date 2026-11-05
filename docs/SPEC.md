# CookShare — Software Requirements Specification & User Stories

**Course:** Internet Programming I · I4-GIC · Academic Year 2026–2027  
**Student:** KHORN VICTOR (`e20230078`) · Group: I4-GIC-A  
**Domain:** Recipe Sharing Web Application (**CookShare**)  

---

## 👥 User Roles & Permissions Summary

| Feature / Action | Guest (Visitor) | Member (Home Cook) | Admin (Moderator) |
| :--- | :---: | :---: | :---: |
| Browse & Search Published Recipes | ✅ Allowed | ✅ Allowed | ✅ Allowed |
| Filter by Category & Dietary Tags | ✅ Allowed | ✅ Allowed | ✅ Allowed |
| View Recipe Details, Ingredients & Steps | ✅ Allowed | ✅ Allowed | ✅ Allowed |
| View Public Reviews & Ratings | ✅ Allowed | ✅ Allowed | ✅ Allowed |
| Register & Login (JWT) | ✅ Allowed | N/A (Logged In) | N/A (Logged In) |
| Create New Recipe (with nested Ingredients & Steps) | ❌ Forbidden | ✅ Allowed | ✅ Allowed |
| Edit / Delete Own Recipe | ❌ Forbidden | ✅ Allowed | ✅ Allowed |
| Edit / Delete Other User's Recipe | ❌ Forbidden | ❌ Forbidden | ✅ Allowed |
| Bookmark Recipe to Favorites (Add / Remove) | ❌ Forbidden | ✅ Allowed | ✅ Allowed |
| Follow / Unfollow Other Chefs | ❌ Forbidden | ✅ Allowed | ✅ Allowed |
| Submit Rating & Review (1 per recipe) | ❌ Forbidden | ✅ Allowed | ✅ Allowed |
| Delete Any User's Inappropriate Review | ❌ Forbidden | ❌ Forbidden | ✅ Allowed |
| Submit Inappropriate Recipe Report | ❌ Forbidden | ✅ Allowed | ✅ Allowed |
| View Moderation Report Queue | ❌ Forbidden | ❌ Forbidden | ✅ Allowed |
| Hide / Restore Reported Recipe | ❌ Forbidden | ❌ Forbidden | ✅ Allowed |
| Dismiss / Resolve Content Reports | ❌ Forbidden | ❌ Forbidden | ✅ Allowed |
| Create & Manage Master Categories & Tags | ❌ Forbidden | ❌ Forbidden | ✅ Allowed |

---

## 📋 User Stories & Acceptance Criteria

### US-01: Browse & Search Published Recipes

- **As a** visitor (Guest, Member, or Admin),
- **I want to** browse a paginated grid of published recipes and search by keyword in real time,
- **So that** I can easily find dishes matching ingredients in my pantry or cravings.

#### Acceptance Criteria:

1. `GIVEN` the user visits the home page, `THEN` the top recipes are displayed with image, title, category, cooking time, difficulty level, author, and rating.
2. `GIVEN` a keyword is typed into the search bar, `THEN` recipes matching the title, description, or ingredient names are filtered dynamically.
3. `GIVEN` recipes have status `HIDDEN`, `THEN` they are omitted from public search results unless viewed by an Admin.
4. `GIVEN` more than 9 recipes exist, `THEN` pagination controls allow navigation to next and previous pages.

---

### US-02: Filter Recipes by Category & Dietary Tags

- **As a** visitor,
- **I want to** filter recipes by cuisine category (e.g., Khmer Traditional, Soups & Stews, Desserts) and dietary tags (e.g., `#Under 30 Mins`, `#Spicy`),
- **So that** I can narrow my cooking choices according to my available time and diet.

#### Acceptance Criteria:

1. `GIVEN` a user clicks a category pill, `THEN` only recipes belonging to that category are shown.
2. `GIVEN` a user selects a difficulty filter (`EASY`, `MEDIUM`, `HARD`), `THEN` the recipe grid updates accordingly.
3. `GIVEN` the filter yields no results, `THEN` an empty state is displayed with a button to reset all filters.

---

### US-03: View Standardized Recipe Details

- **As a** visitor,
- **I want to** view a structured recipe breakdown with itemized ingredients, units, and ordered step-by-step instructions,
- **So that** I can replicate the dish with consistent flavor and portioning.

#### Acceptance Criteria:

1. `GIVEN` a recipe card is clicked, `THEN` the application navigates to `/recipes/:id`.
2. `GIVEN` the detail page loads, `THEN` it displays the recipe hero image, title, author avatar with follow button, cook time, and difficulty.
3. `GIVEN` the ingredients panel, `THEN` each ingredient lists its exact quantity and unit with interactive prep checkboxes.
4. `GIVEN` the instructions panel, `THEN` steps are strictly ordered by sequential integer position numbers (`1, 2, 3...`).

---

### US-04: Compose and Publish a New Recipe (Standardized Form)

- **As a** registered Member,
- **I want to** compose a recipe with dynamic ingredient rows and numbered steps,
- **So that** I can share my cooking formula with the community.

#### Acceptance Criteria:

1. `GIVEN` an unauthenticated Guest clicks "+ New Recipe", `THEN` they are redirected to login.
2. `GIVEN` a Member opens the Recipe Studio, `THEN` they can enter title, description, category, cook time, and difficulty.
3. `GIVEN` the ingredient builder, `THEN` the user can dynamically add or remove ingredient rows with name, quantity, and unit.
4. `GIVEN` the step builder, `THEN` the user can add multiple instructions with automatic sequential numbering.
5. `GIVEN` the form is submitted without a title, ingredients, or steps, `THEN` validation errors are displayed without sending invalid payload to the backend.
6. `GIVEN` valid data, `THEN` the recipe is saved with status `PUBLISHED` and redirected to its detail view.

---

### US-05: Edit & Delete Own Recipes

- **As a** recipe Author,
- **I want to** update or delete recipes I have previously created,
- **So that** I can refine instructions or remove obsolete entries.

#### Acceptance Criteria:

1. `GIVEN` a Member views a recipe they authored, `THEN` "Edit Recipe" and "Delete Recipe" buttons are visible.
2. `GIVEN` a Member views another user's recipe, `THEN` the edit and delete options are hidden and forbidden on backend endpoints.
3. `GIVEN` a delete action is confirmed, `THEN` the recipe and its associated ingredients/steps are removed.

---

### US-06: Bookmark Favorite Recipes

- **As a** registered Member,
- **I want to** bookmark recipes to my personal Favorites collection,
- **So that** I can quickly access dishes I plan to cook regularly.

#### Acceptance Criteria:

1. `GIVEN` a Member clicks the heart/favorite button on any recipe, `THEN` a record in the `favorites` table is created and the button state toggles to active.
2. `GIVEN` a user views their profile, `THEN` a "Saved Favorites" tab lists all bookmarked recipes.
3. `GIVEN` a Guest clicks the favorite button, `THEN` an authentication prompt modal is triggered.

---

### US-07: Rate and Review Recipes

- **As a** registered Member,
- **I want to** submit a 1–5 star rating and optional textual feedback for a recipe,
- **So that** I can inform other home cooks about the taste and results.

#### Acceptance Criteria:

1. `GIVEN` a Member is viewing a recipe, `THEN` an interactive 5-star rating picker and comment textarea are provided.
2. `GIVEN` a user has already reviewed the recipe, `THEN` submitting a second review updates their existing review (Compound unique constraint: 1 review per user per recipe).
3. `GIVEN` an Admin inspects the reviews, `THEN` they have a dedicated button to delete inappropriate or abusive comments.

---

### US-08: Content Moderation & Recipe Flagging Pipeline

- **As a** Member or Administrator,
- **I want to** flag hazardous, unsafe, or inappropriate submissions, and have moderators review them,
- **So that** community safety and high food preparation standards are preserved.

#### Acceptance Criteria:

1. `GIVEN` any Member spots problematic content, `THEN` clicking "Report Recipe" opens a modal requiring a violation category and explanation.
2. `GIVEN` a report is submitted, `THEN` a record in `recipe_reports` is created with status `PENDING`.
3. `GIVEN` an Administrator accesses `/admin/moderation`, `THEN` the report queue lists all pending flags with reporter info and violation rationale.
4. `GIVEN` an Admin clicks "Hide Recipe", `THEN` the recipe's status changes to `HIDDEN`, immediately removing it from public search.
5. `GIVEN` an Admin clicks "Dismiss Report", `THEN` the report status changes to `DISMISSED` without altering the recipe.

---

### US-09: User Authentication & Role-Based Authorization

- **As a** user,
- **I want to** securely register and log in with email and password,
- **So that** my identity is verified and protected across sessions.

#### Acceptance Criteria:

1. `GIVEN` registration with name, email, and password, `THEN` passwords are encrypted using `bcrypt` before database storage.
2. `GIVEN` valid credentials, `THEN` `POST /auth/login` returns a signed JSON Web Token (JWT) containing the user ID and role (`MEMBER` or `ADMIN`).
3. `GIVEN` protected API endpoints, `THEN` requests without a valid `Bearer <token>` return `401 Unauthorized`.
4. `GIVEN` non-admin users attempting to access `/admin/moderation`, `THEN` the system returns `403 Forbidden`.

---

### US-10: 404 Route Not Found Handling

- **As a** user,
- **I want to** receive a clear, friendly error page when navigating to a non-existent recipe or URL,
- **So that** I understand the page was not found and can navigate back to active content.

#### Acceptance Criteria:

1. `GIVEN` a user enters an unmapped route or requests an unknown recipe ID, `THEN` the application displays the 404 Not Found screen.
2. `GIVEN` the 404 screen is displayed, `THEN` primary call-to-action buttons ("Return to Discover", "Browse Categories") guide the user back to valid pages.
