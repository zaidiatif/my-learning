# Chapter 39: Micro-Frontends in JavaScript

This chapter explores how modern large-scale web applications are being decomposed into independently deployable and maintainable front-end modules — using the micro-frontend architecture.

## 1. What Are Micro-Frontends?

Micro-frontends extend the principles of `microservices` to the `frontend world`.

In a traditional monolithic front-end, all features are part of one large codebase — meaning that every small change requires redeploying the entire application.
Micro-frontends solve this by splitting a web app into `independent`, `self-contained units` — each owned by a separate team, built, deployed, and updated individually.

### Definition

“Micro-frontends are an architectural style where a frontend app is composed of semi-independent fragments that can be developed and deployed separately.”

### Concept Overview

Each micro-frontend (MF) represents a `feature or domain` of the overall product.

For example:

- `CartApp` → Shopping cart logic
- `UserApp` → Authentication and profile
- `ProductApp` → Product listings and search
- `CheckoutApp` → Payments and order confirmation

Each micro-frontend:

- Has its own UI, state, and API interactions.
- Can be built with different frameworks (React, Vue, Angular, etc.).
- Is integrated into a container shell (host app).

### Architecture Diagram

+-----------------------------------------------------+
|                Main Shell / Container               |
|-----------------------------------------------------|
|  Navbar (React) | Product List (Vue) | Cart (Svelte) |
+-----------------------------------------------------+


Each team develops, builds, and deploys their part independently.

## 2. Benefits and Challenges

### Benefits

| Benefit	| Description |
|:--- |:--- |
| Independent Deployment	| Each feature can be released without redeploying the whole app. |
| Team Autonomy	| Teams can own and manage their part of the app end-to-end. |
| Scalability	| Multiple teams can work in parallel without blocking each other. |
| Technology Freedom	| Different micro-frontends can use different frameworks or versions. |
| Incremental Upgrades	| Gradually modernize or rewrite old sections without a full migration. |
| Resilience	| Failures in one module don’t break the entire app. |

### Challenges

| Challenge	| Description |
|:--- |:--- |
| Performance Overhead	| Multiple bundles increase load time if not optimized. |
| Shared Dependencies	| Conflicts between framework versions or libraries. |
| Complex Integration	| Communication and routing between apps require orchestration. |
| Consistent UX	| Ensuring a unified design system across independently developed apps. |
| Cross-App State	| Managing shared state or session data between micro-frontends. |
| CI/CD Complexity	| Requires multi-pipeline management and version control discipline. |

## 3. Implementation Strategies

Micro-frontend architecture can be implemented in several ways depending on the project size, complexity, and team setup.

### a. Build-Time Integration

Each micro-frontend is built and combined during the main app’s build process.

#### Example:

- Each team publishes their build artifact (npm package or static bundle).
- The container app imports them and bundles everything together.

#### Pros:

- Simpler deployment setup.
- Easier debugging and testing.

#### Cons:

- Tight coupling — any change requires rebuilding the container.
- Loses the “independent deployment” advantage.

### b. Run-Time Integration (Dynamic Loading)

Micro-frontends are loaded dynamically at runtime, often from remote URLs.

#### Example:

- Each micro-frontend is deployed independently on a CDN or separate domain.
- The container loads it using Webpack’s Module Federation, import maps, or iframes.

#### Pros:

- Full independence and flexibility.
- No need to rebuild the container.

#### Cons:

- Complex version management.
- Possible latency or caching issues.

### c. Route-Based Composition

Different micro-frontends handle different `routes` or `pages`.

#### Example:

- `/products` → ProductApp
- `/cart` → CartApp
- `/profile` → UserApp

The main shell routes users to the correct app based on URL.

#### Pros:

- Simple and clean isolation.
- Best for loosely coupled modules.

#### Cons:

- Not ideal for tightly integrated UIs (like dashboards).

### d. Component-Level Composition

Micro-frontends share the same page and render side-by-side.
For instance, a product card from ProductApp can appear inside a cart from CartApp.

#### Pros:

- Great for complex, composable UIs.
- Enables highly granular reuse.

#### Cons:

- Harder coordination for styles, state, and events.

### e. Iframe-Based Isolation

Each micro-frontend runs inside an iframe.

#### Pros:

- Strong isolation (CSS/JS).
- Easy to deploy across domains.

#### Cons:

- Performance cost.
- Hard to share data or global events.

