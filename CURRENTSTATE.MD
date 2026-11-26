# Iron Journal — Current State

Last updated: 26 November 2026.

## Baseline
- Repository: https://github.com/woodymellor/iron-journal (public fork, main branch).
- Upstream: Nick Boughton's Iron Journal V2, version 2.2.5, for original Ironsworn and Delve.
- Stack: Vue 3, Quasar 2 / Quasar CLI Webpack, TypeScript, Pinia, Dexie (browser database), Yarn, PWA.
- Existing gameplay functionality has not been modified. Preserve original licensing and content attribution (LICENSE.md; MIT code, CC BY 4.0 Ironsworn content).

## Deployment
- Intended live URL: https://woodymellor.github.io/iron-journal/
- GitHub Pages source: GitHub Actions (enabled by user).
- quasar.conf.js: added publicPath '/iron-journal/'; existing hash routing retained. Commit: 4d79340928dc9f6c4549a02b78bc7eca7b7ea65a.
- .github/workflows/pages.yml: automated build/deploy on pushes to main and manual dispatch; Node 18, frozen Yarn lockfile, yarn quasar build -m pwa, dist/pwa uploaded to Pages. Commit: 0ee5653c28b4270d2f2d5f1ec977179f3d11519c.
- Original deploy.sh remains in the repository and targets upstream's server; use GitHub Actions instead.
- First workflow completed successfully: https://github.com/woodymellor/iron-journal/actions/runs/45846474039
- Deployment success verified via GitHub; actual browser behaviour and campaign persistence/import/export have not been independently tested.

## Next task
Add Stargazer-style clocks while retaining the original Ironsworn setting and mechanics. Compare the two codebases to determine whether the clock component and persistence logic can be reused. No clock implementation or other new gameplay feature has been added yet.

## Working constraints
Keep the project fast and incremental; aim for a usable version rather than a comprehensive redesign. Avoid unnecessary dependencies and infrastructure. Confirm baseline browser functionality, then implement and test clocks. See ROADMAP.MD for later, optional features.
