# IBFT unattended overhaul experiment -- run state

Where the run is, for the agent to resume from. Read after every compaction and at the start of every step;
updated before every commit, the last-commit line included. Overwritten as the run goes, committed and
pushed with the record at every commit.

## Now

- **Last commit:** `c42e574` Give partitioning a failure type of its own (row 48)
- **Slice done:** Part A slice 1, time behind ports and tests off the clock -- 13 commits (`4d52581` to `44db935`).
  injected-time holds the cluster harness's 20 alone (decision 2); timeless-tests the wake signal's 6 alone (Q9,
  open). Ports, all in `system-tests/src/infra` but `Clock`: `Clock` (timer_scheduler.rs, `SystemClock`; a
  `Duration` since the clock's origin), `WallClock` (`SystemWallClock`), `Sleeper` (`ThreadSleeper`), `Pause`
  (`TokioPause`, async). `PollingWait` (`src/polling_wait.rs`: `until(interval, timeout, probe)`, `sleep`,
  `elapsed_since`) is every harness and test wait; tests compose real time through `real_time_wait()` in
  `tests/polling_wait_tests.rs`.
- **Slice in progress:** Part A slice 2, a failure type per concern, top-down -- typed-errors 33 left
  (system-tests 26: the client-interface bus 2 and codec 6, `SledStorage::new` and the storage codec 12, the
  context-builder and protocol factories 2, `ClusterConfig` 3; xtask 7: `GateSequence::run` -> `Gate::run` ->
  `CommandRunner` 2, `rust_files`, `crap_report_parser::parse`, `run_log_directory::prepare`). Done: the six
  factories that cannot fail (`85986a6`); `NodeFailure` (`src/node_failure.rs`, `86cb9f5`); `TransportFailure`
  (`8650823`); `PartitionFailure` (`partitionable_transport/partition_failure.rs`, `c42e574`). `NodeFailure`'s
  variants `Storage`, `ClientInterface`, `ContextBuilder`, `Protocol` still carry a `String`. A new failure
  type's test file goes in the test folder mirroring its source's (paired-test-file, everything on). A
  two-variant `Display` can near-duplicate xtask's `TreeFailure`'s (dry4rust): one type per concern. Pattern: `Display` = the old text word
  for word (constraint 7); `Debug` = that text quoted, as a `String`'s was, where `{:?}`/`expect` printed it.
  A recorded dry4rust duplicate reshaped alike: re-record as Q7 and Q13, checked fingerprint by fingerprint.
- **Commits:** 48 on the branch; 4 past the threshold (3 planned, + `97225b7` for the dry4rust re-record); bug fixes
  1 (`eba863f`); the 50th runs all three stages
- **Tests:** 3,440 at the base, 3,555 now (stage 5 435)
- **Offences:** 280 as configured, 491 everything on, shear 0; Grip 642.59 / Braintax 4,510.09 (0.1425); dry4rust
  baseline 213 groups, all matching (re-recorded at `85986a6`, Q13)
- **Questions:** 14 answered, 1 open (9). **Interventions:** 0. **Waits:** 0.

## The run so far

- **Started (UTC):** 2026-11-06T16:02:08Z; branch `unattended-overhaul` created from `8a775a2`.
- **Part 0:** 25 commits, 16:09 to 17:50.
- **Part A so far:** `--fix` (`03744d2`); the harness: five tests back (`43ead85`), the build fallback behind a
  trait (`41891d0`), its `.exe` bug (`d2cbd53`, `eba863f`), stall-based waits (`7f2fed8`); xtask's timeless tests
  (`4d52581`); time behind ports: scratch names (`e950eb0`), the log's wall clock (`97225b7`), the timer's clock
  (`1b0171b`), the sleeper (`3e9c051`), the client-interface bus
  and the pause (`1e0d986`), the runtime-timer harness and `PollingWait` (`3868a69`), the
  client harness (`9d848e2`), the runtime-timer harness handed its wait (`b8fbd48`), the wake
  signal's report (`27580b9`), the infra tests' waits (`97ee66a`), the single-node tests' (`ffe821d`), the cluster tests' (`44db935`); typed errors: infallible factories (`85986a6`), `NodeFailure`
  (`86cb9f5`), `TransportFailure` (`8650823`),
  `PartitionFailure` (`c42e574`).

## Next in line

Part A's slices, in order, with the criterion that put each there (`SETUP.md`, *Choosing the next slice*).

| # | slice | rules | `Chosen-By` |
|---|---|---|---|
| 1 | ~~time behind ports, tests off the clock~~ -- done at `44db935`; left: the cluster harness's 20 (decision 2), the wake signal's 6 (Q9) | injected-time 19 to pay, timeless-tests 46 | risk |
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
`CLAUDE.md` and the justfile.

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