## 4. Tools and Frameworks

Let’s explore key tools that make micro-frontends practical in modern JavaScript ecosystems.

### a. Webpack Module Federation

Introduced in Webpack 5, Module Federation allows applications to dynamically import remote modules from other builds at runtime.

#### Example Setup:

#### Host app (`webpack.config.js`):
```js
new ModuleFederationPlugin({
  name: 'host',
  remotes: {
    cart: 'cart@https://cartapp.com/remoteEntry.js',
  },
});
```

#### Remote app (`webpack.config.js`):
```js
new ModuleFederationPlugin({
  name: 'cart',
  filename: 'remoteEntry.js',
  exposes: {
    './Cart': './src/Cart',
  },
});
```

Then dynamically load:
```js
import Cart from 'cart/Cart';
```

#### Benefits:

- Shared dependencies (React, libraries).
- True runtime independence.
- Perfect for continuous deployment.

### b. Single-SPA

Single-SPA (Single-Single Page Application) orchestrates multiple front-end apps built with different frameworks (React, Vue, Angular, etc.) to work together.

#### Example:
```js
import { registerApplication, start } from 'single-spa';

registerApplication({
  name: 'nav',
  app: () => import('navApp/navApp.js'),
  activeWhen: ['/'],
});

start();
```

#### Benefits:

- Framework-agnostic.
- Routing, lifecycle management built-in.
- Great for incremental migration.

### c. import-maps + ES Modules

Modern browsers support loading modules directly via import maps.

#### Example:
```html
<script type="importmap">
{
  "imports": {
    "userApp": "https://cdn.example.com/userApp/index.js"
  }
}
</script>
<script type="module">
  import { UserProfile } from 'userApp';
  UserProfile.render(document.getElementById('app'));
</script>
```

#### Benefits:

- Native browser support (no bundler needed).
- Lightweight and fast.

### d. FrintJS, Luigi, and Others

Other tools that enable micro-frontend development:

- FrintJS: Reactive framework for modular frontends.
- Luigi: SAP’s micro-frontend framework for enterprise dashboards.
- Open Components (OC): Deploy independent UI components as micro-services.

## 5. Best Practices

### Consistent Design System
Use a shared UI library (like a Web Component or design token system) to maintain visual consistency.

### Version Synchronization
Lock shared dependency versions (like React) to prevent duplication.

### Cross-App Communication
Use custom events, RxJS, or shared stores for data passing.

### Routing Strategy
Centralize routing in the container, delegate child routes to sub-apps.

### Performance Optimization

- Lazy-load remote modules.
- Cache static resources.
- Share dependencies via Module Federation’s shared config.

### Monitoring & Error Boundaries
Wrap each micro-frontend in an error boundary to isolate failures.

### Security
Sandbox remote apps if loaded from different domains.

## 6. Example Architecture
```css
/microfrontends/
 ├── container/
 │   └── (Main Shell, Routing, Shared Header)
 ├── products/
 │   └── ProductApp (Vue)
 ├── cart/
 │   └── CartApp (React)
 ├── auth/
 │   └── AuthApp (Angular)
 └── shared/
     └── UI Components, APIs, Design Tokens
```

Each app:

- Runs its own CI/CD pipeline.
- Is deployed to its own domain.
- Registered dynamically in the container.

## 7. When to Use Micro-Frontends

| Use Case	| Suitable? |
|:--- |:--- |
| Large enterprise app with multiple teams	| Excellent |
| Monolithic app needing modular rewrite	| Great migration path |
| Small startup app	| Over-engineered |
| SaaS platforms with multiple clients	| Scalable |
| Real-time dashboards	| Maybe — depends on complexity |

## 8. Summary

| Concept	| Description |
|:--- |:--- |
| Definition	| Breaking the frontend into smaller, independent units |
| Key Benefits	| Scalability, autonomy, independent deployment |
| Main Tools	| Module Federation, Single-SPA, import-maps |
| Integration Types	| Build-time, runtime, route-based, component-level |
| Challenges	| Complexity, shared dependencies, performance overhead |
| Best Practices	| Shared design system, version control, consistent routing |

## Conclusion

Micro-frontends redefine how large front-end architectures scale.
They offer flexibility, independence, and speed — but also demand strong discipline in versioning, design, and orchestration.

**In essence:**
Micro-frontends enable teams to ship faster and scale smarter — when built with the right boundaries and collaboration strategy.