# Suro

## Decisions

Suro records decisions as ADRs in `docs/adr/`, one numbered file per decision.
This overrides the global `docs/decisions.md` rule for this repo.
The other global rules still apply: pending work goes to GitHub Issues, technical reference goes to `docs/reference.md`, and handoff context goes to `HANDOFF.md`.

## Agent skills

### Issue tracker

Issues live in GitHub Issues on `calmxz/Suro`, through the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

The five default labels: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, and `wontfix`. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: one `GLOSSARY.md` and `docs/adr/` at the repo root. See `docs/agents/domain.md`.
