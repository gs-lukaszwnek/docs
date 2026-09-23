---
url: https://developer-portal.gainsight.com/docs/cli/share-dependencies.md
description: >-
  Let widgets on the same page share one copy of a common dependency instead of
  each bundling its own, using the Developer Studio CLI's automatic
  deduplication.
---

# Share Dependencies Across Widgets

Use this guide when two or more widgets in your project depend on the same package — for example a shared UI library — and you want the community to load one copy instead of one per widget.

## Prerequisites

* Two or more widgets under `widgets/` that declare the same package in `package.json` `dependencies`

## Share a dependency between widgets

Add the package as a normal dependency to each widget that needs it, then rebuild:

```sh
gsds build
```

Any package declared in two or more widgets' `dependencies` is **shared**. `gsds build` externalizes it out of those widgets' bundles automatically — no flag, no extra step. `extensions_registry.json` gets an `importMaps` entry pointing the shared package at its resolved version, so widgets on the same community page load one copy instead of one apiece.

A package only one widget uses stays bundled in that widget; nothing changes until a second widget declares the same dependency.

`devDependencies` never participate — build-time-only packages never reach the browser.

## Exclude a package from deduplication

Some packages break when served as a single shared copy — most commonly a CSS-in-JS library whose independently built copies don't share a class-name registry, causing style collisions with no build-time error. Exclude a package or an entire scope in `gsds.json`:

```json
{ "dedupe": { "exclude": ["@angular/*", "styled-components"] } }
```

A trailing `/*` excludes an entire scope. There is no single flag to turn deduplication off project-wide — `dedupe.exclude` is the only opt-out, by design: deduplication only ever activates for a package two or more widgets actually declare, so excluding the specific packages you don't want shared has the same effect.

`gsds init` seeds new projects with `@angular/*` excluded, and backfills it into any existing `gsds.json` that predates this feature and has no `dedupe` key at all. Angular packages served raw from a module CDN skip the Angular Linker and break at runtime — Angular widgets need their `@angular/*` packages bundled, not shared. Non-Angular dependencies that two Angular widgets happen to share (`rxjs`, `tslib`, or any third-party library) still deduplicate normally.

## If two widgets need different versions

If two widgets share a dependency but resolve it to different installed versions, `gsds build` fails and names every widget/version pair — there is one import map entry per registry, so one version has to win.

**Recover:** Align the versions across the affected widgets, or remove the dependency from one of them, then re-run `gsds build`.

## If a widget's build file can't be patched automatically

`gsds build` makes externalization take effect by patching the widget's build config in place: the `externalPackages` array in `vite.config.ts`, or `architect.build.options.externalDependencies` in `angular.json` for Angular widgets. Nothing else in either file is touched.

A widget scaffolded before this feature existed has no `externalPackages` declaration yet — `gsds build` migrates it into the current shape automatically the first time that widget actually shares a dependency with another. This only fails, with a warning naming the widget, when the file's shape can't be confidently patched — a custom `rollupOptions`, or a build block laid out differently than the standard template.

**Recover:** Copy the externals block from a freshly scaffolded widget's `vite.config.ts` into the named widget's file.

## Next steps

* Flags and exit behavior for `gsds build` in the [Command Reference](reference/commands#gsds-build)
* Which files `gsds build` patches in [Project Files](reference/project-files)
* Error text for version conflicts and patch failures in [Troubleshooting](reference/troubleshooting)
