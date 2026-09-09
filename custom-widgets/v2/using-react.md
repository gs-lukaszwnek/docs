---
url: https://developer-portal.gainsight.com/docs/custom-widgets/v2/using-react.md
description: >-
  Build interactive widgets with React — from a minimal hello-world to advanced
  patterns like custom hooks, design-token integration, and error boundaries
---

# Using React

Build interactive widgets with React that run inside your community. This guide covers everything from a minimal hello-world to advanced patterns like custom hooks, design-token integration, and error boundaries.

React widgets use the same `init(sdk)` contract as any other **Custom Widget** — the platform passes your script the `sdk` object, with access to props, design tokens, events, and the shadow root where your React app mounts. For a framework-agnostic overview of how widgets work at runtime, see [Widget Runtime](core-concepts).

## Prerequisites

* **React 18+** and **ReactDOM 18+** (for `createRoot` API)
* A bundler such as **Vite**, **Rollup**, or **webpack** to produce an ES module
* Familiarity with [ES modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) and the `export` syntax

## Quick Start

Here is the simplest possible React widget:

**`my-widget.js`**

```javascript
import React from 'react'
import { createRoot } from 'react-dom/client'

function MyWidget({ sdk }) {
  const props = sdk.getProps()
  return <h1>{props.greeting || 'Hello, World!'}</h1>
}

export async function init(sdk) {
  await sdk.whenReady()

  const root = createRoot(sdk.$('#root'))
  root.render(<MyWidget sdk={sdk} />)
  sdk.on('destroy', () => root.unmount())
}
```

**Widget HTML template** (e.g. `widgets/my-widget/index.html`):

```html
<div id="root"></div>
<script type="module" src="my-widget.js"></script>
```

Paths in widget HTML are relative to the widget's directory (the `source.path` in your registry). The platform wraps your template in a Shadow DOM automatically — you only provide the inner HTML above. See [Repository Layout — Asset Paths](project-setup#asset-paths-in-widget-html) for details.

::: tip Use a dedicated mount container
The platform injects design tokens and CSS variables into the shadow root. Mount React into a **dedicated element** rather than the shadow root itself — otherwise React's reconciliation may remove platform-managed nodes.

```javascript
// Recommended — React owns only its container
createRoot(sdk.$('#root'))

// Avoid for React — the shadow root has platform-managed content
createRoot(sdk.getContainer())
```

`sdk.getContainer()` returns the shadow root directly, which is fine for vanilla JS widgets that manage DOM manually.
:::

## Step-by-Step: Build a React Widget

The `sdk` object does **not** need to be bundled into your widget — your widget receives it through the `init(sdk)` function call at runtime, loaded via the platform's import map. You **must** bundle React and any other framework dependencies into your widget, but the `sdk` object itself is provided by the platform.

### 1. Set Up Your Project

Create a new project for your widget:

```bash
mkdir my-community-widget && cd my-community-widget
npm init -y
npm install react react-dom
npm install -D typescript @types/react @types/react-dom vite @vitejs/plugin-react
```

**`tsconfig.json`**

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "jsx": "react-jsx",
    "strict": true,
    "esModuleInterop": true,
    "outDir": "dist"
  },
  "include": ["src"]
}
```

**`vite.config.ts`**

```typescript
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  build: {
    lib: {
      entry: 'src/index.tsx',
      formats: ['es'],
      fileName: 'widget',
    },
  },
})
```

> **Note:** React and ReactDOM are bundled into your widget output. Tree-shake aggressively and keep your dependency footprint small to minimize bundle size.

### 2. Create the Widget Entry Point

The entry point exports the `init` function that mounts your React app and registers cleanup.

> The TypeScript examples in this guide import a local `WidgetSDK` type. See the [Widget Runtime Reference](sdk-api-reference) for the full interface — you can create your own type file based on that, or skip types and use plain JavaScript.

**`src/index.tsx`**

```typescript
import { createRoot } from 'react-dom/client'
import type { WidgetSDK } from './types/widget-sdk'
import { TaskList } from './TaskList'

export async function init(sdk: WidgetSDK): Promise<void> {
  await sdk.whenReady()

  const root = createRoot(sdk.getContainer())
  root.render(<TaskList sdk={sdk} />)
  sdk.on('destroy', () => root.unmount())
}
```

### 3. Build the React Component

**`src/TaskList.tsx`**

```typescript
import { useState, useCallback } from 'react'
import type { WidgetSDK } from './types/widget-sdk'

