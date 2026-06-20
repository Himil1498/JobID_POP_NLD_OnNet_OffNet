# JobID / POP / NLD / On-Net / Off-Net — ERP Workflow Demo

A self-contained, dependency-free HTML prototype that visualizes the provisioning
workflow across six connected ERP modules, plus the L1/L2POI registry. Built to
review the flow and conditional fields described in the source PDFs before
implementation.

## Open the demo
Open **`erp_workflow_demo.html`** in any modern browser. No build step, no server.

## Modules
1. **Job ID List** — entry decision tree (On Net / Off Net → media → POP availability).
2. **POP Feasibility** — the hub: Stage 1–7, Juggle survey, and the PR pipeline
   (Rack/Power → Device → Cables → Last Mile Deployment → Provisioning → Acceptance).
3. **NLD / Backhaul Feasibility** — parallel to POP; A–B–C–D topology model with
   per-end Direct / On-Net / Off-Net handoff, Parent Node, and request generation.
4. **On Net Backhaul** — initiates after POP code (Stage 3).
5. **Off Net — End Customer** — Dark Fiber / Broadband from Job ID.
6. **Off Net — POP↔POP Backhaul** — initiates after POP code.
7. **L1/L2POI** — Layer-1 / Layer-2 interconnect registry.

## Demo features
- **Connected simulation** — actions in one module raise requests in others
  (live status badges in the sidebar).
- **Conditional fields** — fields show/hide per the source-PDF logic.
- **Flow & Dependencies** and **Gaps & Assumptions** views.
- **"Show captured data"** per module — live JSON of everything entered.

## Source material
The `*.pdf` files are the original module notes. `Diagram.png` /
`Screenshot 2026-06-19 181256.png` capture the NLD topology scenarios.

> Demo state is in-memory (resets on reload). Gaps found in the PDFs are flagged
> inline with ⚠ markers and listed in the Gaps view.
