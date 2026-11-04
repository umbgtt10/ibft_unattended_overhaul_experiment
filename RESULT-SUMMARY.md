# IBFT unattended overhaul experiment -- result summary

Written at the end of the run, from Part C's measurements, the history's re-derivation and the workbook; no figure
is copied by hand. Each
line answers the bullet of the same number in `SETUP-SUMMARY.md`.

**The bet**: -- (won, or lost on which of its three conditions)

| # | measure | at the base | predicted | actual | verdict |
|---|---|---|---|---|---|
| 1 | commits | -- | about 200 (165-260), nearer 220; 10-30 unplanned | -- | -- |
| 2 | within 18 files and 360 lines | -- | at least 95% | -- | -- |
| 3 | behaviour | -- | no change outside `bug-fix` commits, as far as the tests see: every test green at every commit on the stages it reaches, and at the end; the base's cluster tests and the reference traces hold on the final code | -- | -- |
| 4 | bugs | 4 recorded | the 4, and 10-20 found | -- | -- |
| 5 | tests | 3,440; coverage per crate | none lost and none relaxed outside the licence, every use named; 10-18% added; coverage not lower; every function covered at the base still covered | -- | -- |
| 6 | offences | 483 / 696 / 13, under 39 rules | zero, the harness's time reads excepted, held in the baseline | -- | -- |
| 7 | Grip/Braintax | 0.1425 | 0.150 (0.145-0.160) | -- | -- |
| 8 | autonomy | -- | no stop for the owner; questions answered on the fly or left open; no environment stop; no intervention | -- | -- |
| 9 | wall-clock | stages 1 min 25 s-1 min 30 s after a change, the run's laptop at the base | about a day (20-65 hours), about 26 hours; about 3.5 minutes a commit | -- | -- |
| 10 | raised and answered or implemented | -- | 25-50 | -- | -- |
| 11 | raised and left open | -- | 10-25 | -- | -- |
| 12 | failed attempts and flakes | none in 30 stage 5 runs at the base | reported, not predicted; 0-4 flakes; 0-3 parked | -- | -- |
| 13 | the agent's resource use | -- | 1,500 calls; 2.2 million output; 0.75 billion cached input; 3-8 compactions | -- | -- |
| 14 | files added | 1,071 files | about 360 (265-475); summed about 375 | -- | -- |
| 15 | files removed | -- | about 90 (50-200); summed about 105 | -- | -- |
| 16 | files changed | -- | about 650 (500-800); summed about 1,700 | -- | -- |
| 17 | productive lines | 25,648 in 406 files | about 31,000 (28,500-34,500) | -- | -- |
| 18 | test lines | 109,966 in 491 files | about 122,000 (116,500-130,000) | -- | -- |

## Every commit, re-derived

Part C's last check: every commit of the branch re-derived from the history alone -- its counts, its kind against its
diff, its inventory and assertions, each `bug-fix`'s test on its parent, and its three stages -- beside what the
record says. Every mismatch as found, never corrected. -- to be written.

| # | commit | kind | record | re-derived | mismatch |
|---|---|---|---|---|---|

## Stops, waits and interventions

Every stop of the run with its cause, every wait with its start and end -- a usage limit's above all -- and every
message from the owner during it, from `QUESTIONS.md`'s interventions table. -- to be written.

## The first question

Can a migration of about 200 commits run unattended, with no behaviour change and no lost test? -- to be answered.

## The second question

Does the structure measurably improve? -- to be answered.

## Raised and answered or implemented

From `QUESTIONS.md`, every entry the run answered on the fly: the question, the answer, and the commit that carried
it where it caused a change. -- to be written.

| # | question | answer | commit |
|---|---|---|---|

## Protocol decisions taken along the way

Every suspected bug that was a protocol decision -- a timeout's value, a quorum rule, an order of effects no document
fixed -- and that the run fixed under decision 12: what it was, the decision taken, the test that pins it, and the
commit; a security-relevant one by its placeholder alone. -- to be written.

| # | suspected bug | decision taken | test | commit |
|---|---|---|---|---|

## Raised and left open -- delivered as a result

From `QUESTIONS.md`, every entry the run left open, for the owner to implement, discuss, discard, postpone or move
into `etheram-ibft`'s `OPEN_POINTS.md`: the question, the course its slice took meanwhile, and the run's
recommendation; a security-relevant entry by its placeholder alone. -- to be written.

| # | question | course taken meanwhile | recommendation |
|---|---|---|---|

## What the experiment could not see

The limits `SETUP.md` names -- behaviour no test pins, a change hidden inside a commit of the right kind -- and any
found during the run. -- to be written.
