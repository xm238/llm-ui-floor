# llm-ui-floor

**Stop your LLM from generating mid UI. Give it a taste.**

`llm-ui-floor` is a quality control system for LLM-assisted frontend development. It gives any AI coding tool — Codex, Cursor, Claude, Windsurf, Copilot, v0 — a curated component corpus and a decision layer that forces it to build at a defined visual quality bar instead of defaulting to generic vibe-coded output.

It is not a component library. It is a **UI floor** — the minimum quality level your LLM is allowed to ship.

---

## The problem

LLMs generating UI default to the same patterns every time: duplicate KPI cards, random gradients, unstyled Tailwind gray palettes, flat layouts with no hierarchy, and components that look like five different products duct-taped together. Not because they can't do better — because they have no reference for what better looks like in your project.

`llm-ui-floor` fixes that.

---

## How it works

1. You clone this repo and copy the `components/` folder into your project — a handpicked best-of from 21st.dev, organized by category
2. You run a one-time prompt that makes the LLM read every component and build a rich registry (`docs/component-registry.md`) — what each component does, what it looks like, its deps, its props, its merge candidates
3. You add a short instruction block to your LLM's config file (AGENTS.md, `.cursor/rules`, CLAUDE.md, etc.) that tells it to run a decision tree before generating any UI:
   - **Exact fit** → use the component directly
   - **Partial fit** → merge two components intelligently
   - **No fit** → build new, but match the visual quality of the corpus

That's it. Your LLM now has taste.

---

## Component corpus

