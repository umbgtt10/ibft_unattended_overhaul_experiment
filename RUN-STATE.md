# IBFT unattended overhaul experiment -- run state

Where the run is, for the agent to resume from. Read after every compaction and at the start of every step;
updated before every commit, the last-commit line included. Overwritten as the run goes, committed and
pushed with the record at every commit.

## In progress

- **Part, step, slice:** Part A, the rules block -- first slice: `--fix` (ordered-impl-functions,
  leading-constants, test-file-structure)
- **Last commit:** `046324a` Land the reference-traces driver, its first replay byte for byte (workbook row 25) --
  Part 0's last
- **Started (UTC):** 2026-11-06T16:02:08Z; branch `unattended-overhaul` created from `8a775a2`
- **Part 0:** 25 commits, 16:09 to 17:50; 2 past the threshold (the dry4rust baseline, the driver), both planned.

## Next in line

The slices of Part A, in order, each with the criterion that put it there (`SETUP.md`, *Choosing the next slice*).

| # | slice | rules | `Chosen-By` |
|---|---|---|---|
| 1 | `--fix`, one commit or two, the rest of the workspace restored | ordered-impl-functions 122, leading-constants 1, test-file-structure 1 | measurement |
| 2 | the harness: waits that fail on stalled progress; the cargo-build fallback behind a trait, the five tests back | -- | risk |
| 3 | time behind ports, tests off the clock | injected-time 39, timeless-tests 50 | risk |
| 4 | a failure type per concern, top-down | typed-errors 49 | no-scaffolding |
| 5 | one file per implemented type (with the protocol's and control's fields) | implemented-in-one-file 2 | risk |
| 6 | `pub` or private, each made `pub` with tests | public-or-private 30 | fastest-feedback |
| 7 | named values for tuples | distinct-tuple-elements 11 | fastest-feedback |
| 8 | constants given one home | shared-constants 91 | fastest-feedback |
| 9 | a trait-typed field only from a constructor parameter | injected-dependencies 5 | no-scaffolding |
| 10 | records and objects, a module at a time | records-and-objects 51 | no-scaffolding (last) |
| 11 | each record in the module of what uses it | record-home 23 | no-scaffolding (last) |
| 12 | living documents; the dry4rust baseline re-recorded (216 of 217 groups left) | -- | -- |

Owed: `RuntimeTimerHarness::next_port` has no test (everything-on `tested-public-api`) -- Part B's slice.

## Parked

| # | slice | parked at | failed by | `QUESTIONS.md` entry |
|---|---|---|---|---|

## Counts

- **Questions:** 4 answered, 0 open.
- **Interventions:** 0.
- **Waits:** 0, 0 hours in all.
- **Commits:** 25 on the branch, 2 past the threshold (planned); the 30th runs all three stages.
- **Tests:** 3,440 at the base, 3,521 at `046324a` (stage 5 401).
- **Offences:** 483 as configured, 696 everything on, shear 0; Grip 642.59 / Braintax 4,510.09 (0.1425).

## How the checks are run, each commit

1. `date -u` at the slice's choice; implement; `cargo fmt --all`; `git add -A`;
2. `git diff --cached --no-renames --numstat` for the counts and the threshold (18 files, 360 lines);
3. `cargo xtask test-diff HEAD [--message <scratch msg file>]` -- every `changed` read and judged; a `Maps:` line in
   the message for any removal or `fewer` (constraint 6, with its QUESTIONS entry);
4. `cargo xtask measure /home/umberto/Projects/overhaul-scratch/measure-cN` -- offences both ways rule by rule,
   shear, Grip/Braintax; `cargo stern4rust --offence-threshold 0 --format json` for a quick look;
5. traces: `cargo run --release -p validation --example reference_traces -- check
   /home/umberto/Projects/overhaul-scratch/reference-traces-a1460ca/validation/examples/reference_traces/corpus`
   -- "every trace replays byte for byte";
6. `date -u`, then the stages, one call each, output to `overhaul-scratch/stageN.log`: stage 1 checked with
   `grep -h 'test result' ... | grep -vc '0 failed; 0 ignored'` = 0 and `git status` unchanged by fmt; stage 5's
   slotgate `SUMMARY`;
7. commit `-S -F`, push the branch; the workbook row, the attempts, this file; record commit, the two blocklist
   checks one after the other, then push.

- the harness refuses `git switch -c` (deny `git * -c *`), `cp -r`, `awk`, `slotgate` directly: use `checkout -b`,
  single-file `cp`; remove a scratch dir only through `cargo xtask measure` (its own) -- `base-8a775a2`, `index-c1`,
  `replay`, `replay-target` stay.
- stern baseline keys are package-relative paths: a moved file re-keys its entries (`--write-baseline`, check the
  diff is the same count).
- dry4rust: a gate test helper building a gate from four strings duplicates the build gate test's; build it from
  consts in one line. A small enum-to-string match can be a near duplicate of another.

## Easily forgotten

- every commit: `Kind`, `Rule` for a `rule-offence`, `Chosen-By`, `Over`, `Planned`, `Trigger` when unplanned --
  the counts, the threshold and `Stages` written from the staged index, and `Traces` from the replay;
- one kind a commit; tests that come with a change are that change's; a `refactoring` moves no offence count;
- before every commit: the inventory's diff empty or mapped, the assertion comparison unchanged, the traces
  replayed in release and equal to the parent's -- or a `bug-fix` naming the difference in `Traces:` --, the kind's
  rules met (*What each kind allows*), and the stern baseline no larger;
- a commit that changes what the trace driver reads updates it in the same commit, the same values under new names,
  nothing more;
- stages by what the staged files reach; all three every tenth commit and at a part's end; a commit touching the
  cluster harness runs stage 5 three times;
- a red without a cause: one rerun, logged `flake`; never a second;
- five failed attempts or an hour: park the slice;
- no test removed, no assertion relaxed, no `#[ignore]`; a question that needs one is left open;
- one agent, nothing in parallel.
