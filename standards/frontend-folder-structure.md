# Forever Hotel Frontend Folder Structure Standard

## Purpose

This document defines the standard frontend folder structure for the Forever Hotel Integrated Hotel Management System.

The objective is to keep all subsystem frontends consistent, maintainable, scalable, and easy for team members to understand.

This standard applies to frontend applications implemented using Next.js.

---

## Core Principles

1. Organize application code by feature/domain rather than by page alone.
2. Keep route files under `app/` thin.
3. Do not place business logic or direct API calls inside route `page.tsx` files.
4. Shared reusable UI belongs under `components/`.
5. Feature-specific code belongs under `features/`.
6. Shared infrastructure belongs under `lib/`.
7. Global application providers belong under `providers/`.
8. Feature modules should expose their public API through `index.ts`.
9. Code outside a feature should not import that feature's internal files directly.
10. Only create folders and modules that are required by the subsystem. Do not add fake implementation files only to make the folder tree complete.

---

## Standard Frontend Structure

```text
frontend/
├── public/
│   ├── icons/
│   └── sw.js
│
├── src/
│   ├── app/
│   │   ├── (auth)/
│   │   ├── (protected)/
│   │   ├── offline/
│   │   │   └── page.tsx
│   │   ├── manifest.ts
│   │   ├── layout.tsx
│   │   ├── page.tsx
│   │   └── globals.css
│   │
│   ├── components/
│   │   ├── ui/
│   │   ├── layout/
│   │   ├── feedback/
│   │   └── forms/
│   │
│   ├── features/
│   │   └── <feature-name>/
│   │       ├── components/
│   │       ├── hooks/
│   │       ├── api/
│   │       ├── schemas/
│   │       ├── types/
│   │       ├── constants/
│   │       ├── utils/
│   │       └── index.ts
│   │
│   ├── lib/
│   │   ├── api/
│   │   ├── auth/
│   │   ├── realtime/
│   │   ├── pwa/
│   │   ├── constants/
│   │   ├── validation/
│   │   └── utils/
│   │
│   ├── hooks/
│   ├── providers/
│   ├── config/
│   ├── types/
│   └── styles/
│
├── tests/
│   ├── fixtures/
│   ├── mocks/
│   └── utils/
│
├── .env.example
├── Dockerfile
├── README.md
├── eslint.config.*
├── jest.config.ts
├── jest.setup.ts
├── next.config.*
├── package.json
└── tsconfig.json
```

Only folders required by the subsystem need to exist. The standard describes where code belongs when that responsibility is implemented.

---

## `app/`

The `app/` directory contains Next.js routes, layouts, route groups, and application-level files.

Route files should remain thin and should mainly compose feature components.

Example:

```tsx
import { TaskQueueScreen } from "@/features/task-queue";

export default function TasksPage() {
  return <TaskQueueScreen />;
}
```

Route files should not contain:

- Business logic
- Large feature components
- Direct API calls
- Feature-specific constants
- Reusable domain logic

Business and feature-specific implementation should be placed inside the appropriate feature module under `src/features/`.

---

## Route Groups

Use Next.js route groups to separate authentication-related routes from protected application routes.

```text
app/
├── (auth)/
│   ├── login/
│   │   └── page.tsx
│   └── change-password/
│       └── page.tsx
│
└── (protected)/
    ├── layout.tsx
    └── <protected-routes>/
```

### `(auth)`

The `(auth)` route group contains authentication-related pages such as:

- Login
- Change password

### `(protected)`

The `(protected)` route group contains application pages that require an authenticated user.

The protected layout may be used for shared authenticated layout behaviour, navigation, and route protection where required.

Route-group folder names do not appear in the browser URL.

For example:

```text
src/app/(auth)/login/page.tsx
```

is accessed as:

```text
/login
```

and:

```text
src/app/(protected)/tasks/page.tsx
```

is accessed as:

```text
/tasks
```

---

## `components/`

The `components/` directory contains shared reusable presentation components.

```text
components/
├── ui/
├── layout/
├── feedback/
└── forms/
```

### `ui/`

Small reusable interface components that are not owned by one specific feature.

### `layout/`

Shared application layout components such as:

