# Task-Related Screens in Power Tools UI

The main task-related functionality in this repository is centered around the **Product Type Review / Control Flow** feature, which involves reviewing, filtering, and approving products in the context of experiments. Here’s a detailed breakdown of the key screens and their components:

---

## 1. **Control Flow Screen (`/`, `/experiments`, `/experiment/:experimentId`)**

### Main Layout
- **Header:** Standard app header.
- **Sidebar (SideFilters):** Left side panel for filtering and experiment selection.
- **Main Content (ProductControl):** Right/main panel showing products to review and approve.

---

### A. **Sidebar (SideFilters.tsx)**
**Purpose:**  
Allows users to filter products and select experiments to review.

**Components & Interactions:**
- **Experiment Search & Selection:**
    - Search bar to filter experiments by name.
    - Collapsible list of experiments, selectable.
- **Product Type Filters:**
    - Checkbox list for selecting product types.
    - Section is collapsible.
- **Gender Filters:**
    - Checkbox list for selecting gender categories.
    - Section is collapsible.
- **Product Status Filters:**
    - Checkbox list for product statuses (e.g., Approved, Not Approved).
- **Pagination Controls:**
    - Page size selector.
    - Total pages display.

**Loading State:**
- Shows a loader (FilterLoader) while aggregation updates.

---

### B. **Main Content (ProductControl.tsx)**
**Purpose:**  
Displays products for review, allows selecting, approving, and paginating through products.

**Components & Interactions:**
- **Products Grid (ProductsPreview.tsx):**
    - Displays products as cards in a grid.
    - Each card supports:
        - Selection (checkbox or click)
        - Context menu (right-click) for actions on a product.

- **Context Menu:**
    - Triggered by right-clicking a product card.
    - Actions include:
        - Approve selected products.
        - Move selected products to the top of the page.

- **Bulk Actions:**
    - Approve all selected products.

- **Product Approval Modal (ProductApprovalModal):**
    - Modal confirmation for approving products.
    - Used for both single and bulk approval flows, and when moving to the next page with unapproved products.

- **Pagination:**
    - Controls for navigating between pages of products.
    - Disabled if products are selected.

- **Back to Top Button:**
    - Scrolls the product grid to the top.

**Shortcuts:**
- “Select All” keyboard shortcut support.

**Loading State:**
- Backdrop loading spinner shown when fetching or updating products.

---

### C. **Confirmation & Status Modals**
- **Experiment Approval Modal (ApprovalModal):**
    - Pops up when all tasks (product reviews) in an experiment are complete.
    - Prompts user to confirm marking the experiment as “audit complete”.

- **Error/Success Toasts:**
    - Notifications for actions like approval, errors, and saving updates.

---

### D. **Empty State**
- If no subsidiary or experiment is selected, a centered message prompts the user to make a selection.

---

## 2. **Experiment Loader Screen**

- When navigating to `/experiment/:experimentId`, the app loads the relevant experiment and redirects to the main Control Flow view with the experiment context.

---

## 3. **Data Flow and State**
- Backend/API calls are abstracted via service hooks.
- All state (e.g., selected products, filters, current page, experiment readiness) is managed via a centralized store (`useTestControlStore`).

---

## 4. **Technical Stack**

### Core Stack

- **TypeScript**: All code is written in TypeScript, providing strong typing for reliability and maintainability.
- **React**: UI library for building component-based user interfaces.
- **React Router**: Handles routing/navigation between screens (`/`, `/experiments`, `/experiment/:experimentId`).
- **Vite**: Modern frontend build tool for fast development and optimized production builds.
- **TanStack**: Advanced state management and utilities (commonly used for TanStack Query, Virtual, Table, etc.).

### Additional/Bonus Tools

