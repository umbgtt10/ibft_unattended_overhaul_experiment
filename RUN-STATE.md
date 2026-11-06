# IBFT unattended overhaul experiment -- run state

Where the run is, for the agent to resume from. Read after every compaction and at the start of every step;
updated before every commit, the last-commit line included. Overwritten as the run goes, committed and
pushed with the record at every commit.

## In progress

- **Part, step, slice:** Part 0, the tooling -- `cargo xtask measure`
- **Last commit:** `766a0aa` Run the measuring tools over an exported tree, for cargo xtask measure (workbook row 9)
- **Next piece of measure:** the `measure` command in `main.rs`: export the index (`checkout-index`) or a revision
  (`archive` + `tar`), empty baseline, everything-on config, shipped crates = members minus validation,
  system-tests, xtask, node-contracts. Commit 10: all three stages anyway.
- **Started (UTC):** 2026-11-06T16:02:08Z; branch `unattended-overhaul` created from `8a775a2`

## Next in line

The slices after this one, in order, each with the criterion that put it there (`SETUP.md`, *Choosing the next
slice*).

| # | slice | kind | `Chosen-By` |
|---|---|---|---|
| 4 | `cargo xtask measure`: offences both ways rule by rule, shear, Grip and Braintax | support | dependency |
| 5 | the dry4rust gate and its baseline (past the threshold, planned) | support | dependency |
| 6 | the 13 unused dependencies, a few commits | rule-offence | dependency |
| 7 | the `cargo-shear` gate, `ignored` entries refused without `reasons` | support | dependency |
| 8 | the harness waits that give up say so; slotgate's timeout 120 -> 160 | -- | risk |
| 9 | the single-node harnesses' ports from the slot's range, and `next_port` | -- | risk |
| 10 | the three conflict-detection tests | tests | safety-net |
| 11 | the coverage baseline per module | support | measurement |
| 12 | the reference-traces driver landed, last (past the threshold, planned) | tests | (plan: last) |

## Parked

| # | slice | parked at | failed by | `QUESTIONS.md` entry |
|---|---|---|---|---|

## Counts

- **Questions:** 0 answered, 0 open.
- **Interventions:** 0.
- **Waits:** 0, 0 hours in all.
- **Commits:** 9 on the branch; the tenth commit runs all three stages.
- **Tests:** `cargo xtask test-inventory [<rev>]` -- 3,440 at the base, 3,503 at `766a0aa`. Before every
  commit: `cargo xtask test-diff HEAD --message <msg file>` over the staged index.

## How the checks are run (until `cargo xtask measure` and the inventory diff exist)

- offences: `git checkout-index -a --prefix=<scratch>/index-cN/`, stern with `--baseline` the empty
  `overhaul-scratch/empty-baseline.json` (as configured), then again with `overhaul-scratch/everything-on.toml`
  copied over the export's `stern4rust.toml`; `cargo shear --locked --format json <export>`;
- tests: stage 1's `test result` lines plus stage 5's slotgate summary (3,042 + 398 at the base);
- times: `date -u` at the slice's choice, at each attempt's start and finish.
- the harness refuses `git switch -c` (deny `git * -c *`), `cp -r`, `awk`, `;`-compound commands: use
  `checkout -b`, single-file `cp`, `grep`.

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
