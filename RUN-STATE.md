# IBFT unattended overhaul experiment -- run state

Where the run is, for the agent to resume from. Read after every compaction and at the start of every step;
updated before every commit, the last-commit line included. Overwritten as the run goes, committed and
pushed with the record at every commit.

## In progress

- **Part, step, slice:** Part 0, the tooling -- the harness waits' failure messages (the cluster harness: stage 5
  three times); QUESTIONS.md entry 2 to answer for its kind
- **Last commit:** `f5a0be9` Give slotgate's per-test timeout forty seconds over the harness's longest wait (workbook row 15)
- **Stage 2 now:** stern4rust, cargo-shear, dry4rust, crap4rust per package, twin4rust, iceberg4rust.
- **Traces before the driver lands:** `checkout-index -a -f --prefix=overhaul-scratch/replay/` (the a1460ca driver
  already copied into its `validation/examples/reference_traces`), then `cargo run --release --manifest-path
  overhaul-scratch/replay/Cargo.toml --target-dir overhaul-scratch/replay-target -p validation --example
  reference_traces -- check overhaul-scratch/reference-traces-a1460ca/validation/examples/reference_traces/corpus`.
- **Started (UTC):** 2026-11-06T16:02:08Z; branch `unattended-overhaul` created from `8a775a2`

## Next in line

The slices after this one, in order, each with the criterion that put it there (`SETUP.md`, *Choosing the next
slice*).

| # | slice | kind | `Chosen-By` |
|---|---|---|---|
| 4 | the harness waits that give up say so; slotgate's timeout 120 -> 160 | -- | risk |
| 5 | the single-node harnesses' ports from the slot's range, and `next_port` | -- | risk |
| 6 | the three conflict-detection tests | tests | safety-net |
| 7 | the coverage baseline per module | support | measurement |
| 8 | the reference-traces driver landed, last (past the threshold, planned) | tests | (plan: last) |

## Parked

| # | slice | parked at | failed by | `QUESTIONS.md` entry |
|---|---|---|---|---|

## Counts

- **Questions:** 1 answered, 0 open.
- **Interventions:** 0.
- **Waits:** 0, 0 hours in all.
- **Commits:** 15 on the branch, 1 past the threshold (the dry4rust baseline, planned); the 20th runs all three
  stages.
- **Tests:** 3,440 at the base, 3,516 at `f5a0be9`. **Shear:** 0 since `1663af1`.
- **xtask gate tests:** a test helper building a gate with four strings duplicates the build gate test's; build
  it from consts in one line (dry4rust caught it twice).

## How the checks are run, each commit

1. `date -u` at the slice's choice; implement; `cargo fmt`; `git add -A`;
2. `git diff --cached --no-renames --numstat` (and `--name-status`) for the counts and the threshold;
3. `cargo xtask test-diff HEAD --message <scratch msg file>` -- the inventory's diff and the assertion
   comparison over the index; every `changed` read and judged; a `Maps:` line for any removal (constraint 6);
4. `cargo xtask measure /home/umberto/Projects/overhaul-scratch/measure-cN` -- offences both ways rule by rule,
   shear, Grip/Braintax of the index; the parent's figures are the previous row's;
5. the traces (once the driver has landed);
6. `date -u`, then the stages, one call each, output to `overhaul-scratch/stageN.log`: stage 1 checked with
   `grep -h 'test result' ... | grep -vc '0 failed; 0 ignored'` = 0; stage 5's slotgate `SUMMARY` line;
7. commit `-S -F`, push the branch; the workbook row, the attempts, this file; record commit, the two blocklist
   checks one after the other, then push.

- the harness refuses `git switch -c` (deny `git * -c *`), `cp -r`, `awk`: use `checkout -b`, single-file `cp`.
- `overhaul-scratch/base-8a775a2` (an export of the base) and `index-c1` stay: nothing allowed removes a
  directory but `cargo xtask measure`, which removes its own.

## Easily forgotten

- every commit: `Kind`, `Rule` for a `rule-offence`, `Chosen-By`, `Over`, `Planned`, `Trigger` when unplanned --
  the counts, the threshold and `Stages` written from the staged index, and `Traces` from the replay;
- one kind a commit; tests that come with a change are that change's;
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
