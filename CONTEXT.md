# openfisca/openfisca-core context
> refreshed 2026-09-06 | upstream default: master @ 0e4be150c

## Identity & policies
- upstream: openfisca/openfisca-core, default branch `master`, primary language Python, English-first (yes — code, docs, PRs in English).
- CLA/DCO: none (no CLA bot, no DCO sign-off in CONTRIBUTING).
- AI-assisted PR policy: unstated. No AI_POLICY.md, no AGENTS.md, no org-level `.github` repo. Note: issue #1372 is an open RFC proposing an AI contribution policy (not adopted) — maintainers are sensitive about AI-looking contributions; keep PRs high-quality and human-sounding.
- signed commits required: no.
- PR template: `.github/PULL_REQUEST_TEMPLATE.md` (4 sections: Breaking changes / New features / Deprecations / Technical changes). Fill verbatim.
- external tracker: github.
- CONTRIBUTING: GitHub Flow, PRs to `master`, peer review required, SemVer version bump + CHANGELOG.md entry required (enforced by `.github/is-version-number-acceptable.sh`).

## Conventions (verified from merged PRs)
- branch naming: mixed — `fix/...`, `feat/...`, `perf/...`, `chore/...`, and plain words (`update-reuses`, `eu-oss-link`, `build-test`, `flask3`, `publiccodeyml`). No dominant pattern; fall back to `type/desc`.
- commit style: imperative, no strict Conventional Commits.
- test command: `make test` (runs `openfisca test` on core modules + `pytest tests/`).
- lint command: `make lint` (compileall + isort + black + flake8 + codespell + doc lints).
- CI: heavy matrix (Python 3.10-3.13 × numpy 1.24-2.5, conda + pip). `check-version` job enforces version bump + CHANGELOG on functional changes.
- outside PRs merge: yes, regularly (benjello, guillett, MattiSG, benoit-cty, HugoFara all merge).

## Maintainer picture
- active maintainers: benjello, guillett, MattiSG, benoit-cty, HugoFara.
- response latency: generally responsive; recent PRs merged within days.
- in-flight areas (avoid): as_of/transition_formula (benjello), entity-links (benjello), numpy 2.5 compat (HugoFara), excel reform (guillett).

## Issue-area health
- No maintainer-engaged, actionable, small open issue survives triage (2026-09-06):
  - #1384 [RFC] Drop Conda support — RFC/discussion.
  - #1372 RFC: AI contribution policy — discussion only.
  - #1369 transition-formula bugs — complex, feature-branch related.
  - #1359 type annotation suggestion — no maintainer engagement.
  - #1356 restore tag publication — CI/release, complex.
  - #1353 publiccode.yml sync — tooling suggestion, no engagement.
  - #1316 `assert_near` str values — `kind:fix`, 0 comments, stale (2024-11), but maintainer guillett proposed a fix (commit ba1ff4580, never merged).
  - #1308 fix max_depth default — `kind:fix`, 0 comments, stale.
  - #1155 clone logic bug — maintainer benjello said "should be enforced" (2022), complex/old.
  - #1278 detect duplicate modify_parameters — feature request.
  - #917/#916 — stale.

## Gap ledger (dedupe — READ FIRST, never re-pick)
- `2026-09-06` self-found gap: `assert_near` fails on `str` variables (bytes dtype `|S{n}` vs unicode target) — outcome: pr-opened — lesson: maintainer guillett's proposed fix (commit ba1ff4580) was incomplete (didn't handle bytes dtype); PR #1228 (closed-unmerged) was a larger refactor. Fix: route str/bytes/object value arrays to a string comparison in `assert_near`.

## Mined gaps (discovered, not yet attempted)
- `2026-09-06` tests-ci `assert_near` str handling: `assert_near` casts str/bytes value arrays to float, so a `str` variable (e.g. `postal_code`) with a non-numeric value fails with a bytes-vs-unicode mismatch; numeric-looking strings are silently compared as floats. Repro: YAML test with `postal_code: abcde` output fails. Expected: string values compared as strings. Proposed test: `test_str`, `test_str_list`, `test_str_bytes`, `test_str_object` in `tests/core/tools/test_assert_near.py`. Dedupe: issue #1316 open; PR #1228 closed-unmerged; no open upstream PR. — status: attempted (pr-opened)
