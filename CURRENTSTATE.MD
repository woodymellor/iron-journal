# Iron Journal — Current State

Last updated: 26 November 2026.

## Baseline
- Repository: https://github.com/woodymellor/iron-journal (public fork, main branch).
- Upstream: Nick Boughton's Iron Journal V2, version 2.2.5, for original Ironsworn and Delve.
- Stack: Vue 3, Quasar 2 / Quasar CLI Webpack, TypeScript, Pinia, Dexie (browser database), Yarn, PWA.
- Original gameplay rules remain unchanged. Campaign clocks have been added. Preserve original licensing and content attribution (LICENSE.md; MIT code, CC BY 4.0 Ironsworn content).

## Deployment
- Intended live URL: https://woodymellor.github.io/iron-journal/
- GitHub Pages source: GitHub Actions (enabled by user).
- quasar.conf.js: added publicPath '/iron-journal/'; existing hash routing retained. Commit: 4d79340928dc9f6c4549a02b78bc7eca7b7ea65a.
- .github/workflows/pages.yml: automated build/deploy on pushes to main and manual dispatch; Node 18, frozen Yarn lockfile, yarn quasar build -m pwa, dist/pwa uploaded to Pages. Commit: 0ee5653c28b4270d2f2d5f1ec977179f3d11519c.
- Original deploy.sh remains in the repository and targets upstream's server; use GitHub Actions instead.
- First workflow completed successfully: https://github.com/woodymellor/iron-journal/actions/runs/45846474039
- Deployment success verified via GitHub; actual browser behaviour and campaign persistence/import/export have not been independently tested.

## Implemented changes
- Removed animated intro and artificial startup delays in src/App.vue; retained genuine initialization and a loading spinner.
- Added optional campaign-wide clocks in src/components/models.ts and src/components/Tracks/CampaignClocks.vue, displayed on Challenges page. Supports 4/6/8/10 segments, manual increment/decrement/reset, probability-based rolls, successful doubles advancing twice, and Begin a Session bulk rolls. Uses existing campaign autosave/export structure; old saves without clocks are supported by an empty-list fallback.
- Clock-to-vow linking and automatic journal entries are not implemented.
- Google Drive integration deferred at user's request.
- Latest code committed; build and browser testing pending.

## Next task
Verify deployment, test startup and clocks, including persistence and backup restoration.

## Working constraints
Keep the project fast and incremental; aim for a usable version rather than a comprehensive redesign. Avoid unnecessary dependencies and infrastructure. Test the implemented changes. See ROADMAP.MD for later, optional features.

## Encounter tab
- Added src/pages/Encounter.vue, navigation and optional persistent encounter data. Uses current PC stats/resources, initiative, action rolls and momentum burn, existing foe progress tracks, rank harm and notes. One encounter per campaign; manually entered foes. Build and browser tests pending.
