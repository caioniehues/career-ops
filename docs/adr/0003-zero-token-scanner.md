# Zero-token scanner via direct ATS/board APIs

`scan.mjs` discovers new Postings by calling ATS and job-board APIs directly
(Greenhouse, Ashby, Lever, Workday, SmartRecruiters, Recruitee, Workable, RemoteOK,
Remotive, Working Nomads, IBM, …) through a plugin layer in `providers/*.mjs`. It is pure
HTTP + JSON and spends **zero Claude API tokens**. Adding a source = dropping one
`*.mjs` into `providers/`.

We chose deterministic API fetching over having an LLM read careers pages because
scanning runs broadly and often: doing it with model calls would be slow, costly, and
non-reproducible, while ATS APIs return clean structured listings for free. The
trade-off is a per-provider maintenance burden — each ATS needs its own adapter and
breaks when the API shifts — and sources without a usable API need a local parser
(`providers/local-parser.mjs`) or are out of reach. LLM tokens are spent only later, on
Evaluation, where judgment is actually required.
