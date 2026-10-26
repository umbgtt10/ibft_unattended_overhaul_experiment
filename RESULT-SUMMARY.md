# IBFT unattended overhaul experiment -- result summary

Written at the end of the run, from the measurement script and the workbook; no figure is copied by hand. Each
line answers the bullet of the same number in `SETUP-SUMMARY.md`.

**The bet**: -- (won, or lost on which of its three conditions)

| # | measure | at the base | predicted | actual | verdict |
|---|---|---|---|---|---|
| 1 | commits | -- | about 180 (150-235), nearer 200; 10-30 unplanned | -- | -- |
| 2 | within 15 files and 300 lines | -- | at least 95% | -- | -- |
| 3 | behaviour | -- | no change outside `bug-fix` commits, as far as the tests see: every test green at every commit on the stages it reaches, and at the end | -- | -- |
| 4 | bugs | 4 recorded | the 4, and 10-20 found | -- | -- |
| 5 | tests | about 3,500; coverage per crate | none lost; 10-18% added; coverage not lower | -- | -- |
| 6 | offences | 475 / 696 / 13, under 38 rules | zero, the harness's time reads excepted, held in the baseline | -- | -- |
| 7 | Grip/Braintax | 0.1425 | 0.150 (0.145-0.160) | -- | -- |
| 8 | seams | `node` on 8 crates; `execution-types`; one observer | `node` on 2; `execution-types` gone; five observers | -- | -- |
| 9 | autonomy | -- | no stop for the owner; questions answered on the fly or left open; no environment stop; no intervention | -- | -- |
| 10 | wall-clock | stages 2 min 40 s-2 min 49 s after a change, reference laptop; timed again at the base | about 2 days (1-5), about 45 hours; about 12 minutes a commit | -- | -- |
| 11 | raised and answered or implemented | -- | 25-50 | -- | -- |
| 12 | raised and left open | -- | 10-25 | -- | -- |
| 13 | refused attempts and flakes | Raft one in seven; 1 flake in 36 stage 5 runs | at most one in six; 0-4 flakes; 0-3 parked | -- | -- |
| 14 | the agent's resource use | Raft per commit: 9 calls, 11 thousand output tokens, 4.4 million cached input | 3,300 calls; 3.8 million output; 1.6 billion cached input; 10-23 compactions | -- | -- |
| 15 | peak disk | free disk recorded at the start (reference laptop 42 GB) | at most 30 GB more; never below 10 GB free | -- | -- |
| 16 | files added | 1,088 files | about 355 (260-470); summed about 370 | -- | -- |
| 17 | files removed | -- | about 90 (50-200); summed about 105 | -- | -- |
| 18 | files changed | -- | about 650 (500-800); summed about 1,700 | -- | -- |
| 19 | productive lines | 25,613 in 406 files | about 31,000 (28,500-34,500) | -- | -- |
| 20 | test lines | 110,934 in 510 files | about 122,000 (117,000-130,000) | -- | -- |

## Stops, waits and interventions

Every stop of the run with its cause, every wait with its start and end -- a usage limit's above all -- and every
message from the owner during it, from `QUESTIONS.md`'s interventions table. -- to be written.

## The dry run

The dry run's figures beside the estimates they replaced, and the run's first commits, rehearsed by the dry run,
beside the rest. -- to be written.

## The first question

Can a migration of about 200 commits run unattended, with no behaviour change and no lost test? -- to be answered.

## The second question

Does the structure measurably improve? -- to be answered.

## Raised and answered or implemented

From `QUESTIONS.md`, every entry the run answered on the fly: the question, the answer, and the commit that carried
it where it caused a change. -- to be written.

| # | question | answer | commit |
|---|---|---|---|

## Raised and left open -- delivered as a result

From `QUESTIONS.md`, every entry the run left open, for the owner to implement, discuss, discard, postpone or move
into `etheram-ibft`'s `OPEN_POINTS.md`: the question, the course its slice took meanwhile, and the run's
recommendation; a security-relevant entry by its placeholder alone. -- to be written.

| # | question | course taken meanwhile | recommendation |
|---|---|---|---|

## What the experiment could not see

The limits `SETUP.md` names -- behaviour no test pins, a change hidden inside a commit of the right kind -- and any
found during the run. -- to be written.
