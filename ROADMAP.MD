# Iron Journal — Roadmap

## Objective
Maintain a personal, hosted fork of Iron Journal V2 for the original Ironsworn and Delve setting and rules, borrowing useful campaign-management features from Stargazer (Starforged) without switching systems.

## Approach
- Fork first, modify incrementally; do not rebuild from scratch.
- Prioritise a working, playable version within roughly two hours of initial development, rather than an extended planning or redesign exercise.
- Preserve existing functionality, campaign data, import/export and attribution. Keep changes small and test after each deployment.
- Use GitHub Pages and automated builds; no new backend, database service, login or subscription.

## Priorities
1. **Baseline deployment — completed:** host Iron Journal V2 unchanged in gameplay terms; confirm browser functionality before adding features.
2. **Clocks — next:** add Stargazer-style campaign clocks for independent developments (e.g. approaching winter, rival clan preparations). Proposed controls: named clocks, configurable segment count, increment/decrement and reset; persist with campaign data and preserve backup compatibility. Inspect Stargazer source for reusable components before implementing.
3. **Narrative organisation — proposed, not committed:** seasons, episodes and scenes; episodes behave like main story quests, with optional side quests and free play between them.
4. **Parallel storylines — proposed:** track multiple independent threads and their progress; allow switching between them.
5. **Random connections — proposed:** select existing NPCs, locations and story threads to generate surprises; consider scoped random pools (episode, season, entire campaign).
6. **Custom oracles and generators — proposed:** extend existing tools where useful.

## Separate future project
An Everspark companion inspired by Iron Journal is a possible separate project, not a requirement for this fork. Reuse system-independent components where practical. Avoid changing Ironsworn rules or expanding scope before the initial version is playable.

## References
- Upstream: https://github.com/nboughton/iron-journal
- Original V2: https://nboughton.uk/apps/iron-journal/
- Stargazer: https://nboughton.uk/apps/stargazer/
- Personal deployment: https://woodymellor.github.io/iron-journal/
- See CURRENTSTATE.MD for implementation status.
