# Development Workflow

This guide covers the essential workflow for developing, debugging, and managing versions of your custom widgets. You'll learn how to run your widget in standalone mode for quick iteration, connect it to a live portal environment for integration testing, and manage widget versions across your deployments.

---

## Table of Contents

1. [Local Development](#local-development)
2. [Widget Version Control](#widget-version-control)
3. [Connecting Local Widget to Portal](#connecting-local-widget-to-portal)
4. [Understanding Module Federation](#understanding-module-federation)

---

# Local Development

## Dev Mode Launch (Standalone)

**Notice:** Local dev mode primarily used for the initial widget setup and initial UI development stage. For further implementation stages deploy the widget to the platform and continue the development process in a portal environment.

Run widget standalone without platform integration using the Host application (wlabel):

```bash
npm run dev
```

- Uses src/main.tsx as entry point
- Uses src/local-widget.tsx for mocked data
- Runs on configured port (e.g., localhost:5210)

### main.tsx Example

```ts

import React from "react"
import ReactDOM from "react-dom/client"
import { QueryClient, QueryClientProvider } from "react-query"
import { ThemeProvider } from "styled-components"

import Widget from "./widget/widget"
import { mockTheme, mockProps } from "./local-widget"

const queryClient = new QueryClient()

const root = ReactDOM.createRoot(
  document.getElementById("root") as HTMLElement
)

root.render(
  <React.StrictMode>
    <QueryClientProvider client={queryClient}>
      <ThemeProvider theme={mockTheme}>
        <Widget {...mockProps} />
      </ThemeProvider>
    </QueryClientProvider>
  </React.StrictMode>
)
```

### local-widget.tsx Example

This file contains mock data used during development.

```ts
import type { IWidgetProps } from "./types/widget-props"

export const mockTheme = {
  name: "core-light",
  colors: {
    primary: "rgb(19, 183, 209)",
    secondary: "rgb(32, 78, 110)",
    surface: "rgb(255, 255, 255)",
    onSurface: "rgb(45, 56, 66)"
  },
  sizeUnit: 4,
  borderRadius: 3
}

export const mockProps: IWidgetProps = {
  title: "KPI Tracker (Dev Mode)",
  apiUrl: "http://localhost:3000/api",
  refreshInterval: 30,
  showTrend: true
}
```

# Widget Version Control

The platform supports widget versioning. Each widget can have multiple versions, and portals can use either the **latest version** or a **specific fixed version**.

---

### Releasing Your First Version

To release the first version of your widget:

1. Create an initial merge request with a semantic tag in the title.
2. Ensure the MR title begins with `feat:` or `fix:`.
3. After the MR is merged, the first version will be automatically released and added to the portal.

**Example MR titles**

```
feat: initial KPI tracker widget
feat: add KPI tracking functionality
fix: resolve loading issue
```

For more information about semantic tags, see the **Semantic Release documentation**.

---

## Version Management

### Latest Version (Default)

When a widget is added to a portal, the default version is set to **latest**.

This means:

- Each time a new MR with a semantic tag is merged, a new version is released.
- The portal automatically loads the latest version.
- No manual intervention is required for updates.

---

### Fixed Version

To use a specific version of a widget:

1. Open the portal control panel.
2. Navigate to the **Micro Apps Versioning** page.
3. Search for your widget by name.

If your widget is not listed, verify:

- The initial MR has been merged.
- The MR title included the required semantic tag (`feat:` or `fix:`).

4. Click the **cog icon (⚙)** next to your widget.
5. Select the desired version from the **Change Current Version** dropdown.
6. Click **Apply** to save the changes.

The portal will now use the fixed version you selected instead of automatically updating to the latest version.

---

# Connecting Local Widget to Portal

You can debug your local widget development directly in a portal environment.

---

## Steps to Connect

### 1. Set Widget to Dev Version

1. Open the control panel.
2. Navigate to **Micro Apps Versioning**.
3. Find your widget in the list.
4. Click the **cog icon (⚙)** next to your widget.
5. Select **dev-version** in the **Change Current Version** dropdown.
6. Click **Apply**.

---

### 2. Start Local Development Server

Run this command in the root of your widget repository:

```bash
npm run start_federation
```
### 3. Load Widget in Portal

1. Open the portal in your browser.
2. Navigate to a dashboard.
3. Add your widget from the **Micro App Library**.

Your locally running development build will be loaded into the portal.

---

### 4. See Changes

1. Make changes to your widget code.
2. Reload the portal page to see the changes.
3. Debug using browser **DevTools**.

---

## Benefits

Testing widgets in a portal environment allows you to:

- test widgets in a real portal environment
- debug with actual platform APIs and data
- verify integration with other widgets
- test with production-like configuration
- speed up development iteration

---

# Understanding Module Federation

The platform uses **Webpack Module Federation** to load and manage widgets at runtime.

---

## What is Module Federation?

Module Federation is a **Webpack 5 feature** that allows JavaScript applications to dynamically load code from another application at runtime.

In this architecture:

- **Host Application** — the main platform application
- **Remote Applications (widgets)** — loaded dynamically from a widget store
- **Shared Dependencies** — reused from the host application

Typical shared dependencies include:

- React
- React Query
- styled-components

---

## How Widgets Are Loaded

When a dashboard requests a widget, the following steps occur.

---

### 1. Dashboard Requests Widget Metadata

The portal requests dashboard configuration from the backend.

The response includes:

- widget IDs
- widget settings

---

### 2. Check Widget Cache

The platform checks whether the widget code has already been loaded.

If cached, the platform skips directly to rendering.

---

### 3. Insert Script Tag

```html
<script
type="text/javascript"
data-webpack="VENDOR_WidgetName"
async
src="https://{STORE_BASE_URL}/{widgetId}/remoteEntry.js">
</script>
```
### 4. Load Widget Bundle

The browser downloads the widget's **remoteEntry.js** file.

**Widget bundle location:**
```
{STORE_BASE_URL}/{widgetId}/remoteEntry.js
```

---

### 5. Webpack Runtime Resolution

The Webpack runtime performs the following steps:

- locates the widget scope
- resolves the exposed module
- creates a React component instance

---

### 6. Widget Rendering

The widget is rendered with platform-provided props.

The widget can now access shared dependencies from the host application.

The widget appears on the dashboard.

---

# Widget Configuration for Federation

Example `webpack.config.js` configuration:

```javascript
new ModuleFederationPlugin({
  name: "VENDOR_WidgetName",
  filename: "remoteEntry.js",
  exposes: {
    "./Widget": "./src/widget/index",
    "./Preview": "./src/widget/preview"
  },
  shared: {
    react: { singleton: true, requiredVersion: "^18.0.0" },
    "react-dom": { singleton: true, requiredVersion: "^18.0.0" },
    "styled-components": { singleton: true }
  }
})
```

## Shared Dependencies

The host application exposes shared dependencies that all widgets can use:
- React & React DO
- M - UI library
- React Query - Data fetching and caching
- styled-components - CSS-in-JS styling
- /wl-ui-kit - Platform UI components
- /shared-types - TypeScript types

This approach:
- Reduces widget bundle sizes
- Ensures version consistency across widgets
- Prevents multiple React instances
- Improves loading performance

---

# Development vs Production

## Development

Run the widget in federation mode:

```bash
npm run start_federation
```

### Characteristics

- widget runs on **localhost**
- **Hot Module Replacement (HMR)** enabled
- source maps available for debugging
- portal loads the widget from the local development server

---

## Production

After deployment:

- widget bundle is built and uploaded to the widget store
- served from CDN
- optimized and minified
- versioned for caching

---

## Best Practices

### 1. Do Not Bundle Shared Dependencies

Shared dependencies should be provided by the host application.

Avoid bundling:

- React
- React DOM
- shared libraries

---

### 2. Keep Widget Bundles Small

- include only widget-specific code
- use tree-shaking
- lazy load heavy dependencies

---

### 3. Test Federation Locally

Use:

```bash
npm run start_federation
```

Verify that:

- shared dependencies load from the host
- the widget renders correctly inside the platform

---

### 4. Monitor Bundle Size

- Use bundle analysis tools:

```bash
npm run analyze
```

- Keep main bundle under 200KB (gzipped)
