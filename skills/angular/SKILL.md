---
name: angular
description: Angular — Google's component-based web application framework for building client, mobile, and desktop apps with TypeScript. Use when building or upgrading Angular apps — components, templates and bindings, signals and computed state, dependency injection, RxJS interop, routing, forms, standalone components, zoneless change detection, or migrating from NgModules and the legacy module-based model. Published by HardGraph, a curated graph of provenance-backed knowledge for AI agents.
metadata:
  display-name: Angular
  category: Web frameworks
  tags: [angular, typescript, signals, rxjs, web]
---

# Angular

> **What is HardGraph?** HardGraph publishes curated, provenance-backed agent skills grounded in reproducible vendor documentation.

Angular is a TypeScript web application framework. Modern Angular (v17+) is
built on **standalone components** (no NgModules required), **signals** for
reactive state, and **zoneless** change detection as the emerging default. The
NgModule-based model still exists but is the legacy path; new code should be
standalone.

## Mental model

| Concept               | Modern default                                                     | Legacy counterpart                              |
| --------------------- | ------------------------------------------------------------------ | ----------------------------------------------- |
| Component declaration | `@Component({ standalone: true })` (standalone is the default now) | declarations in an `@NgModule`                  |
| Reactive state        | `signal`, `computed`, `linkedSignal`, `resource`                   | RxJS `BehaviorSubject`, manual change detection |
| Change detection      | zoneless (`provideZonelessChangeDetection`)                        | zone-based with `NgZone`                        |
| Routing               | `provideRouter` + lazy-loaded routes                               | `RouterModule.forRoot`                          |
| HTTP                  | `provideHttpClient` + `inject(HttpClient)`                         | `HttpClientModule`                              |

## Resolving versions

**Resolve the current Angular version from the registry, not memory.**

```bash
npm view @angular/core version
npx ng version   # local CLI version
```

Angular ships a major release roughly every six months. Standalone, signals, and
the new control flow (`@if`, `@for`, `@switch`) are stable; verify the exact
minor a feature landed in before relying on it.

## Components and templates

A standalone component declares its dependencies in `imports`, not in a module:

```ts
import { Component, input, output } from "@angular/core";

@Component({
  standalone: true,
  selector: "app-greeting",
  template: `<h1>Hello, {{ name() }}</h1>
    <button (click)="selected.emit(name())">Pick</button>`,
})
export class GreetingComponent {
  name = input<string>(); // signal input
  selected = output<string>(); // signal output
}
```

New control flow replaces structural directives with block syntax:

```html
@if (user(); as u) {
<p>{{ u.name }}</p>
} @else {
<p>Not signed in</p>
} @for (item of items(); track item.id) {
<li>{{ item.label }}</li>
}
```

`track` is **required** in `@for` — it is how Angular diffing stays efficient.

## Signals

Signals are the primary reactivity primitive. `computed` derives from them;
`effect` runs side effects when they change; `resource` bridges async data.

```ts
import { signal, computed } from "@angular/core";

const count = signal(0);
const doubled = computed(() => count() * 2);
count.set(1);
count.update((c) => c + 1);
```

Read a signal by calling it (`count()`). This is also what registers the
dependency inside a `computed`.

## Dependency injection

`inject()` is the modern form, used inside a field initialiser or constructor:

```ts
import { Injectable, inject } from "@angular/core";
import { HttpClient } from "@angular/common/http";

@Injectable({ providedIn: "root" })
export class UserService {
  private http = inject(HttpClient);
  load() {
    return this.http.get<User>("/api/user");
  }
}
```

Prefer `providedIn: 'root'` for singletons; use `provideRouter`/`provideHttpClient`
in `app.config.ts` (the standalone bootstrap) instead of module providers.

## RxJS interop

Angular provides `toSignal` / `toObservable` for interop between signals and
observables. Reach for RxJS for time-based transforms (debounce, combine) and
signals for synchronous component state — do not mix them ad hoc.

## Current vs deprecated

- **Standalone over NgModules.** New components are standalone; do not add them
  to a module.
- **New control flow** (`@if`/`@for`/`@switch`) over `*ngIf`/`*ngFor`. The
  structural directives still work but are not recommended for new code.
- **`inject()`** over constructor-parameter DI.
- **Signals** for component state; zoneless change detection where supported.
- Use `ng build`/`ng serve` via the application builder, not the legacy
  browser-based builder.

## References

- [Components](https://angular.dev/guide/components.md)
- [Signals](https://angular.dev/guide/signals.md)
- [Dependency injection](https://angular.dev/guide/di.md)
- [Templates](https://angular.dev/guide/templates.md)
- [Routing](https://angular.dev/guide/routing.md)
- [RxJS interop](https://angular.dev/ecosystem/rxjs-interop.md)
- [Overview](https://angular.dev/overview.md)
- [npm: @angular/core](https://www.npmjs.com/package/@angular/core)
