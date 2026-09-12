# Forever Hotel Frontend UI Standard

## 1. Purpose

This document defines the shared frontend user-interface standard for the
Forever Hotel Integrated Hotel Management System.

The purpose of this standard is to maintain visual and functional consistency
across subsystem frontends while reducing duplicated UI development effort.

The approved Figma designs remain the primary reference for overall page
structure, layout, branding, and colour usage. However, individual controls
such as inputs, buttons, tables, forms, dialogs, date pickers, and status
indicators do not need to be recreated manually when an appropriate reusable
component is available.

---

## 2. Scope

This standard applies to the frontend applications of the following
Forever Hotel subsystems:

1. Hotel Website
2. Manager Dashboard
3. Front Desk System
4. Food Ordering / Guest Application
5. Kitchen Management System
6. Worker Management System

Each subsystem is maintained in its own repository and therefore manages its
own frontend dependencies.

---

## 3. Standard Frontend Technology

The common frontend technology stack is:

- **Framework:** Next.js
- **Language:** TypeScript
- **Primary UI Component Library:** Ant Design (`antd`)
- **Icons:** Ant Design Icons and/or Lucide React
- **Styling:** Ant Design theme tokens, shared CSS variables, and custom CSS
  where required
- **Design Reference:** Approved Forever Hotel Figma designs

Ant Design is the primary reusable UI component library for subsystem
frontends.

---

## 4. Design Reference Policy

The approved Figma designs define:

- Page structure
- Major layout
- Navigation structure
- Brand identity
- Colour palette
- General information hierarchy
- Core user workflows

Frontend developers are not required to reproduce every Figma icon, input,
button, table, or other control exactly.

Where an appropriate Ant Design component exists, it may be used while
preserving the approved overall layout, colour system, and intended workflow.

For example, a Figma search field may be implemented using an Ant Design
`Input`, while a Figma status indicator may be implemented using an Ant Design
`Tag`.

---

## 5. Primary UI Library

Ant Design (`antd`) is the default UI component library for reusable
application controls.

Developers should prefer Ant Design for components such as:

- Button
- Input
- Input.Search
- Select
- Form
- Table
- Tag
- Badge
- DatePicker
- TimePicker
- Modal
- Drawer
- Alert
- notification
- message
- Spin
- Skeleton
- Empty
- Pagination
- Tabs
- Dropdown
- Tooltip
- Popconfirm
- Steps
- Upload
- Descriptions

Ant Design should be used where it provides a suitable implementation without
conflicting with the approved subsystem design.

---

## 6. Custom Components

Ant Design is the default reusable component library, but it is not mandatory
for every visual element.

Custom components may be created when:

- A required component does not have a suitable Ant Design equivalent
- The approved Figma structure requires a significantly different layout
- A subsystem requires a specialized workflow
- A custom navigation shell is required
- Custom branding cannot be represented cleanly using an existing Ant Design
  component
- Using an Ant Design component would unnecessarily complicate the
  implementation

Existing working custom components do not need to be rewritten only to replace
them with Ant Design components.

For example, an already implemented custom sidebar or header may remain if it
matches the approved project structure and colour palette.

---

## 7. Forever Hotel Colour Palette

### 7.1 Brand Colours

| Token | Colour |
| --- | --- |
| Primary Navy | `#1A3C5E` |
| Navy Mid | `#2E5F8A` |
| Navy Light | `#E8EEF4` |
| Gold | `#C9920D` |
| Gold Light | `#FDF6E3` |
| Sidebar / Header | `#12263A` |

### 7.2 Semantic Colours

| Token | Colour |
| --- | --- |
| Success | `#1A6B3C` |
| Success Background | `#E8F5EE` |
| Danger | `#8B1A1A` |
| Danger Background | `#FBEAEA` |
| Warning | `#7A4F00` |
| Warning Background | `#FEF7E6` |

### 7.3 Surface Colours

| Token | Colour |
| --- | --- |
| Page Background | `#F5F3EF` |
| Card Background | `#FFFFFF` |
| Alternate Table Row | `#F9F7F4` |
| Border | `#DDD8CF` |

### 7.4 Text Colours

| Token | Colour |
| --- | --- |
| Primary Text | `#1A1A18` |
| Muted Text | `#5A5650` |

These colours should be used consistently across all subsystem frontends.

---

## 8. Ant Design Theme Configuration

Subsystem frontends should configure Ant Design using the approved
Forever Hotel colour palette rather than relying entirely on the default
Ant Design theme.

A typical configuration may follow this structure:

```tsx
<ConfigProvider
  theme={{
    token: {
      colorPrimary: "#1A3C5E",
      colorSuccess: "#1A6B3C",
      colorError: "#8B1A1A",
      colorWarning: "#C9920D",
      colorBgLayout: "#F5F3EF",
      colorBgContainer: "#FFFFFF",
      colorText: "#1A1A18",
      colorTextSecondary: "#5A5650",
      colorBorder: "#DDD8CF",
    },
  }}
>
  {children}
</ConfigProvider>