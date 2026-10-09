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
* The Developer Studio CLI (`gsds`) — see [Get Started with the CLI](/cli/getting-started). `gsds create --framework react` sets up React, TypeScript, and Vite for you
* Familiarity with [ES modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) and the `export` syntax

## Quick Start

Here is the shape of the simplest possible React widget. These two files show the `init(sdk)` contract only; they are not a project you can build as is:

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

  const root = createRoot(sdk.getContainer())
  root.render(<MyWidget sdk={sdk} />)
  sdk.on('destroy', () => root.unmount())
}
```

**Widget HTML template** (e.g. `widgets/my-widget/index.html`):

```html
<script type="module" src="my-widget.js"></script>
```

JSX must be bundled before the browser can load it. To build and run a React widget, follow [Step-by-Step: Build a React Widget](#step-by-step-build-a-react-widget): `gsds create --framework react` scaffolds the files with the bundler set up, and `gsds build` produces the module.

Paths in widget HTML are relative to the widget's directory (the `source.path` in your registry). The platform wraps your template in a Shadow DOM automatically — you only provide the inner HTML above. See [Repository Layout — Asset Paths](project-setup#asset-paths-in-widget-html) for details.

::: tip React owns the whole container
`sdk.getContainer()` returns the widget's shadow root, and `createRoot(sdk.getContainer())` — what `gsds create --framework react` generates — lets React manage everything inside it. If your widget HTML has other markup you want to keep, such as a loading placeholder, give React a dedicated element instead:

```javascript
createRoot(sdk.$('#root')) // with <div id="root"></div> in your widget HTML
```

:::

## Step-by-Step: Build a React Widget

The `sdk` object does **not** need to be bundled into your widget — your widget receives it as the argument to its `init(sdk)` function at runtime. You **must** bundle React and any other framework dependencies into your widget, but the `sdk` object itself is provided by the platform.

### 1. Scaffold the Widget

From the root of your project, run:

```bash
gsds create --name task-list --framework react --category custom
```

`gsds create` makes `widgets/task-list/` with React, TypeScript, and Vite already configured, and adds the widget's entry to `extensions_registry.json`. The files this guide edits are:

| File | Purpose |
|------|---------|
| `src/main.tsx` | Entry point — Vite bundles it into `dist/widget.js` |
| `src/types.ts` | The `WidgetSDK` and `WidgetProps` types |
| `src/widget.css` | The widget's styles — added to the Shadow DOM by `src/main.tsx` |

The registry entry's `source` points at the build output: `path` is `widgets/task-list/dist` and `entry` is `index.html`.

> **Note:** React and ReactDOM are bundled into your widget output. Tree-shake aggressively and keep your dependency footprint small to minimize bundle size.

### 2. Create the Widget Entry Point

The entry point exports the `init` function that mounts your React app and registers cleanup. The scaffolded **`src/main.tsx`** already does this: it waits for `sdk.whenReady()`, adds `src/widget.css` to the widget's Shadow DOM, mounts the `App` component, and unmounts it on `destroy`.

> The TypeScript examples in this guide import the `WidgetSDK` type from the scaffolded `src/types.ts`. See the [Widget Runtime Reference](/sdk/runtime-reference) for the full interface, and extend `src/types.ts` when you use more of it.

To mount the component you build in the next step instead of `App`, change two lines in `src/main.tsx`. Replace the `App` import with:

```typescript
import { TaskList } from "./TaskList";
```

Then replace the `root.render(<App sdk={sdk} />);` line with:

```typescript
root.render(<TaskList sdk={sdk} />);
```

### 3. Build the React Component

Create **`src/TaskList.tsx`** (you can delete the scaffolded `src/App.tsx`, which this guide does not use):

```typescript
import { useState, useCallback } from 'react'
import type { WidgetSDK } from './types'

interface TaskListProps {
  sdk: WidgetSDK
}

export function TaskList({ sdk }: TaskListProps) {
  const props = sdk.getProps()

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

### 4. Style the Widget

`src/main.tsx` adds `src/widget.css` to the widget's Shadow DOM, so your styles go in that file. Replace the content of **`src/widget.css`** with:

```css
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
```

You do not need to change **`public/index.html`**. It is the widget's HTML template: the platform wraps it in a Shadow DOM, and it holds only the tag that loads the bundle:

```html
<script type="module" src="./widget.js"></script>
```

The `./widget.js` path matches the bundle Vite writes next to `index.html` in `dist/` — the scaffolded `vite.config.ts` names it `widget`. Paths are relative to the widget's directory. See [Repository Layout — Asset Paths](project-setup#asset-paths-in-widget-html) for details.

**Key points:**

* `sdk.getContainer()` returns the shadow root directly — React mounts there, no `<div id="root">` wrapper needed
* Your styles are scoped to the Shadow DOM automatically — they won't leak out or be affected by the host page
* Use `var(--color-action-primary-default, #9254D9)` to adopt the community's branding via design tokens

### 5. Bundle for Production

From the root of your project, run:

```bash
gsds build
```

This installs the widget's dependencies if they are missing and writes `dist/widget.js` — an ES module ready to be published — and `dist/index.html`. Commit the `dist/` directory with the rest of the widget: the platform publishes the files in `source.path`, so `dist/` must be in your repository.

To try the widget in your community before you publish, run `gsds preview` — see [Get Started with the CLI](/cli/getting-started#_5-preview-it-in-your-community).

## Working with Props

When props change at runtime (e.g. an editor updates configuration in the **No-Code Builder**), the `sdk` object emits a `propsChanged` event. Use `useState` and `useEffect` to keep your component in sync:

```typescript
import { useState, useEffect } from 'react'
import type { WidgetSDK } from './types'

function MyWidget({ sdk }: { sdk: WidgetSDK }) {
  const [props, setProps] = useState(sdk.getProps() as { title: string })

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
import type { WidgetSDK } from '../types'

export function useWidgetProps<T extends object>(sdk: WidgetSDK): T {
  const [props, setProps] = useState<T>(sdk.getProps() as T)

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
import type { WidgetSDK } from '../types'

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
  const props = sdk.getProps() as { username: string }
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

## Complete Example

For a larger working React widget (project structure, Vite config, build output, and import map setup), see the [React widget example](https://github.com/gainsight-hub/widgets-repository-template/tree/main/widgets/react_widget). To start a new React widget, use `gsds create --framework react` as shown above.

## Next Steps

* [Widget Runtime](core-concepts) — How widgets are loaded, the `sdk` object's API, and the widget lifecycle
* [Widget Definition Reference](widget-schema) — Define your widget in `extensions_registry.json`
* [Configurable Widgets](configurable-widgets) — Let editors customize your widget via a form in the **No-Code Builder**
* [Repository Layout](project-setup) — How to organize widget files in your repository
