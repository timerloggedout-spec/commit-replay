# Commit Replay

**Owner-original.** A compile-and-file method on the **git plane**:
new files are created from **pointers** to existing fragments
(functions / classes / snippets) in git history, instead of being
written again. A recipe costs its pointer list, not file content —
the metric is LLM tokens down.

- Spec of record: `DESIGN.md` (commit-store/v1: gobject / gpointer /
  gindex / grecipe).
- Engine (v1, feature-complete): `scripts/commit_replay/`
  {`extract` ∘ `fragments` ∘ `compose` ∘ `jit` ∘ `edit`}
  - `extract.py` — delta-compact index of git history (gindex)
  - `fragments.py` — fragment index: symbol + line-range per blob
  - `compose.py` — materialize a file from a recipe; `--check` verifies
    bytes **and** recipe lineage parent hashes (fail-closed)
  - `jit.py` — resolve symbol-only recipe segments just-in-time
  - `edit.py` — surgical recipe ops (replace/drop/reorder/insert/
    customize); lineage hash chain mandatory on output
- Extracted 2026-11-02 from `timerloggedout-spec/termux-monorepo`
  (engine home per Replay Systems ruling; the monorepo keeps the
  import-by-reference consumer record: design doc, config, recipes,
  indexes). Sibling system: Repo Replay (session plane) — planes are
  separate forever; cross-plane contact is verify-diff only.
