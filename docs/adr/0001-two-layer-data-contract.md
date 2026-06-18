# Two-layer data contract: User layer vs System layer

Every file in the repo belongs to either the **User layer** (personal data and
customizations — `cv.md`, `config/profile.yml`, `modes/_profile.md`, `data/*`,
`reports/*`, …) or the **System layer** (logic, scripts, templates, modes that ship with
releases). The update process may freely overwrite the System layer but must never read,
modify, or delete anything in the User layer.

We chose this split over the obvious alternative — one flat repo updated wholesale —
because the system is meant to be **forked and personalized**: users edit archetypes,
scoring, and negotiation scripts in place. Without a contract, every `git pull` /
`update-system.mjs apply` would clobber their work. The cost is discipline: the rule
"personalization goes in `modes/_profile.md` / `config/profile.yml`, never
`modes/_shared.md`" must be honored by every contributor and agent, and a few files
(e.g. `writing-samples/README.md`) need explicit ownership carve-outs. Full file list:
`DATA_CONTRACT.md`.