interface TaskListProps {
  sdk: WidgetSDK
}

interface WidgetProps {
  title?: string
}

export function TaskList({ sdk }: TaskListProps) {
  const props = sdk.getProps<WidgetProps>()

  const [tasks, setTasks] = useState<string[]>([])
  const [input, setInput] = useState('')

  const addTask = useCallback(() => {
    if (!input.trim()) return
    setTasks((prev) => [...prev, input.trim()])
    sdk.emit('taskAdded', { task: input.trim() })
    setInput('')
  }, [input, sdk])

  return (
    <div>
      <h2>{props.title || 'Tasks'}</h2>
      <ul>
        {tasks.map((task, i) => (
          <li key={i}>{task}</li>
        ))}
      </ul>
      <div style={{ display: 'flex', gap: '0.5rem' }}>
        <input
          type="text"
          value={input}
          onChange={(e) => setInput(e.target.value)}
          onKeyDown={(e) => e.key === 'Enter' && addTask()}
          placeholder="New task..."
        />
        <button onClick={addTask}>Add</button>
      </div>
    </div>
  )
}
```

### 4. Define the HTML Template

The HTML template is the markup you provide in your widget repository. The platform wraps it in a Shadow DOM automatically — you only supply the inner content:

```html
<style>
  :host {
    display: block;
    font-family: system-ui, sans-serif;
  }

  h2 {
    margin: 0 0 1rem;
    font-size: 1.25rem;
    color: var(--color-action-primary-default, #9254D9);
  }

  ul {
    list-style: none;
    padding: 0;
    margin: 0 0 1rem;
  }

  li {
    padding: 0.5rem;
    border-bottom: 1px solid #e5e7eb;
  }

  input[type="text"] {
    flex: 1;
    padding: 0.4rem 0.6rem;
    border: 1px solid #d1d5db;
    border-radius: 6px;
  }

  button {
    padding: 0.4rem 0.8rem;
    background: var(--color-action-primary-default, #9254D9);
    color: white;
    border: none;
    border-radius: 6px;
    cursor: pointer;
  }
</style>

<script type="module" src="widget.js"></script>
```

The `src="widget.js"` path matches the Vite output filename from `vite.config.ts` — copy `dist/widget.js` into your widget directory so it sits alongside `index.html`. Paths are relative to the widget's directory. See [Repository Layout — Asset Paths](project-setup#asset-paths-in-widget-html) for details.

**Key points:**

* `sdk.getContainer()` returns the shadow root directly — React mounts there, no `<div id="root">` wrapper needed
* Your styles are scoped to the Shadow DOM automatically — they won't leak out or be affected by the host page
* Use `var(--color-action-primary-default, #9254D9)` to adopt the community's branding via design tokens

### 5. Bundle for Production

```bash
npm run build
```

This produces `dist/widget.js` — an ES module ready to be published. Include it in your widget repository alongside the HTML template and [Widget Definition Reference](widget-schema) entry.

## Working with Props

When props change at runtime (e.g. an editor updates configuration in the **No-Code Builder**), the `sdk` object emits a `propsChanged` event. Use `useState` and `useEffect` to keep your component in sync:

```typescript
import { useState, useEffect } from 'react'
import type { WidgetSDK } from './types/widget-sdk'

function MyWidget({ sdk }: { sdk: WidgetSDK }) {
  const [props, setProps] = useState(sdk.getProps<{ title: string }>())

  useEffect(() => {
    const unsubscribe = sdk.on('propsChanged', (newProps) => {
      setProps(newProps as { title: string })
    })
    return unsubscribe
  }, [sdk])

  return <h1>{props.title}</h1>
}
```

The `on()` method returns an unsubscribe function, which aligns perfectly with `useEffect` cleanup.

## Advanced Patterns

### Custom Hook: `useWidgetProps`

Extract the props subscription into a reusable hook so every component gets reactive props without duplicating the `on('propsChanged')` boilerplate:

**`src/hooks/useWidgetProps.ts`**

```typescript
import { useState, useEffect } from 'react'
import type { WidgetSDK } from '../types/widget-sdk'

export function useWidgetProps<T extends object>(sdk: WidgetSDK): T {
  const [props, setProps] = useState<T>(sdk.getProps<T>())

  useEffect(() => {
    const unsubscribe = sdk.on('propsChanged', (newProps) => {
      setProps(newProps as T)
    })
    return unsubscribe
  }, [sdk])

  return props
}
```

**Usage in any component:**

```typescript
function MyWidget({ sdk }: { sdk: WidgetSDK }) {
  const props = useWidgetProps<{ title: string }>(sdk)
  return <h1>{props.title}</h1>
}
```

### Custom Hook: `useWidgetSDK`

Create a React context to make the `sdk` object available throughout your component tree without prop drilling:

**`src/hooks/useWidgetSDK.tsx`**

```typescript
import { createContext, useContext, type ReactNode } from 'react'
import type { WidgetSDK } from '../types/widget-sdk'

const WidgetSDKContext = createContext<WidgetSDK | null>(null)

export function WidgetSDKProvider({ sdk, children }: { sdk: WidgetSDK; children: ReactNode }) {
  return (
    <WidgetSDKContext.Provider value={sdk}>
      {children}
    </WidgetSDKContext.Provider>
  )
}

export function useWidgetSDK(): WidgetSDK {
  const sdk = useContext(WidgetSDKContext)
  if (!sdk) {
    throw new Error('useWidgetSDK must be used within a WidgetSDKProvider')
  }
  return sdk
}
```

**Usage in `init`:**

```typescript
export async function init(sdk: WidgetSDK) {
  await sdk.whenReady()

  const root = createRoot(sdk.getContainer())
  root.render(
    <WidgetSDKProvider sdk={sdk}>
      <App />
    </WidgetSDKProvider>
  )
  sdk.on('destroy', () => root.unmount())
}
```

**Usage in any child component:**

```typescript
function UserGreeting() {
  const sdk = useWidgetSDK()
  const props = sdk.getProps<{ username: string }>()
  return <p>Hello, {props.username}!</p>
}
```

### Error Boundaries

Wrap your widget in an error boundary to prevent crashes from taking down the host page:

**`src/components/WidgetErrorBoundary.tsx`**

```typescript
import { Component, type ErrorInfo, type ReactNode } from 'react'

interface ErrorBoundaryProps {
  fallback?: ReactNode
  children: ReactNode
}

interface ErrorBoundaryState {
  hasError: boolean
}

export class WidgetErrorBoundary extends Component<ErrorBoundaryProps, ErrorBoundaryState> {
  state: ErrorBoundaryState = { hasError: false }

  static getDerivedStateFromError(): ErrorBoundaryState {
    return { hasError: true }
  }

  componentDidCatch(error: Error, errorInfo: ErrorInfo) {
    console.error('[Widget Error]', error, errorInfo)
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback || <p>Something went wrong.</p>
    }
    return this.props.children
  }
}
```

**Usage in `init`:**

```typescript
export async function init(sdk: WidgetSDK) {
  await sdk.whenReady()

  const root = createRoot(sdk.getContainer())
  root.render(
    <WidgetErrorBoundary>
      <WidgetSDKProvider sdk={sdk}>
        <App />
      </WidgetSDKProvider>
    </WidgetErrorBoundary>
  )
  sdk.on('destroy', () => root.unmount())
}
```

## React Tips

For general widget best practices (Shadow DOM, styling, cleanup, bundle size), see [Widget Runtime](core-concepts#best-practices). The tips below are React-specific.

* **`await sdk.whenReady()` before mounting**. This ensures the `sdk` object is fully initialized. After that, call `createRoot` and `render` — let React handle async rendering from there.
* **Use `React.lazy` and `Suspense`** for code-splitting within large widgets.
* **Memoize expensive computations** with `useMemo` and `useCallback`.
* **Always call `root.unmount()`** in the `destroy` handler. Failing to do so leaks React's internal state.
* **Ensure React is bundled** in your widget output — the platform does not provide it.

For common widget issues (mount point, styles, props, module errors), see [Common Issues](common-issues#widget-development-issues).

## Template Repository

For a complete working React widget (project structure, Vite config, build output, and import map setup), see the [React widget in the template repository](https://github.com/gainsight-hub/widgets-repository-template/tree/main/widgets/react_widget). Fork it to get started quickly.

## Next Steps

* [Widget Runtime](core-concepts) — How widgets are loaded, the `sdk` object's API, and the widget lifecycle
* [Widget Definition Reference](widget-schema) — Define your widget in `extensions_registry.json`
* [Configurable Widgets](configurable-widgets) — Let editors customize your widget via a form in the **No-Code Builder**
* [Repository Layout](project-setup) — How to organize widget files in your repository
