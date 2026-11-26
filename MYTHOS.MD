# Mythos — Modular Solo Narrative Engine

> **Status:** Design notes / working specification, 26 November 2026. This describes our agreed direction, not a finished ruleset. Numerical examples and proposed procedures are **illustrative**, not confirmed mechanics.

## At a glance

**Mythos is a system-independent, persistent, open-world storytelling engine for solo RPGs.** It combines the **structure of a television series** with the **freedom of a computer RPG such as Skyrim**. Rather than generating a complete plot, it generates thematic direction, prompts and connections, then records what emerges in play.

A setting persists across multiple **seasons** (story arcs). Each season has an **archetype** that biases its kinds of developments. An individual **episode** advances that season's storyline and has a generated title that helps colour its events. Meanwhile the player may follow other seasons, undertake side quests or explore freely. Oracle rolls can draw on the accumulated cast, threads and world history to introduce surprises and unexpected connections.

### One-minute gameplay loop

1. **Create or resume the show:** choose a setting, character(s), RPG rules and Mythos modules. Load existing NPCs, locations, threads, XP and storyline states.
2. **Choose what to pursue:** advance an episode in any season, start or resume a side quest, investigate a thread, or simply explore. Several seasons may be active simultaneously.
3. **Set the narrative context:** the season's archetype, the episode's generated title (where applicable), existing threads and the player's actions influence relevant plot generators and oracles.
4. **Play scenes:** use the underlying RPG for actions and conflict; use Mythos oracles/random tables for questions, twists, encounters and developments. Thematic results are favoured, **not guaranteed**.
5. **Record what changes:** update characters, relationships, locations, threads, new connections, storyline stages, progress clocks, XP and consequences. Previously minor material may become important.
6. **Resolve, pause or switch:** progress a storyline when ready and when its resolution conditions make sense; otherwise leave it open and do something else. The world remembers where each storyline stopped.

## Narrative hierarchy

| Level | Meaning | Example / notes |
| --- | --- | --- |
| **Show** | The persistent setting/world, cast and history. | An ongoing unLondon game; can encompass many seasons. |
| **Season** | One overarching, archetype-driven storyline, comparable to an RPG questline or story arc. | Like Skyrim's Companions, Civil War or Dragonborn questlines. Multiple seasons can overlap. |
| **Episode** | An individual major adventure/storyline quest **within a season**. | Has a title, themes, state and progress; can require several real-life play sessions. |
| **Side quest** | An independent, optional adventure. | May later connect to a season, become an episode, or reveal a bigger story. |
| **Free play** | Open-ended exploration, socialising and spontaneous activity. | No active episode required. |
| **Session** | Real-world time spent playing. | Not the same as an episode. A session may contain part of an episode, side quests or free play. |
| **Thread** | An unresolved question, problem, opportunity or developing situation. | May outlive an episode or cut across several seasons. |

**Important:** Seasons do **not** have to be played in chronological blocks. The player can complete one episode, leave that storyline dormant, advance another, do side quests and return much later. Mythos retains each storyline's current stage. An episode is not necessarily the same thing as a side quest or a single play session.

## Archetypes are interchangeable presets

An archetype describes **the kind of story being told**, not a character class. It sets the season's prevailing tone/themes and influences the interpretation and *probability* of random developments. It is a bias, never a prohibition: anomalies and off-theme results remain possible.

Potential interchangeable archetype packs include:

- **Major Arcana:** e.g. The Chariot favours action/drive; The Moon favours mystery/uncertainty; The Tower favours disruption/collapse.
- **Astrological signs**, **Jungian archetypes**, **Joseph Campbell-derived structures**, or custom sets.
- A Chuubo's-inspired coloured-arc module, if desired.

**Adventure Crafter inspiration:** archetype presets could adjust the weighting or priority of its broad plot-point themes: **Action, Tension, Mystery, Social, Personal**. For example, The Chariot tends towards Action, The Moon towards Mystery, but neither is exclusive. This is **one layer below** the archetype itself: the archetype configures the generator rather than directly dictating the next event. Exact weightings and integration are undecided.

An archetype is primarily attached to a **season**. A side quest or episode might use another independent prompt or theme, and a surprise could seed an entirely new season.

## Episode titles and thematic prompts

Episodes receive evocative titles to suggest subject, tone or motifs, without fixing their plots. Episode generation combines:

