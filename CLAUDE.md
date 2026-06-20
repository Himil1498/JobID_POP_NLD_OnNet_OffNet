# CLAUDE.md

Guidance for Claude Code when working in this repository.

## Project
A single-file, dependency-free HTML prototype (`erp_workflow_demo.html`) that
visualizes the provisioning workflow across the ERP modules (Job ID List, POP
Feasibility, NLD/Backhaul Feasibility, On Net Backhaul, Off Net End-Customer,
Off Net POP↔POP, and the L1/L2POI registry). It is a **visualization/demo for
review** — built so the conditional fields and cross-module flow from the source
PDFs can be inspected and corrected before real implementation.

## How to run / validate
- **Run:** open `erp_workflow_demo.html` in any browser. No build, no server, no deps.
- **Validate after every edit:** extract the `<script>` and parse it before finishing:
  ```
  node -e "const fs=require('fs');const h=fs.readFileSync('erp_workflow_demo.html','utf8');const m=h.match(/<script>([\s\S]*)<\/script>/);try{new Function(m[1]);console.log('JS OK');}catch(e){console.log('JS ERROR:',e.message);}"
  ```

## Architecture (must follow)
- Everything lives in one file: embedded CSS + one `<script>`. **Keep it a single
  self-contained file — do not add external libraries, frameworks, or a build step.**
- UI is **declarative**: the `MODULES` object defines each module → `stages[]` →
  `fields[]`. Add or change fields by editing the schema, **not** by hand-writing HTML.
- Field objects: `{type,key,label,hint,options,showIf,gate,colspan,...}`.
  Types: `text, num, date, textarea, select, radio, checks, file, info, repeater, custom`.
- Conditional visibility uses `showIf(v,state)` or by building the `fields` array
  inside a `fields:(v)=>{...}` function. Stage gating uses `gate(state)`.
- Shared state is the global `S` (`S.values`, `S.flags`, `S.requests`). Read/write
  field values via `getv(mod,stage,key)` / `setv(...)`.
- Re-render is full-DOM via `render()`; focus + caret are restored automatically.
  Keep field IDs stable so typing is not interrupted.

## Conventions / Rules
1. **Validate JS** (command above) after every change; only finish when it prints `JS OK`.
2. **Single file, no dependencies** — never introduce npm packages, CDNs, or a bundler.
3. **Schema-driven** — implement features by extending `MODULES`/helpers, not ad-hoc DOM.
4. **NLD endpoint terminology is A End and B End** (not D End).
5. **Cross-module requests** go through `raise()` / `nldReconcilePop()` and must be
   **guarded against duplicates** (check `S.requests` by a stable `type` tag).
   Reconcile-style helpers should also remove a request when its condition becomes false.
6. **Auto-generate on selection** where the spec says a request "is generated" — no
   extra button unless asked; avoid leaving stray explanatory messages in the form.
7. **Gaps**: anything ambiguous or missing from the PDFs is flagged inline with a
   `⚠` marker (`gapFlag('Gx')`) and listed in the **Gaps & Assumptions** view. When you
   make an assumption to fill a gap, register it there rather than hiding it.
8. **Ambiguous requirements**: implement the most reasonable best-guess, keep it small,
   and call out the decision so the user can revert. The user iterates and will say
   "revert" if a guess is wrong — make reverts easy (localized, well-scoped edits).
9. **Demo state is in-memory** (resets on reload / via "Reset demo state"). Do not add
   persistence unless explicitly requested.
10. **PDFs are the source of truth** for module logic; re-read the relevant PDF before
    changing a module's stages or fields.

## Git workflow
- Active work happens on a feature branch (currently `revert-nld-abcd-topology`);
  it gets merged/pulled into `main` later. Do **not** commit directly to `main`
  unless asked.
- Commit only when asked. End commit messages with:
  `Co-Authored-By: Claude Opus 4.8 (1M context) <noreply@anthropic.com>`
- Pushing is outward-facing — push when the user has asked or the branch workflow
  clearly expects it.

## Source material
`*.pdf` = original module notes. `Diagram.png` / `Screenshot 2026-06-19 181256.png` =
the NLD A/B topology scenarios. `README.md` = public-facing overview.