- Headers
- Sidebars
- Navigation
- Bottom navigation
- Application shells

### `feedback/`

Reusable user-feedback components such as:

- Loading states
- Empty states
- Error states
- Status messages

### `forms/`

Reusable form-related components.

Feature-specific components must remain inside the relevant feature rather than being placed in the shared `components/` directory.

---

## `features/`

Business/domain-specific frontend code must be organized under `features/`.

Example:

```text
features/
└── tasks/
    ├── components/
    ├── hooks/
    ├── api/
    ├── schemas/
    ├── types/
    ├── constants/
    ├── utils/
    └── index.ts
```

### Feature Responsibilities

`components/`
: UI used only by that feature.

`hooks/`
: React hooks specific to the feature.

`api/`
: Feature-specific API functions.

`schemas/`
: Validation schemas.

`types/`
: Feature-specific TypeScript types.

`constants/`
: Constants and mappings belonging to the feature.

`utils/`
: Feature-specific utility functions.

`index.ts`
: Public interface of the feature.

Feature folders should only contain subfolders that are actually required.

---

## Feature Public Boundaries

Each feature should expose required functionality through its `index.ts`.

Example:

```ts
export { TaskCard } from "./components/task-card";
export { TaskDetailScreen } from "./components/task-detail-screen";
export type { Task } from "./types/task";
```

Other parts of the application should import through the feature boundary:

```ts
import { TaskCard } from "@/features/tasks";
```

Avoid imports such as:

```ts
import { TaskCard } from "@/features/tasks/components/task-card";
```

from outside that feature.

This prevents external modules from depending on feature internals and makes refactoring safer.

Feature code may use relative imports internally where appropriate.

---

## `lib/`

The `lib/` directory contains shared technical infrastructure rather than feature-specific business logic.

```text
lib/
├── api/
├── auth/
├── realtime/
├── pwa/
├── constants/
├── validation/
└── utils/
```

Examples include:

- Shared API client
- Authentication helpers
- Realtime/WebSocket client
- PWA utilities
- Shared validation helpers
- Application-wide constants
- Shared utility functions

Feature `api/` modules should use the shared API infrastructure instead of creating separate HTTP clients.

---

## `hooks/`

The top-level `src/hooks/` directory contains reusable hooks shared by multiple features.

Hooks used only by one feature should remain under:

```text
features/<feature-name>/hooks/
```

---

## `providers/`

Application-wide React providers belong under:

```text
src/providers/
```

Examples include:

- Ant Design provider
- Query/data provider
- Authentication provider
- Other global context providers

Providers can be composed through a single application provider where appropriate.

Example:

```text
providers/
├── antd-provider.tsx
└── app-providers.tsx
```

---

## `config/`

Application configuration belongs under:

```text
src/config/
```

This can include:

- Environment-related configuration
- Application-level settings
- Runtime configuration
- Validated public environment values

Secrets must not be committed to source control.

---

## `types/`

The top-level `src/types/` directory is for types shared across multiple features.

Types used by only one feature should remain inside:

```text
features/<feature-name>/types/
```

---

## `styles/`

Global style resources belong under:

```text
src/styles/
```

Examples include:

- Shared design tokens
- Global theme variables
- Common styling definitions

Feature-specific styles should remain close to the relevant feature where appropriate.

---

## API Organization

Shared HTTP/API infrastructure belongs under:

```text
src/lib/api/
```

Feature-specific endpoint functions belong under:

```text
src/features/<feature-name>/api/
```

Feature API modules should call the shared API client rather than creating their own independent HTTP configuration.

Authentication and authorization are handled through the shared system architecture. Frontend features should not create independent subsystem authentication mechanisms.

---

## Authentication Organization

Shared authentication helpers belong under:

```text
src/lib/auth/
```

Authentication screens and feature-specific authentication UI belong under:

```text
src/features/auth/
```

Authentication-related routes belong under:

```text
src/app/(auth)/
```

Protected application routes belong under:

```text
src/app/(protected)/
```

Authentication tokens should not be stored in insecure browser storage when the system architecture uses secure HTTP-only cookies.

---

## Realtime Organization