- **Season archetype:** longer-term thematic bias.
- **Episode title:** immediate inspiration, e.g. *Smoke and Mirrors*, *The Enemy Within*.
- **Existing threads/characters:** continuity and unresolved consequences.
- **Plot-point generators and oracles:** actual developments, including unexpected results.

Title sources are modular: **TV-style episode titles, idioms, song titles, mixed pools or custom lists**. The Perchance *stock-episodes* source we examined includes a random pool of stock episode titles and idioms; it is a source of prompts, not an official Chuubo's quest requirement. Titles need not refer to existing TV episodes or imply a preset plot.

## Modular stack

| Layer | Swappable sources / examples |
| --- | --- |
| **Show / setting** | Any genre, setting or world; an existing RPG setting or an original one. |
| **Archetype pack** | Tarot/Major Arcana, astrology, Jungian, custom, Chuubo's-inspired arcs. |
| **Story structure** | Episodic questlines, Chuubo's-inspired arcs, Hero's Journey, three-act, custom or open-ended. |
| **Title / inspiration pool** | Stock TV episode titles, idioms, song names, mixed or custom. |
| **Plot generator** | Tana Pigeon's *Adventure Crafter*, *Plot Unfolding Machine*, custom tables, combinations. |
| **Oracles / event tables** | Mythic, Everspark, GameMaster's Apprentice, custom tables, mixtures. |
| **Progression / pacing** | XP thresholds, clocks, milestones, tracks or other options. |
| **Underlying RPG system** | Ironsworn, Into the Odd, another RPG or freeform play. |

Modules should be selectable, combined and saved as reusable presets. An archetype's weights apply only to generators that expose compatible parameters; otherwise its theme can still guide interpretation.

## Persistent narrative memory: nested lists

Mythos maintains structured lists of **characters/NPCs, threads, locations, relationships, factions and important events**.

Rolls should be scope-selectable:

1. **Episode:** things attached to the current episode.
2. **Season:** elements from the whole season, including earlier episodes.
3. **Show/global:** elements from any storyline or side quest across the world.
4. **Automatic scope:** randomly choose a scope, *usually* favouring episode, then season, with a smaller chance of a wider-world connection.

An **illustrative**, unapproved automatic scope distribution is 60% current episode / 30% current season / 10% whole show. Other distributions and player-defined weights should be possible.

**Data-design preference:** maintain **one record per narrative element**, with links/tags to one or more episodes, seasons and scopes; derive nested roll lists from those links. A recurring character should not need to be manually duplicated in several lists.

The engine should let results:

- Reintroduce a known NPC, thread, place or faction instead of inventing something unrelated.
- Create a previously unknown relationship between existing elements.
- Reveal that an apparently separate side quest is part of a larger storyline.
- Connect two seasons, foreshadow a new one or reopen a seemingly resolved matter.
- **Promote** an incidental character/thread from episode-level significance to season-level or show-level significance.

Resolved/dormant material may remain eligible for occasional surprising callbacks. Weights can favour relevant, current elements while preserving rare far-reaching surprises. Connections emerge through gameplay; Mythos need not pre-plan them.

## Storyline state, XP and clocks

Maintain a persistent **status and current stage** for each season, episode, side quest and thread as appropriate. Candidate statuses: **discovered**, **active**, **dormant**, **resolved**, **failed** (provisional). Finished episodes remain in history; advancing a season starts or unlocks its next episode.

### Persistent character XP — revised proposal

We discussed **reversing Chuubo's XP approach**:

- XP is earned across the character's **whole life/campaign**, not separately for each episode.
- XP is **not spent or reset** when starting or completing a quest.
- Increasing XP reflects experience and generally makes **larger or more complex storyline tiers** resolvable.
- A character can **start any tier at any time**, even beyond their current XP. They may investigate or make progress, but the full resolution might be out of reach, and the adventure may be dangerous.
- An experienced character can always pursue **easier, lower-tier adventures** by choice.
- Sometimes side quests or free exploration may be the practical route to gaining the XP or capabilities needed to advance a season.

An illustrative (not agreed) tier ladder was: 0–49 XP minor, 50–99 local, 100–199 complex, 200–399 major, 400+ epic. **Do not treat these numbers as implemented rules.** Narrative scale/complexity must be distinguished from the underlying RPG's combat difficulty.