The components in this repo are a handpicked sweep of **[21st.dev](https://21st.dev)** — the community-built React component platform. The only filter was quality: anything that didn't look vibe-coded went in. They're not from the same author, they don't share one aesthetic, and they weren't designed to work together. That's fine — the registry handles the rest.

All credit for the components goes to the 21st.dev community and their contributors. This repo provides the system, not the components themselves.

---

## Quickstart

### 1. Get the components folder

Clone this repo. The `components/` folder is already in it — that's your corpus, ready to go.

```bash
git clone https://github.com/your-username/llm-ui-floor
```

Then copy the `components/` folder into your own project repo.

---

### 2. Build the registry

Paste this prompt into your LLM tool of choice (Codex, Cursor, Claude, etc.):

```
Scan every subfolder inside `components/`. For every .txt file found, read its full contents and create docs/component-registry.md.

Each entry must include:
- Name: component name from the filename
- Category: the subfolder it lives in
- File path: relative path to the txt file
- What it does: one clear sentence
- Visual character: 2–3 sentences on aesthetic — density, motion behavior, surface treatment, color assumptions, radius style, interaction feel
- Primary use case tag: one from [shell, navigation, data-display, form, feedback, layout, hero, overlay, chart, background, animation]
- Secondary use case tags: any additional relevant tags
- NPM dependencies: exact package names from the txt file's install section
- Props / key arguments: main configurable props
- Context / providers required: any providers or hooks needed
- Merge candidates: other components in the registry this could reasonably combine with
- Codex usage note: one sentence on when to prefer this over building from scratch

Group entries by category subfolder. Use a rich definition-style block per component, not a flat table.

At the top of the file write:
"This registry is the canonical index of the component corpus in components/. Read this file before generating any new UI component. Update it whenever components are added or removed."

If the components/ folder is large, process one subfolder at a time and append results to the same file.
```

This runs once. Re-run it (or run per-subfolder) when you add new components.

---

### 3. Configure your LLM tool

Pick your tool below and add the instruction block to its config file.

---

#### Codex (OpenAI) — `AGENTS.md`

Add to your `AGENTS.md`:

```markdown
## Component-first rule (mandatory)

Before writing any UI code, open `docs/component-registry.md` and run this decision tree:

1. Search by use-case tag and category for what you need
2. Exact fit → copy the component tsx into `components/ui/`, install its listed npm deps, use it directly
3. Partial fit across 2+ components → merge them, preserve both visual grammars, unify spacing and radius
4. No fit → build new, but match the visual quality of the corpus (motion, surface layering, spacing density, type hierarchy, restrained color, full interactive states)

Generating a component without checking the registry first is not allowed.
Every component you ship must look like it belongs next to the components in `components/`.

Also add `docs/component-registry.md` to your session reading list.
```

---

#### Cursor — `.cursor/rules` or `.cursorrules`

Create or edit `.cursor/rules`:

```
Before generating any UI component:

1. Read docs/component-registry.md
2. Search for an exact or partial match by use-case tag
3. Exact match → use it directly (copy tsx to components/ui/, install deps)
4. Partial match → merge components, unify visual grammar
5. No match → build new at corpus quality level

Quality bar: motion is functional not theatrical, surfaces are layered, spacing is tight and on the 4px scale, all interactive states are handled, radius is consistent within components, color is restrained.

Never ship a component that looks generic next to what's already in components/.
```

---

#### Claude (Anthropic) — `CLAUDE.md`

Create `CLAUDE.md` in your repo root:

```markdown
# UI Build Rules

Before writing any UI component, read `docs/component-registry.md`.

Decision tree:
- Exact fit → import directly from components/, install listed deps
- Partial fit → merge, preserve visual grammar of both
- No fit → build new at the same quality level as the corpus

Visual quality checklist before shipping:
- All states handled: hover, active, disabled, loading
- Motion serves a function, not decoration
- Radius consistent within the component
- Color restrained — neutrals carry the surface, one accent guides state
- Spacing on the 4px scale
- Typography has a clear reading order

If what you're about to build wouldn't look at home next to the components in components/, stop and revise.
```

---

#### Windsurf — `.windsurf/rules` or `AGENTS.md`

Windsurf respects both `AGENTS.md` and `.windsurf/rules`. Add to either:

```
UI component rule:
Before generating any component, read docs/component-registry.md.
Run the decision tree: exact fit → use it / partial fit → merge it / no fit → build at corpus quality.
Never generate a component without checking the registry first.
The components in components/ are the quality floor. Match or exceed them.
```

---

#### GitHub Copilot — `.github/copilot-instructions.md`

Create `.github/copilot-instructions.md`:

```markdown
## UI quality rule

Before suggesting any UI component code:
1. Assume docs/component-registry.md exists and contains the component index
2. Prefer reusing or merging components from components/ over generating new ones
3. When building new: match the visual quality of the existing corpus
   - Functional motion only
   - Layered surfaces, not flat blocks
   - Full interactive states
   - Restrained color with one accent
   - Consistent radius within a component
   - 4px-scale spacing
```

---

#### Lovable / v0

These tools don't have a persistent config file in the same way, but you can prepend this to any prompt:

```
Before generating any UI, assume there is a component registry at docs/component-registry.md listing available high-quality components organized by category. Check for exact or partial matches before building new. If building new, match this quality bar: functional motion, layered surfaces, tight spacing on 4px scale, all interactive states handled, restrained color, consistent radius. Do not generate generic vibe-coded output.
```

For Lovable, you can also paste the registry contents directly into the knowledge/context area if the project supports it.

---

## Folder structure after setup

```
your-repo/
  components/
    backgrounds/
      dotted-surface.txt
    buttons/
      glow-button.txt
    cards/
      ...
  docs/
    component-registry.md    ← auto-generated by the registry prompt
    (your other project docs)
  AGENTS.md                  ← Codex config
  CLAUDE.md                  ← Claude config
  .cursor/rules              ← Cursor config
  .github/copilot-instructions.md
```

---

## Keeping the registry fresh

When you add new components to `components/`, re-run the registry-build prompt for that subfolder only:

```
Read all .txt files in components/[category-name]/ and append their registry entries to docs/component-registry.md in the same format as existing entries. Do not rewrite existing entries.
```

---

## Tips for large component sets

If your `components/` folder is very large (1000+ files), run the registry-build prompt per subfolder rather than all at once:

```
Read all .txt files in components/backgrounds/ only and create registry entries for them. Append to docs/component-registry.md.
```

Run once per category. The registry accumulates.

---

## Why this works

LLMs don't generate bad UI because they lack capability. They generate bad UI because they have no project-specific reference for what good looks like. A generic prompt produces generic output. A registry with visual character descriptions, motion behavior notes, and merge candidates gives the model enough signal to make real decisions — not just fill in a template.

The decision tree (use → merge → build) mirrors how a senior developer actually thinks about components. The LLM follows the same logic when it has the same information available.

---

## Credits

**System design and method:** Adil — [@ylq.0](https://instagram.com/ylq.0) · adilmcahidhq@gmail.com

**Component corpus:** [21st.dev](https://21st.dev) and the 21st.dev community. All components are sourced from their platform. This repo provides the deployment method, not the components themselves. Support their work.

---

## License

MIT. Use it, fork it, adapt it. If you build something cool with it, tag [@ylq.0](https://instagram.com/ylq.0).
