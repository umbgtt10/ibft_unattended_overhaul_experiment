# IBFT unattended overhaul experiment -- run state

Where the run is, for the agent to resume from. Read after every compaction and at the start of every step;
updated before every commit, the last-commit line included. Overwritten as the run goes, committed and
pushed with the record at every commit.

## Now

- **Last commit:** `9d848e2` Hand the client process harness the waits it uses (workbook row 39)
- **Slice in progress:** Part A -- time behind ports, tests off the clock; injected-time done but the cluster
  harness's 20 (decision 2); left: timeless-tests 28 (system-tests: wake signal 6, grpc transport/bus/client
  interface 6, sled reopen, cluster 4, single node 10, ...). Ports, all in `system-tests/src/infra` but `Clock`:
  `Clock` (timer_scheduler.rs, `SystemClock`; returns a `Duration` since the clock's origin), `WallClock`
  (`SystemWallClock`), `Sleeper` (`ThreadSleeper`), `Pause` (`TokioPause`, async). `PollingWait`
  (`src/polling_wait.rs`, clock + sleeper, `until(interval, timeout, probe)`, `sleep`, `elapsed_since`) is the
  harness loop. The client harness takes its `PollingWait` and `Pause` from the tests (stern's
  `injected-dependencies` refused it choosing a `TokioPause` itself); the runtime-timer harness still picks its own
  `PollingWait` in `build_single_node`/`build_restart_from_db` -- the tool does not see through the concrete
  wrapper; to bring it in line, its ~45 test call sites hand it one (a `refactoring`, moves no count).
- **Next:** commit 40 runs all three stages.
- **Commits:** 39 on the branch; 4 past the threshold (3 planned, + `97225b7` for the dry4rust re-record); bug fixes
  1 (`eba863f`); the 40th runs all three stages
- **Tests:** 3,440 at the base, 3,538 now (stage 5 418)
- **Offences:** 318 as configured, 529 everything on, shear 0; Grip 642.59 / Braintax 4,510.09 (0.1425); dry4rust
  baseline 215 groups, 214 matching (the runtime-timer harness's two waits paid in `3868a69`)
- **Questions:** 7 answered, 0 open. **Interventions:** 0. **Waits:** 0.

## The run so far

- **Started (UTC):** 2026-11-06T16:02:08Z; branch `unattended-overhaul` created from `8a775a2`.
- **Part 0:** 25 commits, 16:09 to 17:50.
- **Part A so far:** `--fix` (`03744d2`); the harness: five tests back (`43ead85`), the build fallback behind a
  trait (`41891d0`), its `.exe` bug (`d2cbd53`, `eba863f`), stall-based waits (`7f2fed8`); xtask's timeless tests
  (`4d52581`); time behind ports: scratch names (`e950eb0`), the log's wall clock (`97225b7`), the timer's clock
  (`1b0171b`), the sleeper (`3e9c051`), the client-interface bus
  and the pause (`1e0d986`), the runtime-timer harness and `PollingWait` (`3868a69`), the
  client harness (`9d848e2`).

## Next in line

Part A's slices, in order, with the criterion that put each there (`SETUP.md`, *Choosing the next slice*).

| # | slice | rules | `Chosen-By` |
|---|---|---|---|
| 1 | time behind ports, tests off the clock -- the cluster harness's own reads stay (decision 2) | injected-time 19 to pay, timeless-tests 46 | risk |
| 2 | a failure type per concern, top-down | typed-errors 49 | no-scaffolding |
| 3 | one file per implemented type (with the protocol's and control's fields) | implemented-in-one-file 2 | risk |
| 4 | `pub` or private, each made `pub` with tests | public-or-private 30 | fastest-feedback |
| 5 | named values for tuples | distinct-tuple-elements 11 | fastest-feedback |
| 6 | constants given one home | shared-constants 91 | fastest-feedback |
| 7 | a trait-typed field only from a constructor parameter | injected-dependencies 5 | no-scaffolding |
| 8 | records and objects, a module at a time | records-and-objects 51 | no-scaffolding (last) |
| 9 | each record in the module of what uses it | record-home 23 | no-scaffolding (last) |
| 10 | living documents; the dry4rust baseline re-recorded | -- | -- |

Owed: `RuntimeTimerHarness::next_port` has no test (everything-on `tested-public-api`) -- Part B's slice; nor has
`RuntimeTimerHarness::new`, which the tool stopped reporting at `3868a69` only because it matches calls by name and
arity (seven arguments, like some other `new` a test calls) -- Part B's slice must test it all the same.
Living documents owed: `stern4rust.toml`'s and `CLAUDE.md`'s "483"; stage 2's new gates (shear, dry4rust) in
`CLAUDE.md` and the justfile; the dry4rust baseline (216 of 217 groups match).

## Parked

| # | slice | parked at | failed by | `QUESTIONS.md` entry |
|---|---|---|---|---|

## How the checks are run, each commit

1. `date -u` at the slice's choice; implement; `cargo fmt --all`; `git add -A`;
2. `git diff --cached --no-renames --numstat` for the counts and the threshold (18 files, 360 lines);
3. `cargo xtask test-diff HEAD [--message <scratch msg file>]` -- every `changed` read and judged; a `Maps:` line in
   the message for any removal or `fewer` (constraint 6, with its QUESTIONS entry);
4. `cargo xtask measure /home/umberto/Projects/overhaul-scratch/measure-cN` -- offences both ways rule by rule,
   shear, Grip/Braintax; a rule-offence re-records the stern baseline (`--write-baseline`) after checking
   `baseline_stale_entries` is what it paid and nothing is added;
5. traces: `cargo run --release -p validation --example reference_traces -- check
   /home/umberto/Projects/overhaul-scratch/reference-traces-a1460ca/validation/examples/reference_traces/corpus`;
6. `date -u`, then the stages, one call each, output to `overhaul-scratch/stageN.log`: stage 1 checked with
   `grep -h 'test result' ... | grep -vc '0 failed; 0 ignored'` = 0 and `git status` unchanged; stage 5's `SUMMARY`;
7. commit `-S -F`, push the branch; the workbook row, the attempts, this file; record commit, the two blocklist
   checks one after the other, then push.

- bug-fix check: `git worktree add --detach overhaul-scratch/wt-parent HEAD`, copy the test in, `cargo test
  --manifest-path wt-parent/Cargo.toml --target-dir overhaul-scratch/wt-target ...` compiles and fails; then
  `git worktree remove --force` and `cargo clean --target-dir overhaul-scratch/wt-target`.
- the harness refuses `git switch -c` (deny `git * -c *`), `cp -r`, `awk`, `slotgate`: use `checkout -b`, single-file
  `cp`; only `cargo xtask measure`, `git worktree remove` and `cargo clean` remove directories -- an export made with
  `git archive | tar -x` is removed by writing a `CACHEDIR.TAG` into it (`Signature:
  8a477f597d28d172789f06886806bc55`) and `cargo clean --target-dir` on it. Older leftovers to clear that way:
  `index-c1`, `replay`, `replay-target`, `pending`; `base-8a775a2` once Part C has no use for it.
- everything-on detail for one rule: export the parent and `git write-tree`'s tree, copy
  `overhaul-scratch/stern-all.toml` over each export's `stern4rust.toml`, run `cargo stern4rust --manifest-path
  <export>/Cargo.toml --package <p> --rule <r> --offence-threshold 0` on both and compare.
- stern baseline keys are package-relative paths: a moved file re-keys its entries.
- dry4rust: helpers of five lines and more count, test functions do not; a reshaped recorded duplicate fails.
- a test that newly calls an untested pub function lowers `tested-public-api` (everything on): not a `tests` commit.

## Easily forgotten

- every commit: `Kind`, `Rule` for a `rule-offence`, `Chosen-By`, `Over`, `Planned`, `Trigger` when unplanned --
  the counts, the threshold and `Stages` written from the staged index, and `Traces` from the replay;
- one kind a commit; tests that come with a change are that change's; a `refactoring` moves no offence count;
- before every commit: the inventory's diff empty or mapped, the assertion comparison unchanged, the traces
  replayed in release and equal to the parent's -- or a `bug-fix` naming the difference in `Traces:` --, the kind's
  rules met (*What each kind allows*), and the stern baseline no larger;
- a commit that changes what the trace driver reads updates it in the same commit, the same values under new names;
- stages by what the staged files reach; all three every tenth commit and at a part's end; a commit touching the
  cluster harness runs stage 5 three times;
- a red without a cause: one rerun, logged `flake`; never a second;
- five failed attempts or an hour: park the slice;
- no test removed, no assertion relaxed, no `#[ignore]`; a question that needs one is left open;
- one agent, nothing in parallel.