### Clocks and narrative state

Clocks (or interchangeable progress tracks/milestones) can track:

- **Episode progression** and major obstacles.
- **Season-level progression** across multiple episodes.
- **Side quests, threads** and ad hoc events (threats, schemes, deadlines, relationships).

A clock represents evolving fiction, **not necessarily a predetermined sequence of scenes**. Resolution will normally require a combination of appropriate **narrative state**, enough **progress** and possibly sufficient **XP**. The solo player ultimately judges when to bring a story to a conclusion; meeting an XP threshold makes resolution possible, not automatic or guaranteed. Resolution may succeed, partially succeed or fail.

### Emergent escalation and nested stories (design ideas)

These are particularly promising, **not yet locked rules**:

- A straightforward quest can **escalate** when play reveals greater stakes, scope or danger.
- A major season-level problem can spawn smaller, attainable adventures.
- A dormant storyline can return much later.
- A side quest can become an episode or the seed of a future season.
- Seemingly unrelated threads can merge, fork or cross over.

## Concrete example

A character in **unLondon** has **65 XP**. Three storylines coexist:

- **The Moon** (mystery): currently at Episode 3, *The Vanishing*; the next episode is within the character's current capability.
- **The Chariot** (conflict): at Episode 2, but its decisive confrontation is beyond the character's current XP.
- **The Tower** (upheaval): a recently discovered, much larger storyline that can be explored but not yet settled.

The player chooses a minor side quest instead. An oracle rolls on **show-wide characters**, returning someone introduced in an early Moon episode. That NPC turns out to be involved in the Chariot conflict. A new thread is recorded linking both seasons; a relevant clock advances. The player gains XP, finishes the side quest, then decides which storyline to resume. **None of that crossover had to be written in advance.**

## What to build first (proposed minimal viable tool)

Prioritise a **usable, lightweight journal/dashboard** rather than a complete, prescriptive game system:

1. **Show dashboard:** current character XP; active/dormant seasons, episode stages and side quests.
2. **Season and episode records:** archetype preset, episode title, status, notes and optional clocks.
3. **Persistent narrative lists:** editable characters, threads and places, tagged by episode/season/global scope.
4. **Context-aware random picker:** roll character/thread/location from current episode, season, all-show or weighted automatic scope.
5. **Pluggable prompt/oracle sources:** begin with a few selectable generators; expand later.
6. **Save/load persistent state** and simple session notes; retain completed history.

**Open rules questions:** precise XP awards and tier thresholds; when an episode may advance/resolve; clock sizes and tick triggers; automatic scope probabilities; how archetype priorities map into different generators; whether seasons have a fixed number of episodes; whether there are any automatic triggers for new seasons/crossovers.

## Core design commitments

- **Player-directed open world**, with optional structured plotlines; multiple active seasons.
- **Continuity first:** narrative memory makes surprises relate to existing fiction.
- **Theme without railroading:** weighted outcomes, not mandated plot beats.
- **Emergent connections:** NPCs, threads, episodes and seasons can unexpectedly relate or grow.
- **Replaceable modules**, including archetypes, titles, plot generators, oracles, progress and underlying game rules.
- **Low bookkeeping / quick to play**; avoid recreating the complexity of Chuubo's rulebook.
- **Story scale grows with XP**, while low-stakes play remains a choice.
- **No predetermined ending required:** play reveals what the story is about.

### Influences

*Chuubo's Marvelous Wish-Granting Engine* (thematic arcs, episode/title prompts and narrative pacing), *The Adventure Crafter* (weighted plot-point themes), *Skyrim* (interleaved questlines and free play), serial television (show/seasons/episodes), *Mythic*, *Everspark*, *The GameMaster's Apprentice* and *Plot Unfolding Machine* (modular oracle/plot ideas). These are **inspirations**; Mythos is not presented as their official rules or a faithful adaptation.

## Implementation relationship to Iron Journal

The personal Iron Journal fork now has campaign clocks, an Encounter workspace, optional Menace tracks on foes and general progress tracks, and shared resource bars. These are useful existing building blocks for future Mythos integration, not an implementation of Mythos itself. Seasons, episodes, scoped narrative lists, archetype weighting and Mythos XP progression remain design proposals. See [CURRENTSTATE.MD](CURRENTSTATE.MD) and [ROADMAP.MD](ROADMAP.MD).
