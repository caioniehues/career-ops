# Career-Ops — Domain Context

The shared vocabulary of career-ops, an AI-driven job-search pipeline. This file is a
glossary, not a spec — it defines what each term *means* so agents, contributors, and
users name the same concept the same way. Implementation lives in the scripts, modes,
and `DATA_CONTRACT.md`.

When two words exist for one concept, the canonical term is the heading; the rejected
ones are listed under `_Avoid_`.

## Pipeline & tracking

**Posting**:
A single job listing under consideration — the thing that gets evaluated. Travels from
the Pipeline, through Evaluation, into the Tracker.
_Avoid_: offer (collides with the **Offer** state), job, listing, vacancy.

**Pipeline**:
The inbox of pending Posting URLs waiting to be evaluated (`data/pipeline.md`). A
staging area, not a status.
_Avoid_: queue, backlog, inbox.

**Tracker**:
The application ledger and source of truth for every Posting's lifecycle
(`data/applications.md`). The SQLite `.db` beside it is a derived index, never the
source.
_Avoid_: database, log, spreadsheet.

**Evaluation**:
The act of scoring a Posting against the candidate and producing a Report.
_Avoid_: assessment, review, analysis.

**Report**:
The written output of one Evaluation (`reports/{###}-{slug}-{date}.md`): blocks A–F, a
Legitimacy assessment (Block G), and a Machine Summary.
_Avoid_: writeup, doc, evaluation (that's the act, not the artifact).

## Lifecycle states

The Tracker's `status` field holds exactly one canonical state (source of truth:
`templates/states.yml`). Title-case, no bold, no dates.

**Evaluated**: Report done, decision pending.
**Applied**: Application submitted.
**Responded**: Company replied, not yet interviewing.
**Interview**: Active interview process.
**Offer**: An offer was received from the company.
_Avoid_: using "offer" for the Posting itself — see **Posting**.
**Rejected**: Declined by the company.
**Discarded**: Dropped by the candidate, or the Posting closed.
**SKIP**: Doesn't fit; don't apply.

## Scoring

**Score**:
The global 1–5 fit rating for a Posting, a weighted average of blocks A–F. 4.0+ = worth
applying; below 3.5 = recommend against.
_Avoid_: rating, grade, points.

**Block**:
One scoring dimension in a Report. A–F feed the Score (Match, North Star, Comp,
Cultural signals, Red flags, Global). Block G (Legitimacy) is separate and does **not**
affect the Score.

**North Star**:
The candidate's target career direction — the set of Archetypes a Posting is measured
against for fit.
_Avoid_: goal, objective, target role.

**Archetype**:
A named role pattern the candidate is targeting (defined in `modes/_profile.md`). The
unit of North Star alignment.
_Avoid_: persona, profile, category.

**Proof point**:
A concrete, metric-bearing achievement from the candidate's portfolio, used to back a
Match claim. Sourced from `cv.md` and `article-digest.md` at evaluation time, never
hardcoded.
_Avoid_: highlight, accomplishment, bullet.

**Legitimacy tier**:
Block G's qualitative verdict on whether a Posting is a real, active opening: **High
Confidence**, **Proceed with Caution**, or **Suspicious**.
_Avoid_: confidence, legitimacy score (it is not a number).

**Ghost posting**:
A listing that is expired, fake, or never intended to be filled. The risk Block G exists
to flag.
_Avoid_: dead posting, fake job.

## Data layers

**User layer**:
Files holding personal data and customizations, never touched by updates (`cv.md`,
`config/profile.yml`, `modes/_profile.md`, `data/*`, `reports/*`, …). See
`DATA_CONTRACT.md` for ADR 0001.
_Avoid_: user data, personal files.

**System layer**:
Files holding system logic that updates may overwrite (`modes/_shared.md`, all other
modes, `*.mjs`, templates, `dashboard/*`). Personal data must never live here.
_Avoid_: core, framework files.

**Mode**:
A single self-contained instruction file in `modes/` defining one task the agent can run
(e.g. `oferta`, `scan`, `apply`). The unit of agent behavior.
_Avoid_: command, prompt, skill (a skill *routes to* a mode).

**Profile**:
The candidate's identity and customization. Split deliberately: `config/profile.yml`
(structured identity, targets, comp) and `modes/_profile.md` (narrative archetypes,
negotiation scripts). Both are User layer.
_Avoid_: settings, config (ambiguous between the two files).

## Candidate-facing artifacts

**CV**:
The candidate's canonical résumé (`cv.md`), the single source for skills and Proof
points. Rendered to PDF/LaTeX by the generators.
_Avoid_: resume, profile (that's the Profile).

**Story bank**:
The accumulating set of STAR+R interview stories across Evaluations
(`interview-prep/story-bank.md`).
_Avoid_: examples, anecdotes.

## Scanning

**Provider**:
A plugin in `providers/*.mjs` that fetches Postings from one ATS or board API
(Greenhouse, Ashby, Lever, …). Files prefixed `_` are shared helpers, not Providers.
_Avoid_: scraper, connector, adapter.

**Portal**:
A configured job source the scanner checks, declared in `portals.yml`.
_Avoid_: site, board, source.

**Tracked company**:
A company entry in `portals.yml` the scanner monitors for new Postings.
_Avoid_: watched company, target.