- **Lodash**: Utility library for common JS data operations (e.g., isEmpty, get).
- **React Toastify**: For non-intrusive toast notifications upon user actions and errors.
- **Tailwind CSS**: Utility-first CSS framework for rapid UI styling.
- **Mocking/Testing Tools**
    - [msw (Mock Service Worker)](https://mswjs.io/): For intercepting and mocking network requests during development or tests.
    - [json-server](https://github.com/typicode/json-server): For quick local REST API mocking.
- **Jest/React Testing Library**: Popular choices for component and integration testing.
- **React Hotkeys**: For keyboard shortcut support.
- **React Context**: For global state management (in addition to TanStack/store solution).
- **Prettier & ESLint**: For code linting and formatting.
- **Cypress**: (Bonus) Add end-to-end (E2E) test cases using [Cypress](https://www.cypress.io/). E2E tests should cover critical user flows, such as filtering, selecting, and approving products, and navigation between screens. Including a solid E2E test suite is considered a bonus and will demonstrate attention to quality and real-world readiness.

---

## 5. **Backend Mocking Requirements**

- You must **hardcode the API responses**.
- Use **Node.js** and **Express** for the mock server implementation.
- Your mock endpoints must replicate the full functionality and data structure expected by the frontend (products, experiments, filters, approval actions, etc.).
- No real database integration is required—just return hardcoded/mock data for all endpoints.
- If you use **NestJS** (a progressive Node.js framework), this will be considered a **bonus point** as it demonstrates knowledge of scalable backend architecture and TypeScript-first server development.

---

## 6. **Mocking Guidance**
To fully mock the backend for this flow:
- All experiment, product, and filter data must be mocked.
- Endpoints for product fetching, approving, and status updates must return realistic payloads.
- Support search, filtering, pagination, and status change flows in the mock server.
- Ensure modals and toasts behave as if the backend is present.

---

## 7. **Pull Request and Branching Standards**

**It is mandatory to follow these PR and branch management standards for this task:**

- **Do NOT merge any code directly into the `main` branch.**
- Always work on feature branches.
- Use the following branching model:
    - Create a main parent feature branch:  
      e.g., `feature/backend-mock` or `feature/frontend-task`
    - For each new story, bugfix, or sub-feature, create a story branch on top of the parent feature branch:  
      e.g., `story/filter-sidebar`, `story/product-approval-modal`
    - Raise PRs *from story branch to the parent feature branch*, never to `main`.
    - Merge each story/feature one at a time to the parent feature branch.
    - Use clear and descriptive PR names, referencing the feature or issue you are working on.
    - This approach allows reviewers to see the implementation steps, fixes, and the order in which tasks were completed.

**Example flow:**
1. Create `feature/backend-mock`.
2. Create `story/products-api` from `feature/backend-mock`.
3. Implement the products API mock, then raise a PR from `story/products-api` to `feature/backend-mock`.
4. Repeat for other features or stories.
5. Only after all review cycles and QA merge the `feature/backend-mock` branch into `main` (if authorized).

---

**Summary Workflow:**

```
main
 │
 ├─ feature/parent-feature
 │     ├─ story/feature-1
 │     ├─ story/feature-2
 │     └─ ...
 │
 └─ hotfix/...
```
- Merge only up the tree: story → feature → main

---

## 8. **Kick-start Project**

- **To save your time, we have created a sample project to kick start your work.**  
  You do not need to spend time configuring the environment or setting up boilerplate code—just focus on implementing the required functionality and tasks as described.
- Please use the provided structure and dependencies as your base.

---

## 9. **Additional Notes**

- A **recorded video** is attached to this task—please use it as a reference for expected UI/UX, workflows, and edge cases.
- Make sure your mock implementation matches the flows and behaviors demonstrated in the video.
- If anything in the UI or workflow is unclear, refer to the video for clarification before reaching out.

---

## 10. **Summary**
**Task screens** include:
- A filterable sidebar for selecting experiments and filters.
- A responsive product grid with context menus and bulk actions.
- Pagination and modal dialogs for workflow confirmations.
- Loading and error states for improved UX.

---

**Tip:** See `/src/views/control-flow/` for main task UI code.