Shared realtime client infrastructure belongs under:

```text
src/lib/realtime/
```

Feature-specific realtime behaviour should remain within the owning feature and consume the shared realtime client.

This keeps transport-level logic separate from domain behaviour.

---

## Testing Structure

Unit and component tests should normally be colocated with the code they test.

Example:

```text
features/
└── tasks/
    └── components/
        ├── task-card.tsx
        └── task-card.test.tsx
```

Shared test support files belong under:

```text
tests/
├── fixtures/
├── mocks/
└── utils/
```

Where API mocking is required, shared Mock Service Worker (MSW) handlers and setup should be placed under the test mocks area.

Tests should follow a clear Arrange-Act-Assert or Given-When-Then structure.

New frontend code should maintain the project-required minimum test coverage threshold of 80%.

Accessibility checks should be included for relevant UI components, including automated checks such as `jest-axe` where appropriate.

Cross-service and end-to-end testing may be maintained in the shared Forever Hotel `tests` repository where required by the project test strategy.

---

## PWA Structure

Subsystems requiring Progressive Web App functionality should use:

```text
public/
├── icons/
└── sw.js

src/
├── app/
│   ├── manifest.ts
│   └── offline/
│       └── page.tsx
│
└── lib/
    └── pwa/
```

The service worker may cache appropriate static resources and application-shell assets.

Authenticated API responses, sensitive operational information, and private user/task data must not be stored in the public service-worker cache.

---

## Required Frontend Project Files

A frontend repository should include the project files required by the technologies and team standards in use, such as:

```text
.env.example
Dockerfile
README.md
eslint.config.*
jest.config.ts
jest.setup.ts
next.config.*
package.json
tsconfig.json
```

Do not commit actual secret `.env` files.

---

## Worker Management System Example

The Worker Management System follows the common frontend standard using routes such as:

```text
worker-management/frontend/src/app/
├── manifest.ts
├── offline/
│   └── page.tsx
│
├── (auth)/
│   ├── login/
│   │   └── page.tsx
│   └── change-password/
│       └── page.tsx
│
└── (protected)/
    ├── layout.tsx
    ├── tasks/
    │   ├── page.tsx
    │   └── [taskId]/
    │       └── page.tsx
    ├── my-tasks/
    │   └── page.tsx
    ├── deliveries/
    │   └── [taskId]/
    │       └── page.tsx
    ├── shift/
    │   └── page.tsx
    ├── performance/
    │   └── page.tsx
    └── profile/
        └── page.tsx
```

WKMS feature modules may include:

```text
features/
├── auth/
├── workers/
├── tasks/
├── task-queue/
├── assignments/
├── escalations/
├── food-deliveries/
├── shifts/
└── performance/
```

Only features that have actually been implemented need to exist in the repository.

---

## Naming Guidelines

Use lowercase kebab-case for folder and file names where appropriate.

Examples:

```text
task-card.tsx
task-detail-screen.tsx
mock-tasks.constants.ts
task-images.constants.ts
```

Use clear names based on the feature or responsibility of the file.

Use standard Next.js file names such as:

```text
page.tsx
layout.tsx
loading.tsx
error.tsx
not-found.tsx
```

where those files are required.

---

## Import and Dependency Rules

To keep feature boundaries clear:

1. Code outside a feature should import from that feature's `index.ts`.
2. Feature-specific implementation should remain inside the owning feature.
3. Shared infrastructure should be placed under `lib/`.
4. Shared visual components should be placed under `components/`.
5. Avoid circular dependencies between features.
6. Avoid moving feature-specific code into global folders only for convenience.
7. Cross-feature dependencies should be kept minimal and intentional.

---

## Consistency Rule

All Forever Hotel subsystem frontends should follow this common structure unless a documented architectural decision requires an exception.

When adding new functionality:

1. Identify the owning feature/domain.
2. Place feature-specific code inside that feature.
3. Keep route files thin.
4. Use shared infrastructure from `lib/`.
5. Use shared presentation components from `components/`.
6. Expose feature functionality through `index.ts`.
7. Add appropriate tests with the implementation.
8. Do not create unused folders or placeholder implementation files only to imitate the full reference tree.

---
