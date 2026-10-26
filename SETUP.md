# IBFT unattended overhaul experiment -- setup

`etheram-ibft` under every stern4rust rule, then bookkeepers as plug-ins, run unattended as an experiment.

## Status

**Draft, written from discussions with the owner**, and committed as drafts while it changes. It is
frozen in a commit of its own when the run starts, its subject beginning `Freeze:`: from then on a prediction is
never edited, only amended in a dated addendum that gives its reason.
The six documents of the experiment are listed under *Reporting*.

**Nothing in it starts before all three of:**

1. **The owner's current deadline elsewhere** has passed.
2. **Raft validated**: `etheram-raft`'s Phase 11 closed -- its step 31, the consumer `etheram-raft-embassy`, and
   its P9, that consumer's stages 4 to 6 on the rig -- and both repositories merged.
3. **A branch of its own** in `etheram-ibft`. `main` stays as it is: it is a tagged base other work depends on,
   and `etheram-ibft-embassy` builds it by `branch = "main"`.

And one the work itself needs: **every house tool released**, and the run on those releases. stern4rust 0.18.0
carries 38 rules: 0.17.0's 24, the ten Raft's rules block applied, and four that block did not meet -- R035
`records-and-objects`, R037 `record-home`, R038
`injected-dependencies` and R039 `distinct-dev-dependencies`. Every one is enforced here. Beside it, the releases of
dry4rust, crap4rust, twin4rust, iceberg4rust, slotgate, grip4rust and braintax4rust, their versions recorded at the
freeze.

**Then, before the freeze** (*Before the freeze*): the guard built, tested and reviewed once by the owner; a dry run
of about 25 commits, on branches of its own and pushed as the run pushes; the predictions it can measure recalibrated
from it; Part B's design decisions answered. Some hours of running, and the two largest unknowns gone before anything
is frozen.

**The experiment ends with Part B's last commit, and its tests are its validation**: every test -- the base's, the
new ones and the changed ones -- green at every commit on the stages its change reaches, and all of them at the end.
The further checks -- the base's cluster tests against the final binary, reference traces, coverage followed function
by function, a mutation sample -- and the consumer, `etheram-ibft-embassy` retargeted, its stages 1 to 6 on the rig
and a short sweep, are optional follow-ups after the experiment, outside its scope and not its evidence. They are
planned in `etheram-ibft`'s `docs/history/[PROPOSAL] UNATTENDED OVERHAUL FINAL CHECKS AND CONSUMER.md`.

**It is run as an experiment**: every commit follows the rules in *The experiment* below, and says so.

**The bet**: the run completes Parts A and B with no intervention from the owner, no test lost, and every test -- the
base's, the new and the changed -- green at every commit. Any one of these failing loses it.

**Following it.** The run starts at the date and time, in UTC, that its `Freeze:` commit names, set and announced in
advance. From then on every commit it lands on `etheram-ibft`'s branch is pushed here at once -- its row in
`WORKBOOK.csv`, its refused attempts in `WORKBOOK-ATTEMPTS.csv`, and `RUN-STATE.md` saying where the run is -- so the
branch `main` of `https://github.com/umbgtt10/ibft_unattended_overhaul_experiment` shows it as it goes, and
`https://github.com/umbgtt10/ibft_unattended_overhaul_experiment/commits/main.atom` follows it in a feed reader. The
dry run before it is pushed the same way, to the branch `dry-run`. The code itself is in `etheram-ibft`, a private
repository: what is public is the record of every commit, not the code.

## Goal

The same two things `etheram-raft` did, in the same order, on a protocol about four times its size:

- **Part A, the rules block.** IBFT holds every one of stern4rust 0.18.0's 38 rules, with today's skips and
  baseline left as they are. Raft's rules block (2026-10-22, `4d74fd0`..`13bc4e7`) is the model, and for
  `records-and-objects`, `record-home` and `injected-dependencies` their rollout across the house tools and Raft on
  2026-10-25 (`9b1df94`..`058b445` in Raft).
  A record -- a type with no behaviour, derives not counting -- keeps public fields and has no accessors; an object
  keeps its fields private, read through its methods; a record lives in the module of what uses it; a trait-typed
  field is filled only from a constructor's parameter; a dev-dependency adds what the dependencies do not give.
- **Part B, the restructuring.** IBFT's Phase 11: the protocol orchestrates its bookkeepers -- the owner's four
  accountants, Faction, Beggar, the certifier and consensus (2026-10-18) -- as plug-ins behind ports, with execution
  beside them as a fifth seam: `execution-types` goes, its port into `node`, its data model into `types`, and `evm`
  becomes a plug-in. Every rule holds with nothing skipped and nothing baselined. Raft's Phase 11 and its
  `P2-ADR-BookkeepersArePlugIns` are the model.
- **Then, outside the experiment and optional**, the further checks and the consumer (*Status*).

**Zero behaviour change**, as in Raft: the work moves, names, types and tests what exists. A bug found on the way is
fixed test-first in a commit of its own; nothing else changes what the protocol does. A bug already recorded in
`OPEN_POINTS.md` is fixed only where a slice moves the code that carries it -- moving it unfixed would build it into
the new seam -- and the same way, test-first in a commit of its own; the rest stay open (*Open points*, below). That
keeps the deliberate behaviour changes few and named, which the experiment's first question needs. New behaviour -- the
certifier's credentials exchange, provisioned keys, join wiring -- is not in this plan: Part B makes room for it.

## Starting state (measured on `main` at `41e68a1`)

**Size.** Lines of Rust and `#[test]` attributes per package (parameterised cases not expanded):

| package | src lines | test lines | tests |
|---|---:|---:|---:|
| `node` | 12,991 | 29,729 | 1,117 |
| `system-tests` | 10,637 | 11,159 | 397 |
| `recovery-control` | 3,155 | 4,596 | 212 |
| `node-infra` | 3,182 | 4,309 | 219 |
| `validation` | 2,475 | 35,937 | 814 |
| `recovery-ibft` | 2,306 | 4,106 | 174 |
| `xtask` | 1,946 | 3,015 | 174 |
| `types` | 1,778 | 3,377 | 170 |
| `evm` | 1,710 | 3,383 | 167 |
| `execution-types` | 491 | 1,197 | 56 |

`node` alone is 3.8 times Raft's (3,410 lines at the start of Raft's rules block), and `system-tests` 3.5 times.

**Files and lines by category**, for predictions 16 to 20. *Productive* is the `.rs` under the seven shipped
crates' `src/`, build scripts included; *test* is the `.rs` under every `tests/` directory and every `.rs` of the
crates that exist only to test -- `validation`, `system-tests` with its build script, and `node-contracts` once it
exists -- which is why it is 29 lines above the size table's sum; *tooling*
is `xtask`; *other* is every other file -- manifests, documents, baselines, configuration. Lines are physical lines.
Files are counted as the guard counts them, `git diff --no-renames --name-status` from the base to the end, so a
moved file is one removed and one added; the commits' own trailers, summed, count a file once in each commit that
touches it.

| category | files | lines |
|---|---:|---:|
| productive | 406 | 25,613 |
| test | 510 | 110,934 |
| tooling | 60 | 4,961 |
| other | 112 | -- |
| **total** | **1,088** | **141,508** |

Raft's Phase 11 is the calibration -- 89 commits from `74b5bf6` to `e35902e`:

| | productive | test | tooling | other | all |
|---|---:|---:|---:|---:|---:|
| lines, start to end | 5,787 to 7,017 (+21%) | 16,153 to 26,306 (+63%) | 2,886 to 6,303 | -- | -- |
| files added, start to end | 90 | 104 | 64 | 25 | 283 |
| files removed | 55 | 16 | 8 | 0 | 79 |
| files changed | 64 | 93 | 31 | 21 | 209 -- 97% of the files that stayed |
| files changed, summed over the commits | 227 | 360 | 102 | 88 | 777 -- 8.7 a commit |

Its architecture work grew the productive lines by 21% and its rules block by 0.2%; the rules block's 79 added
files were tests and tooling. IBFT's rules block should stay near that: `records-and-objects` takes accessors out of
records as it gives objects the methods their readers need, and `record-home` moves records into the files of their
users or into files of their own, which adds files rather than lines.

**Grip and Braintax** over the seven shipped crates, scored with `cargo-grip4rust` 0.9.1 and
`cargo-braintax4rust` 0.13.1 on a `git archive` export of `41e68a1`; the run's rows use the releases recorded at the
freeze, which score the base again (*Reporting*):

| crate | Grip | Braintax | Grip/Braintax |
|---|---:|---:|---:|
| `node` | 296.6 | 2,289.1 | 0.130 |
| `node-infra` | 150.8 | 591.4 | 0.255 |
| `recovery-control` | 62.7 | 367.7 | 0.171 |
| `recovery-ibft` | 49.7 | 247.3 | 0.201 |
| `types` | 35.5 | 440.0 | 0.081 |
| `evm` | 35.2 | 504.0 | 0.070 |
| `execution-types` | 11.1 | 64.1 | 0.173 |
| **total** | **642** | **4,503** | **0.1425** |

Raft's Phase 11 started at 0.1446.

**The stages' durations**, timed at `41e68a1` on the laptop this plan was drafted on, not the run's
(*The environment*): kept as a reference, since the run's laptop times them again at the base and the predictions
use those. One stage at a time, nothing else running, in three rounds -- nothing changed; then one `node` source
touched, what most of Part A's commits change; then one `execution-types` source touched, the crate every other one
builds on, the worst case:

| round | stage 1 | stage 2 | stage 5 | all three |
|---|---:|---:|---:|---:|
| nothing changed, the first run after other builds | 74 s | 60 s | 174 s | 5 min 8 s |
| one `node` source touched | 20 s | 28 s | 112 s | 2 min 40 s |
| one `execution-types` source touched | 30 s | 34 s | 105 s | 2 min 49 s |

Stage 2 regenerated every coverage export after the change, so its half-minute is not stale data. Stage 5 agrees
with the logs of twelve full runs on 14 and 18 October, 1 minute 40 seconds to 2 minutes 10 seconds, and is two
thirds of a commit's validation.

**A flaky test at the base.** The third round's stage 5 failed with nothing changed but a file's time:
`single_node::restart_tests::new_existing_db_restores_height_and_last_hash_across_restart` could not open its sled
database -- "could not acquire lock", `WouldBlock`. The test writes through `SledStorage`, drops it, and opens the
node on the same path at once; sled releases its lock asynchronously, and the test waits for it
(`wait_for_db_unlock`) after its second drop but not after its first. It is the one failure in the 36 stage 5 runs
whose logs are kept -- about 3% a run, and over about 200 commits several reds that are not the change. Part 0
measures the rate and fixes the race; the flake rule covers the rest (*When a stage is red without a cause*).

Every figure in this section is re-measured at the branch base, by the same measurement script that measures the
end (*Reporting*); where the two differ, the base's figure is the one the predictions are judged against.

**Offences, under the 38 rules of stern4rust 0.18.0.** Read-only, on a `git archive` export of `main` at `41e68a1`,
974 files scanned, the two generated ones excluded.

| | offences |
|---|---:|
| as configured -- today's skips and baseline, what 0.18.0 reports on `main` | **475** (8 more baselined) |
| everything on -- no skip, no baseline, the generated code still excluded; where Raft ended Phase 11 | **696** |

As configured, by rule:

| rule | count | where |
|---|---:|---|
| `ordered-impl-functions` | 122 | every package; `--fix` |
| `shared-constants` | 91 | `evm`'s gas and opcode tables (33 each), `types`, the WAL record |
| `records-and-objects` | 51 | `node` 13, `system-tests` 13, `node-infra` 7, `recovery-control` 5, `types` 4, `execution-types` 4, `xtask` 3, `recovery-ibft` 2 |
| `timeless-tests` | 50 | `system-tests` 46 -- the pending-timer store, the wake signal, the manual clock -- and `xtask` 4 |
| `typed-errors` | 49 | `system-tests` 42 -- the storage codec 12, the client-interface codec 6 -- and `xtask` 7 |
| `injected-time` | 39 | `system-tests`: the cluster harness 20, the client and runtime-timer harnesses 5 each |
| `public-or-private` | 30 | `recovery-control` 18, `node` 12 |
| `record-home` | 23 | `system-tests` 12, `types` 5, `node` 2, `evm`, `execution-types`, `node-infra`, `recovery-control` |
| `distinct-tuple-elements` | 11 | `system-tests` 5, `node` 3, `evm`, `execution-types`, `xtask` |
| `injected-dependencies` | 5 | `node` 3, `evm` 2 |
| `implemented-in-one-file` | 2 | `node`, `recovery-control` |
| `test-file-structure` | 1 | `node`: an import out of order; `--fix` |
| `leading-constants` | 1 | `node` |

The three new rules that find something:

- **`records-and-objects`, 51**: 26 objects exposing fields -- `Block`, `Transaction`, `Account` and the receipt in
  `execution-types`, the hard state and two state snapshots in `types`, the protocol's own type with 27 fields, the
  node's diagnostics with 28, recovery's control type --; 14 records hiding theirs -- the wire types of the codec,
  the transport and storage, the node builder's parts --; and 11 records with accessors, six of them configuration.
- **`record-home`, 23**: 13 records named in a crate's public API but declared in another type's file -- the
  cluster configuration's three, the WAL cursor's four field groups, the timer key, the transaction status, the
  interpreter's bytecode step --; 8 wire types used by several files but not `pub`; and 2 records with one user, in
  files of their own.
- **`injected-dependencies`, 5**: the bootstrapper building its coordinator, three times, and `evm`'s two engines,
  whose `Default` builds the value transfer it passes to `new`.

R039, `distinct-dev-dependencies`, finds nothing.

The three rules the configuration skips add, with everything on: `returned-mutations` 94 (`evm` 57, 52 of them in the
interpreter's `tiny_evm_ops.rs`; `node` 20; `validation` 17), `paired-test-file` 99 (`validation` 60,
`system-tests` 39) and `tested-public-api` 20 (`system-tests`). The baseline suppresses eight
`single-implemented-type` offences, seven in `system-tests` and one in `validation`, and holds a ninth entry that
matches nothing.

**What Raft had by the end of Phase 11 and IBFT has not:**

- no `cargo xtask test-inventory`, so no record that a commit lost no test;
- no dry4rust gate and no baseline of today's duplication;
- no dependency gate keeping a bookkeeper out of `node`'s tree;
- no gate on unused dependencies. `cargo-shear` 1.14.0 finds 13, every one real -- removed from a scratch clone,
  the workspace builds and 3,081 tests pass: `primitive-types` in `execution-types` and `node`, `tiny-keccak` in
  `node`, `faction` in `node-infra`, `system-tests` and `validation`, `evm`, `proptest` and `rstest` in
  `system-tests`, `postcard` and `serde` in `validation`, and `eframe` and `rstest` in
  `[workspace.dependencies]` with no member using them;
- no Grip/Braintax table;
- `node` depends on `faction` directly. Recovery is already split -- `recovery-control` and `recovery-ibft`,
  `RECOVERY-AS-BUILT.md` section 9 -- which is the furthest any bookkeeper has got towards a port.

**What IBFT already has that Raft had to make:** a `types` crate, `etheram_ibft_types` -- messages, certificates,
the hard state and the WAL, the validator set -- depending on `etheram-core` and `execution-types` only.

**`execution-types`**, 16 files, holds two things: the chain's data model -- the base types (`types.rs`),
`Account`, `Block`, `Transaction`, `StateRoot`, `HashDigest`, `CanonicalHasher` -- and the execution seam: the
`ExecutionEngine` trait, its results, mutations and receipts. It was cut out of `node` by
`[IMPLEMENTED] EXECUTION-TYPES AND EVM WORKSPACE CRATES` so that `evm` holds no IBFT code, and `evm` still depends
on nothing else of IBFT's. `ExecutionEngine` is a port in all but its place: `evm`'s `ValueTransferEngine` and
`TinyEvmEngine` implement it, `node-infra`'s `NoOpExecutionEngine` is the default, and seven test files fake it.
Every package but `recovery-control` imports the crate -- about 295 files, the base types alone 179 and `block`
152 -- and so do 78 files of `etheram-ibft-embassy`.

## The environment, recorded at the start

The result belongs to a machine, a toolchain and an agent, so all three are recorded at the start -- by the
measurement script where it can -- and again at the end; a difference between the two is reported. What is known
today:

**The machine.** The owner's separate laptop, with 96 GB of RAM, used for nothing else during the run (decision 7).
Its processor, drive, free disk and operating system are recorded by the measurement script at the start.
`etheram-ibft-embassy` is not on it: the consumer's work happens elsewhere, after the experiment. The durations and
the free disk in *Starting state* were measured on another laptop, the one this plan was drafted on; they stay as a
reference, and the run's laptop measures its own at the base.

**The toolchain.** Rust 1.99.1, pinned by `etheram-ibft`'s `rust-toolchain.toml` with `rustfmt`, `clippy` and
`llvm-tools`; just 1.58.0; cargo-llvm-cov 0.9.1; cargo-shear 1.14.0; and every house tool at a release made before
the start: cargo-stern4rust 0.18.0, and cargo-dry4rust, cargo-crap4rust,
cargo-twin4rust, cargo-iceberg4rust, slotgate, cargo-grip4rust and cargo-braintax4rust at versions recorded at the
freeze. No development build is used, and none is upgraded during the run.

**The agent** is what the experiment tests, so it is recorded as closely as the machine:

- the model -- `claude-opus-5-5`, Opus 5.5, today -- and its effort setting;
- the harness -- Claude Code, 2.1.296 today -- its version held for the run with automatic updates off, and its
  permission mode;
- the starting prompt, word for word, in this repository;
- the instructions in effect: every `CLAUDE.md` the run reads -- the user's, `etheram-ibft`'s, this repository's --
  and the agent's memory directory, archived at the start, their SHA-256 here. The archive itself stays outside
  this repository: the memory names private work.

**The owner's use of the machine.** None: the laptop is used for nothing else during the run (decision 7). Any use
all the same is recorded with its times, and the durations measured inside it are flagged in the workbook, since
they share the processor.

## Open points (at the base)

`OPEN_POINTS.md` holds about forty entries. Eleven are taken in -- structural work with no behaviour change, tests
the ladder needs to be what the experiment assumes, and recorded bugs in code a slice moves:

| entry | kind | where |
|---|---|---|
| `types` must be a leaf -- it should not depend on `execution-types` | `refactoring` | Part B step 1, which merges `execution-types` into `types`: closed there |
| The Observer trait is one surface for five concerns | `refactoring` | Part B step 7: split by concern, as its own entry asks, batched with the consumer's integration after the experiment |
| `recovery_state.rs`: peer knowledge is separable | `refactoring` | Part B step 4: peer knowledge into a type of its own, the in-flight slot and peer standing kept together |
| A snapshot apply does consensus's work itself | `refactoring` | Part B step 4: the sixth orthogonality slice |
| `#[must_use]` on the state-deciding bools | `refactoring` | Part B step 4, before the port, which carries it from the start |
| Three conflict-detection tests pass with blocks that are invalid anyway | `tests` | Part 0: before anything moves |
| Retention and snapshot recovery are each tested; their join is not | `tests` | Part B step 4: characterization first |
| REMOVED: the harness helper tests, and the coverage with them | `refactoring`, `tests` | Part A: the build fallback behind a trait, the five tests back |
| The retention tests race a deadline they barely beat | `refactoring` | Part A: the harness's waits fail on stalled progress, not elapsed time |
| A snapshot apply leaves the new height with no timers; a replayed recovery message is reported as a duplicate `PrePrepare`; `requests_blind` cannot count a blind request | `bug-fix` | Part B step 4: the code each sits in moves behind the Beggar port |
| A replayed proposal is judged without the accounts its transactions touch | `bug-fix` | Part B step 6: the buffered replay moves with consensus |

**Left out, and why:**

- **Hardening that needs the owner's decision**, much of it the shelved batch 2: recorded in `etheram-ibft`'s
  `OPEN_POINTS.md`.
- **A future nonce in the pool**: unreachable today, and a precondition of the transaction-gossip workstream.
- **Firmware observability** -- the two decisions that leave no line, a released episode's parity, the severity of
  the line logging a sent catch-up request, re-announcing the recovery target: each changes what a capture says, and
  the sweep after the experiment compares like with like.
- **Design questions**: `NewView` growing as q², execute-and-release during import, CI's credential, the lifecycle
  proposal, restart rehydration, the known feature gaps.
- **Copies on the hot path**: behaviour-neutral, but memory work rather than structure -- taken only where a slice
  already rewrites the line.
- **The gate for a diagnostic counter that reads zero by construction**: firmware counters, and a tool of its own.

## The experiment

Set by the owner on 2026-10-23.

### What it asks

In the context it needs -- a sound architecture and a test ladder that covers every layer (`TEST-LADDER.md`):

1. **Can a migration of about 200 commits run unattended, with no behaviour change and no lost test?** The main
   question.
2. **Does the structure measurably improve?** Measured by Grip and Braintax, and by the tests the overhaul adds.

**The evidence for the first:**

- *Unattended*: the run never stops for the owner. A question it raises is answered on the fly, the answer recorded,
  or left open and delivered with the implementation (*Questions raised*). What can still stop it is its
  environment -- a key, the machine, the network -- and each such stop is counted, with its cause.
- *No behaviour change*: every test -- the base's, the new ones and the changed ones -- green at every commit on the
  stages its change reaches, and all of them at the end; a stage none of whose inputs changed keeps its last green
  result (*Which stages a commit runs*), and every tenth commit runs all three.
  Behaviour changes only in a bug-fix commit, test-first, its test seen failing before the fix. The checks that go
  further -- the base's cluster tests against the final binary, reference traces, a mutation sample, the consumer on
  the rig -- are optional follow-ups after the experiment, and not part of its evidence (*Status*).
- *No lost test*: the inventory's diff at every commit is empty or mapped, and no assertion is relaxed.

**The evidence for the second:**

- the Grip/Braintax ratio over the shipped crates -- `node`, `node-infra`, `types`, `execution-types`, `evm`,
  `recovery-control`, `recovery-ibft`, and the crates Part B adds -- at the first commit and the last, with every
  commit's row between. A crate that comes or goes is in the rows where it exists: `execution-types` until Part B
  merges it into `types`, a move between two crates in scope, which leaves the total as it was;
- the test count at the start and at the end, by package, and how many tests each part added.

### The rules for every commit

1. **Small.**
2. **Reversible.**
3. **Of one kind**, marked in its description -- **a hard rule**: a commit never mixes two kinds, and the guard
   refuses one that does (*The guard enforces one kind*, below):

   | kind | what it is |
   |---|---|
   | `refactoring` | structure changes, behaviour does not: Part B's moves and ports |
   | `bug-fix` | behaviour changes, test-first, the test seen failing |
   | `rule-offence` | fixes offences of the rules it names -- some or all of them; how a rule's offences are sliced across commits is the run's choice |
   | `tests` | adds tests and changes no production code: characterization before code moves |
   | `support` | the plan, documents, scores, baselines, gate tooling |

4. **Counted**: the number of files it removes, adds and changes.
5. **No ignored tests** -- a hard rule: no `#[ignore]` anywhere, and every `test result` line of every stage reads
   `0 ignored`. Neither IBFT nor Raft has one today; the guard keeps it so.
6. **One agent, nothing in parallel** -- a hard rule: one agent does the whole run, one step at a time. No
   subagents, no workflows, no second session; no slice implemented while a guard validates another; no two stages,
   guards or repositories' gates at once; no work split across worktrees.

How each is made concrete:

- **Small.** The threshold is **15 files and 300 lines** -- files added, removed and changed together, lines added
  and removed together -- and **at least 95% of the commits stay within it**. A strong suggestion rather than a
  gate: a commit past it lands when its gates pass, and says why, as a `--fix` across the workspace would. Each
  commit is marked `Threshold: within` or `Threshold: exceeded`. Raft's rules block, the calibration, had a median
  of 7 files and 115 lines, but only 32 of its 39 commits within (82%): the `--fix` that reordered 48 files, and six
  that each brought a port or a failure type to several adapters at once -- the gates' `GateFailure`, 23 files and
  871 lines, the largest of those. Here such a change goes a gate, an adapter, a module at a time.
- **Reversible.** A commit made only when the stages its change reaches pass, on one concern, reverts newest-first to a green
  parent by construction: the revert is the parent's tree. What the experiment can measure beyond that is how many
  commits revert on their own, out of order -- a sample reverted singly at the end, each run through stage 1.
- **One kind.** A trailer: `Kind: refactoring`, `bug-fix`, `rule-offence`, `tests` or `support`, and for a
  `rule-offence` a `Rule:` trailer naming its rules. Tests that come with a change are that change's: Raft's every
  fix brought tests of its own. `tests` means tests alone. A refactoring that an offence's fix needs goes first, in
  its own commit. A bug found while fixing an offence is its own commit.
- **Planned or not.** Every commit says whether the plan foresaw it: `Planned: yes`, or `Planned: no` with a
  `Trigger:` naming what caused it -- a bug found, a gate's unexpected finding, or a raised
  question the run answered on the fly, by its number in `QUESTIONS.md`. Raft planned 31 commits for its Phase 11
  and landed 50; the workbook says how far this run's plan held.
- **One agent, nothing in parallel.** The workbook shows it: no two of its intervals overlap -- implementation and
  validation, commit after commit, and every attempt in the attempts file -- so the wall-clock is their sum and the
  gaps between them. The gates keep the concurrency they are defined with -- cargo's parallel compilation, stage
  5's eight slotgate slots -- because that is the gate, measured alike at the base and the end, not the agent
  working on two things at once. What the experiment times is one agent working serially; prediction 10 assumes it.
- **Counted.** Trailers the guard writes from the staged index -- `git diff --cached --no-renames --name-status` --
  so they cannot be wrong: `Files-Added`, `Files-Removed`, `Files-Changed`, a moved file counting as one removed
  and one added, with `Lines-Added` and `Lines-Removed` beside them and the `Threshold` mark they decide. Raft's 39
  would read 79 added, 0 removed, 254 changed.

### The guard enforces one kind

The guard already measures what deciding a kind takes -- the files a commit touches, the offence count rule by rule,
the inventory's diff -- and refuses a commit that contradicts the kind it declares:

| kind | refused unless |
|---|---|
| `tests` | only `tests/` directories change -- no shipped crate's `src/`, no documents, no tooling; the inventory only grows; no offence count moves |
| `support` | only documents, baselines, configuration and gate tooling (`xtask`) change; no shipped crate's `src/` |
| `rule-offence` | its `Rule:` trailer names the rules, and their counts fall -- not necessarily to zero; no other rule's count rises; no test is lost |
| `refactoring` | every offence count stays as it was -- a count that falls makes the commit a `rule-offence`, or two commits; no assertion of an existing test changes |
| `bug-fix` | a test of the commit is red on the parent: run against the parent's sources in a scratch worktree, it compiles and fails, and passes on the commit; no offence count rises |

`validation/src` and `system-tests/src` count as production for these checks: `system-tests/src` holds the gRPC
runtime beside the scenario support, so a characterization test that needs a new scenario source lands in a commit
of that source's kind.

**What the guard cannot see**, named in the final report as the experiment's limits:

- **behaviour no test pins.** A `refactoring` can still change what no test checks. That is the experiment's premise
  -- a test ladder that covers every layer -- and the optional checks after the experiment a further net under it;
- **a stowaway**: an unrelated change inside a commit of the right kind that moves no count. The threshold bounds
  how much can hide; nothing mechanical goes further.

**What it costs:** the `bug-fix` check builds the parent in a scratch worktree, minutes per fix -- twelve in all of
Raft's Phase 11. The assertion check compares existing tests by name between the parent and the commit, a piece of
`xtask` built and tested in Part 0.

### Which stages a commit runs

Set by the owner on 2026-10-23: not the whole suite blindly, but every stage a change can reach. A stage is skipped
only when none of the files it reads has changed, so its last green result still stands -- what keeps a revert's
parent green and the first question's evidence sound.

| the commit touches | stages |
|---|---|
| a `src/` folder, a manifest, `Cargo.lock`, `rust-toolchain.toml`, a build script, a `.proto`, `xtask/` | 1, 2, 5 |
| `tests/` folders only, in stage 1's packages | 1, 2 |
| `tests/` folders only, in `system-tests` | 1, 2, 5 |
| gate configuration or baselines only -- `stern4rust.toml`, `dry4rust-baseline.json`, `docs/header.txt` and the like | 2 |
| documents only | none -- the inventory's diff still runs |
| every tenth commit, and the end of each part | 1, 2, 5 |

- **Stage 2 runs for a tests-only change too.** stern4rust lints test files, crap4rust scores code against the
  coverage tests give it, twin4rust wants a mirrored test file for each source file, and `cargo-shear` checks the
  dev-dependencies against what tests use.
- **Stage 1 runs for any `.rs` change**, `system-tests`' tests included: its format and clippy checks cover every
  target, and `xtask test-layout` checks every test file.
- **The general rule is that any file a stage reads is that stage's input.** `docs/header.txt` is why "documents
  need nothing" is not quite true.
- **The periodic full run** catches what the input map misses: a test reading a file nobody listed, a flaky test, a
  tool that drifted on the machine.

The guard decides from the staged file list it already reads for the counts, and writes the stages it ran in a
`Stages:` trailer beside the kind. The saving is real but modest -- stage 5 is the long stage, and the commits that
skip it are the ones that touch tests alone, outside `system-tests`.

### When a stage is red without a cause, a slice is stuck, or the run must stop

Set before the start: an unattended run that meets these without a rule makes one up on the way, and a rule made up
on the way is the weakest evidence the experiment can have.

**A red without a cause: one rerun.** When a stage fails on a test the change cannot plausibly reach, or with a
message about the environment -- a lock, a port, a timeout -- the stage is rerun once, unchanged. Green on the
rerun, the red is a flake: the attempt is logged with outcome `flake` and the test's name, and the commit lands on
the green. Red again, it is real, and refused as any other. Never a second rerun, never a test rerun alone, never a
test skipped. A test that flakes twice in the run goes to `QUESTIONS.md`, and its fix -- a race in the test, like
the base's -- is an unplanned `tests` commit when no assertion changes. Part 0 measures the base's rate first:
stage 5 ten times at the base, about twenty minutes at the reference laptop's pace.

**A slice that will not land: parked.** After five refused attempts on one slice, or an hour of implementation
without a green attempt, the slice is parked: its work kept on a local branch, `parked/<n>`, never pushed; the
working tree back at the last commit; an open entry in `QUESTIONS.md` saying what refused it; and the run goes on
with the next slice (*Choosing the next slice*). A parked slice is tried once more, after the other slices of its
step; failing again, it stays parked and is delivered with the result.

**The run stops**, and reports what it has, on the first of:

- seven days of wall-clock, above the predicted range's upper end of five;
- twelve hours without a commit landing;
- less than 10 GB of free disk that clearing build caches cannot restore;
- an environment stop -- a key, the machine, the network -- that has not cleared in an hour.

**A pause with a known end is a wait, not a stop**: a usage limit with its reset time, above all. It is recorded with
its start and end, the run resumes when it ends, and its time is reported apart from the run's active wall-clock
(prediction 10). How the harness resumes after a limit without anyone typing is the dry run's to establish; if it
cannot, each resume is an intervention, and the run is paced to stay inside the limits instead.

A stop is the result's first line, with its cause. The result summary is generated from what exists, and the two
questions are answered for the commits that landed: a partial run is a result, not a failure to report.

**Any message from the owner during the run is an intervention** -- a question, an instruction, a "continue" alike
-- recorded in `QUESTIONS.md`'s interventions table with its time, its words and what followed. The first
question's "unattended" is judged on that table: empty, it holds.

### Choosing the next slice

When more than one slice could come next, the first of these questions that tells them apart decides:

| # | criterion | the slice that goes first |
|---|---|---|
| 1 | `owner-order` | the one the owner's order puts first: Faction, then Beggar, then the certifier, consensus last. A set order is not decided again |
| 2 | `dependency` | the one others build on: the data model before the ports, `execution-types` merged before anything imports its new paths, a gate's findings fixed before the gate |
| 3 | `measurement` | the one whose result changes what later gates measure: renames, moves and duplicates removed before the measuring gates read the tree -- why stern4rust and dry4rust run first in stage 2 |
| 4 | `safety-net` | the characterization tests before the code they cover moves |
| 5 | `risk` | the one most likely to surprise: a bug or a broken assumption found at commit 20 costs less to plan around than one found at 150. Mechanical slices can wait |
| 6 | `no-scaffolding` | the order that needs no temporary code, or the least. Raft's xtask took its typed errors bottom-up, which needed a `From<LaunchFailure> for String` bridge; the bridge got a test, and removing it now removes a test. Top-down, the bridge was never needed. Likewise a file is not touched twice where one pass will do -- `records-and-objects` and `record-home` come last in Part A, so that Part B moves types that already own their state and live beside their users |
| 7 | `threshold` | one that fits 15 files and 300 lines; one that does not is split before it starts, not halfway through |
| 8 | `teaches` | one representative slice, before the pattern is repeated -- slotgate before the other tools, when `cargo-shear` went in: it showed what the rest would cost |
| 9 | `fastest-feedback` | the one whose gates answer soonest, usually the smallest |

Not a criterion: what lowers the offence count fastest. The count is the experiment's output, not its target; chasing
it rewards the cheap mechanical slices over the ones that lower risk.

**Each commit records the choice**, in two trailers beside its kind: `Chosen-By:` names the criterion that decided
it, and `Over:` the slices it was chosen over -- `none` when only one could come next. An unattended run of two
hundred commits is then auditable in its decisions as well as its results, and a criterion that keeps overriding the
plan's order says the order was wrong.

### The per-commit report

The final report holds, for every commit:

- its kind, and for a `rule-offence` the rules it names;
- the criterion it was chosen by, and the slices it was chosen over;
- the files it adds, removes and changes, its lines, and whether it is within the threshold;
- the test count before and after;
- the offence count before and after, as configured and with everything on;
- its Grip/Braintax row;
- how many attempts the guard refused before it landed, and the gate that refused each;
- the time each attempt took;
- the stages it ran, and the time saved against running all three.

The guard writes a record on every attempt, so a refused one is recorded too, straight into this repository's
workbook -- outside `etheram-ibft`, so no `support` commit has to carry a row, and a commit never needs its own hash.
With each commit that lands, the guard commits the record, signed, and pushes it (*Reporting*).
At the end the result summary answers the two questions against the setup summary (*Reporting*).

## Constraints, at every step

What held Raft's 89 commits on `phase-11` together, unchanged:

1. **One concern per commit**, whole on its own, so reverting newest-first leaves the tree green. Signed.
2. **The stages a commit's change reaches, before it lands** (*Which stages a commit runs*), through a guard that
   commits only if every one it runs passes; the tools at their releases, recorded at the freeze -- Raft ran
   stern4rust's development build beside the release only while its rules were unreleased. Never
   `cargo install --path`.
3. **The test inventory** before and after each commit, its diff empty or mapped: no test leaves without a trace.
4. **A Grip/Braintax row per commit**, in the workbook. A dip is explained in
   its row, not a reason to stop.
5. **Bugs fixed test-first, each in a commit of its own.** Nothing else changes behaviour.
6. **No test removed and no assertion relaxed**, ever, during the run. Where a slice seems to need one, the question
   is left open in `QUESTIONS.md` and the slice takes another route or waits. A test that only checked `is_err()`
   may be made stronger; never weaker.
7. **Error types print what was printed**, word for word, panic text included.
8. **Questions and open points raised on the way go to `QUESTIONS.md`**: answered on the fly and recorded, or left
   open and delivered with the implementation. None stops the run (*Questions raised*).
9. **The node's outside stays as it is**: the wire protocol, the gRPC client API, the configuration file and the
   command line -- what the optional black-box check after the experiment would run the base's tests through.

## Before the freeze

What the first two weeks of Raft's run would have told this plan, had it been measured first. Done after the
preconditions in *Status* and before this document is frozen; none of it counts in the run's figures.

**The guard, built and tested first.** Every piece of evidence the run produces passes through it -- the kind
check, the red-on-the-parent check, the unchanged-assertion check, the stage selection, the trailers, the attempts
file -- so it is not built inside the run it judges. It lives in this repository, `guard/`, with tests of its own
against a scratch clone of `etheram-ibft`: a commit of each kind accepted, and each way of breaking a rule refused --
two kinds mixed, a test lost, an assertion relaxed, an `#[ignore]`, a `bug-fix` whose test passes on its parent, a
missing `Chosen-By`. The owner reviews it once, before the dry run. The `xtask` pieces it calls in `etheram-ibft` --
the test inventory, the assertion comparison -- stay Part 0's first commits, and the guard uses its own copies until
they land.

**The dry run, about 25 commits**, on the run's laptop and on branches of its own -- `unattended-overhaul-dry-run`
in `etheram-ibft` and `dry-run` in this repository --, pushed exactly as the run pushes, commit by commit through the
blocklist check (*Reporting*), and never merged. It covers Part 0's tooling, one representative slice of each of
Part A's rule families and the start of one of Part B's seams, under every rule of the run, unattended, and it spans
at least one compaction. It measures what the predictions could only estimate:

- the agent's pace on this code -- implementation minutes a commit, the largest share of prediction 10 -- and the
  stages' durations on the run's laptop;
- its resource use a commit -- model calls, output and cached input tokens, compactions -- prediction 14, and where
  that leaves the account's usage limits over about 200 commits;
- what happens at a limit: whether the harness waits and resumes alone;
- refused attempts and flakes (13), and the disk a commit costs (15);
- the guard on real commits rather than its fixtures, and the push of every commit and its record;
- whether `RUN-STATE.md` (*Reporting*) is enough to resume after a compaction: the dry run has at least one.

Then predictions 10, 13, 14 and 15 are recalibrated from it, each with the dry run's figure beside the estimate it
replaces, and the run starts again from the base (decision 13), so its record is whole from the first commit.

## Part 0: the tooling (about 8 commits)

Before any rule, what makes the rest checkable:

- `cargo xtask test-inventory` and its diff, ported from Raft's;
- a dry4rust gate, with the baseline recorded at the branch's start: from there it fails on what a change adds;
- **a dependency gate, `cargo-shear`** pinned at 1.14.0, run with `--locked` right after stern4rust -- its
  corrections only edit manifests, so they move nothing the later gates measure. The 13 unused dependencies go
  first, in `rule-offence` commits, and then the gate. A dependency `cargo-shear` cannot see used -- a crate linked
  only for what it provides, one declared only to switch on a feature, one used only by generated code reached
  through `include!` -- is listed in `[package.metadata.cargo-shear] ignored`, and the gate refuses an entry
  without its reason beside it, in the same table's `reasons`;
- the coverage baseline per module, as Raft's P0 recorded one;
- **the environment recorded** (*The environment*), and **the base's flake rate**: stage 5 ten times before any
  change; then **the restart test's race fixed** -- a `tests` commit, `wait_for_db_unlock` after its first drop, no
  assertion changed;
- **the three conflict-detection tests made to test what they are named for** (`tests`): their conflicting
  proposals are built on blocks valid but for the conflict, so detection -- not validation -- refuses them. If one
  then fails, it is a bug, fixed test-first;
- the branch's base scored by Grip and Braintax, at the tool versions every later row will use;
- the guard the experiment needs, built and tested before the freeze (*Before the freeze*): it refuses a commit
  whose gates fail, whose kind its diff contradicts, that ignores a test, or that names no criterion it was chosen
  by; runs the stages the staged files reach; writes the kind, count, threshold, stage and planned trailers;
  records every attempt -- kind, counts, tests, offences both ways, the score, the stages, the gate that refused
  it, the time --; and pushes each commit that lands and its record, through the blocklist check (*Reporting*).
  Part 0 adds only the `xtask` commands it calls.

## Part A: the rules block (about 75 commits, 60 to 95)

Raft's block took 37 commits for 120 offences, 49 of them by one `--fix`: about two offences per commit made by
hand. IBFT's 351 by hand are more clustered -- two `evm` tables hold 66 -- so the rate is higher and the count
lower than a straight scale:

| slice | rules | commits |
|---|---|---:|
| `--fix` | `ordered-impl-functions`, `leading-constants`, `test-file-structure` | 1-2 |
| constants given one home | `shared-constants` | 5-8 |
| `pub` or private, each made `pub` with tests of its own | `public-or-private` | 4-6 |
| named values for tuples | `distinct-tuple-elements` | 4-6 |
| one file per implemented type | `implemented-in-one-file` | 3-6 |
| time behind ports, tests off the clock | `injected-time`, `timeless-tests` | 8-12 |
| a failure type per concern | `typed-errors` | 12-15 |
| records with public fields and no accessors; objects with private fields, read through their methods | `records-and-objects` | 6-10 |
| each record in the module of what uses it | `record-home` | 4-6 |
| a trait-typed field filled only from a constructor's parameter | `injected-dependencies` | 1-2 |
| the harness: the cargo-build fallback behind a trait and the five tests removed on 2026-08-22 back; waits that fail on stalled progress rather than elapsed time | -- | 3-5 |
| bugs found on the way | -- | 1-3 |
| living documents in `etheram-ibft`, any baseline re-recorded | -- | 1-2 |

Known before starting, from Raft:

- **Typed errors** follow the house pattern -- `LaunchFailure`, `GateFailure`, `StageFailure` in `etheram-core`'s
  and Raft's `xtask` -- and a `Display` that prints the old text. A failure holding a type without `PartialEq`
  turns `assert_eq!(result, Ok(..))` into `assert_eq!(result.expect(..), ..)`, the same check.
- **Time** comes in through ports declared by the package and implemented by an adapter in `main.rs`'s wiring:
  Raft's `Sleeper`, `MonotonicClock`, `WallClock` and `Pause`. The cluster harness's own time reads stay, recorded in
  the baseline as Raft's seven are (decision 2).
- **Raft's gRPC adapters never stopped their servers** when dropped: the server held the adapter's state and the
  state held the server (`fc9fd86`). IBFT's gRPC transport and client interface are worth the same test early.
- **dry4rust's baseline matches by fingerprint.** Reshaping a recorded duplicate that stays a duplicate fails the
  gate; remove the duplication, or leave the bodies byte-identical.
- **clippy's `result_large_err`** refuses a private function returning tonic's `Status`; box it, or keep the
  conversion in the trait methods.
- **These rules follow their rollout across the house tools and Raft.** In six commits Raft made its getter-only
  structs records and kept its objects' fields private a type or two at a time; it moved its records to their users
  in one commit, and passed its bootstrapper its coordinator in another, 19 files and 672 lines, past the threshold.
  IBFT's bootstrapper has the same shape, and `evm`'s two engines a smaller one.
- **A record loses its accessors, and a test that only read one is kept**, reading the field, its assertion the
  same: removing it needs the owner (constraint 6), so a slice that seems to need it leaves the question open.
- **An object the whole workspace reads** -- `Block`, `Transaction` and `Account` in `execution-types`, `Block` alone
  imported by 152 files -- is what the rules make it (decision 1): with behaviour of its own, its fields go private
  and every reader changes, which is the upper end of `records-and-objects`' range.
- **The protocol's own type and recovery's control type are implemented in 13 and 6 files** -- the two
  `implemented-in-one-file` offences -- and expose 27 and 5 fields. A private field is out of reach of an
  implementation in a sibling file, so for these two the rules go together, and they are the upper end of the range.
- **`record-home` moves a record without a re-export shim**, so each move updates every importer at once: a record
  whose importers alone pass the threshold moves in a commit of its own, which says why.
- **`records-and-objects` and `record-home` come last in Part A**, a module at a time. Done before Part B, the types
  Part B moves already own their state and live beside their users.

## Part B: the restructuring (about 100 commits, 80 to 130)

**The skipped rules, 20 to 30 commits.** `returned-mutations` in `evm` is the largest single piece: the
interpreter's operations take the machine by `&mut`, and 52 of them become functions that return what they change.
`paired-test-file` in `validation` and `system-tests` follows Raft's answer -- each `<scenario>_scenario_tests.rs`
pairs with a `<scenario>_scenario.rs` in `src` that hands its tests the cluster they start from. `tested-public-api`
in `system-tests` is tests for the 20 functions no test calls. The eight baselined `single-implemented-type` offences
are split into files of their own, and the baseline, with its ninth entry that matches nothing, is deleted.

**The seams, 75 to 100 commits** -- execution first, then the bookkeepers, then the observer -- one vertical
slice each, in the owner's order, each slice shaped as Raft's were: characterization tests at the protocol's boundary first, then the port and its data model in `node`, today's
logic as the default in `node-infra`, a contract suite in a `node-contracts` crate that every implementation runs,
and the translation in a plug-in crate that depends on `node`.

1. **Execution, and `execution-types` dissolved.** First, because every later move builds on the data model:
   - **one mechanical commit merges the crate into `types`**: its modules become `types`' own, and every import of
     `execution_types::` is rewritten to its new path, about 295 files -- a path rewrite with no logic in it,
     marked `Threshold: exceeded`. Without re-export shims a module's move updates every importer at once, and
     each of its modules has more than 15 of them; moved one by one, they would be nine commits past the
     threshold, the whole 5% allowance;
   - **`ExecutionEngine` into `node/src/traits`**, beside the other ports -- its importers, 22 files, make it the
     second commit past the threshold, and the last the slice needs;
   - **`evm` a plug-in**, depending on `node` for the port and on `types` for the data model;
     `NoOpExecutionEngine` stays the default in `node-infra`;
   - **a contract suite** in `node-contracts` that both engines and the no-op run: what every engine owes the
     protocol;
   - **a boundary check** beside `cargo xtask recovery-boundary`: `evm` imports only `types`' execution modules.
     Today the crate graph alone keeps IBFT's messages, certificates and WAL out of `evm`'s reach; inside `types`,
     a check has to.

   With `execution-types` gone, `types` depends on `etheram-core` alone -- the leaf `OPEN_POINTS.md` asks it to
   be.

   This holds while execution is IBFT's alone. If a second protocol executes blocks -- consensus as one function
   over two rule sets, the direction of 2026-10-18, puts execution outside it -- the data model and the port need a
   protocol-independent home, in `etheram-core` or a crate of their own; with the port in place, that extraction
   is mechanical.
2. **`node`'s own dependencies.** Three it does not need in production:
   - **`tiny-keccak` and `primitive-types`**: nothing in `node`, source or tests, uses either -- the hashing is
     `execution-types`' and `evm`'s. Part 0 has removed both, with the other unused dependencies, before its
     dependency gate;
   - **`ed25519-dalek`**: only `Ed25519SignatureScheme` uses it, an implementation of `node`'s own
     `SignatureScheme` port that nothing in `node`'s production code builds -- `node-infra`'s
     `SignatureSchemeBuilder` does, and `system-tests`' protocol factory and malicious node. It moves to
     `node-infra`'s implementations, beside `NoOpExecutionEngine`, its test with it; `node`'s certificate tests
     keep it through the dev-dependency on `node-infra` that `node` already has. About 16 files, at the
     threshold's edge.

   What is left is `etheram-core` and `types`, and the two bookkeepers the next steps take out: `faction` and
   `recovery-control`.
3. **Faction**, presence and bootstrapping: `node` stops depending on `faction`; the cluster port and its faction
   plug-in, with a fixed-membership default.
4. **Beggar**, recovery: a port shaped by today's `recovery-control`, which becomes its default. Its trait must not
   fix who drives a transfer -- IBFT's lagging node pulls, Raft's leader pushes -- if the same port is to serve both.
   In order:
   - **characterization**: the join of retention and snapshot recovery, which only its halves have tests for -- a
     peer behind a cluster that has deleted what it needs is refused with the serving floor, exhausts its peers,
     enters snapshot mode, adopts, and lands at the cluster's height, the cluster live throughout;
   - **`#[must_use]`** on `complete_request`, `fail_request`, `refuse_request` and `note_lag_observed`, and on any
     bool whose falsity means "I declined to change state": no production call site discards one today, and the
     port's trait carries the attribute from its first commit;
   - **peer knowledge** -- the statuses each peer reported and the broadcast attempts -- out of `recovery_state.rs`
     into a type of its own; the in-flight slot and peer standing stay together, so a timeout's record and a refusal's stay apart
     beside the request, as `P1-ADR-OnlyARefusalExcludesAPeer` requires;
   - **the snapshot apply, split** as the importer was -- the sixth orthogonality slice: `recovery-ibft` verifies and
     reports the jump, consensus reacts to it (height, membership, vote trackers, round timers), and the control
     layer hears it as an event and re-arms its own check;
   - **three recorded bugs, each test-first**: the timers a snapshot apply leaves unarmed at its new height;
     recovery's message kinds reported as consensus's `PrePrepare`, fixed with one node-level message-kind
     vocabulary for both; and the last-resort request never counted as blind.
5. **The certifier's seam**, whose key is valid at which height: `node`'s `SignatureScheme` port, with
   `Ed25519SignatureScheme` -- in `node-infra` since step 2 -- behind it, deriving every key from the peer id as
   now. Only the seam: the credentials exchange and the key variants are
   later work, and the derived variant stays so that firmware already built from it can be rebuilt exactly.
6. **Consensus**, last: the quorum, the tallies and the decision behind a port, in the shape agreed on 2026-10-18 --
   a step from inputs to outputs, time and randomness outside it. A proposal held in a buffer and replayed is judged
   today under its releaser's context, without the accounts its transactions touch; it is re-queued so the runtime
   builds it its own -- the one recorded bug in the code this step moves, fixed test-first.
7. **The observer, split by concern**: `ProtocolObserver`'s 78 methods into consensus, recovery, retention, peer
   discovery and the output dimensions' refusals, composed where a node needs all of them. Every implementor follows,
   `etheram-ibft-embassy`'s `EmbeddedObserver` with the consumer's retargeting after the experiment, so the break
   is one, batched with the integration as its own entry asks. If a facet cannot be implemented without threading state from another, the split is wrong
   there.
8. **The gates**: `node` depends on `etheram-core` and `types` and nothing else -- no bookkeeper and no crypto
   implementation in its tree --, no plug-in depends on another -- `evm` included --; every rule on, nothing
   skipped or baselined.
9. **An ADR and the living documents** in `etheram-ibft`.

Raft planned 31 commits for its Phase 11 and landed 50: eleven bugs surfaced while code moved. Expect the same here,
more of them.

## Estimate

| part | commits |
|---|---:|
| 0, the tooling | about 8 |
| A, the rules block | about 75 (60-95) |
| B, the restructuring | about 100 (80-130) |
| **the experiment** | **about 180 (150-235)** |

The further checks and the consumer, optional follow-ups after the experiment, are not counted here (*Status*).

The open points taken in add about twenty: the observer's split, Beggar's characterization, split and three fixes,
the harness's coverage and waits, the conflict tests, the replayed proposal. The threshold adds to all of it:
Raft's commits, at 82% within it, would have needed perhaps a tenth more to reach 95%, so read the total as nearer
200. Two of the commits past the threshold are planned: Part B's two moves out of
`execution-types`. The bugs found on the way and the raised questions the run answers with a change are the
unplanned share.

**Wall-clock: about 2 days of continuous running (1-5)**, one agent working serially (rule 6):

| per commit | minutes | basis |
|---|---:|---|
| implementation | about 7 (4-15) | Raft took about 3 a commit -- its rules block 36 commits in 2 hours 3 minutes, its Phase 11 46 in about 3 hours; IBFT is 3.5 to 4 times its size, and a change here reaches more call sites, `Block` alone imported by 152 files |
| validation | about 3 (2.5-5) | the three stages measured at 2 minutes 40 seconds to 2 minutes 49 seconds after a change, 5 minutes 8 seconds for a first run after other builds (*Starting state*), on the reference laptop: the run's laptop times them again at the base, and the dry run recalibrates this row; a commit touching tests alone skips stage 5 |
| refused attempts, flakes, scoring and the record's push | about 2 | Raft's guard refused about one attempt in seven, most early in stage 2; a flake costs a stage's rerun; the inventory, the offence counts and the Grip/Braintax row about a minute, not yet timed; the record's commit and push, through the blocklist check, seconds |
| **a commit** | **about 12 (8-22)** | |

| over the run | hours |
|---|---:|
| about 180 commits at about 12 minutes | about 36 (20-86 over 150-235 commits) |
| fixed work: Part 0's measurements and flake runs; the `bug-fix` checks' scratch builds of the parent; coverage at both ends | about 3 |
| a run of days rather than hours -- the agent's context compacted many times, each costing re-orientation | 10-20% |
| **total** | **about 45 -- 2 days (1-5)** |

With validation measured, the agent's own pace is most of a commit's time and most of the uncertainty: only the run
shows it on code this size.

## Predictions

Draft until the freeze. Each is a measure, its value at the base, and the value predicted at the end, with a range
where one is honest; `SETUP-SUMMARY.md` carries the twenty as its bullets, and `RESULT-SUMMARY.md` answers them in the
same order.

| # | measure | at the base | predicted at the end |
|---|---|---|---|
| 1 | commits on the `etheram-ibft` branch | -- | about 180 (150-235), read as nearer 200: Part 0 about 8, A about 75, B about 100; of them 10-30 unplanned. |
| 2 | commits within 15 files and 300 lines | -- | at least 95%; planned exceptions the two moves out of `execution-types` and any mechanical `--fix` |
| 3 | behaviour | -- | no change outside `bug-fix` commits, as far as the tests see: every test -- the base's, the new ones and the changed ones -- green at every commit on the stages its change reaches, and all of them at the end; every `bug-fix` with a test red on its parent. The further checks are optional follow-ups after the experiment, outside its result |
| 4 | `bug-fix` commits | 4 recorded bugs taken in | the 4, and 10-20 found on the way -- Raft found 11 in Phase 11's 50 architecture commits and 1 in its 37 rules commits |
| 5 | tests | about 3,500 (the inventory at the base sets it); coverage per crate (the script sets it) | none lost; 10-18% added -- Raft's rules block added 16%, many of them tests of accessors these rules do not ask for; coverage not lower, overall or per crate |
| 6 | offences | 475 as configured (8 more baselined), 696 with everything on, under stern4rust 0.18.0's 38 rules; 13 `cargo-shear` findings | zero, all three -- the cluster harness's time reads excepted, held in the baseline (decision 2) |
| 7 | Grip/Braintax | 0.1425 (642 / 4,503) | 0.150 (0.145-0.160), +5% (+2% to +12%) -- Raft's architecture work gained 7%, its rules block nothing in the crates its table scores |
| 8 | seams | `node` depends on 8 crates; `execution-types` stands apart; one observer trait of 78 methods | `node` depends on `etheram-core` and `types` only; `execution-types` gone; the observer split into five concerns |
| 9 | autonomy | -- | the run never stops for the owner; every question it raises in `QUESTIONS.md`, answered on the fly or left open; stops the environment forces counted with their cause -- predicted none; interventions -- any message from the owner during the run -- none |
| 10 | wall-clock | the three stages 2 minutes 40 seconds to 2 minutes 49 seconds after a change, on the reference laptop; the run's laptop times them again at the base | about 2 days, continuous (1-5), one agent working serially, nothing in parallel (rule 6): about 12 minutes a commit (8-22) -- implementation about 7, validation about 3, refused attempts, flakes, scoring and the record's push about 2 -- over about 180 commits, plus about 3 hours of fixed work and 10-20% for a run of days, about 45 hours (*Estimate*); active time, a usage limit's waits reported apart; recalibrated from the dry run. |
| 11 | questions and open points raised and answered on the fly, or implemented | -- | 25-50, each with its answer and, where it caused a change, its commit -- a rough figure: Raft's 37-commit rules block raised about ten |
| 12 | questions and open points raised and left open, delivered as a result of the experiment | -- | 10-25, each with the course its slice took meanwhile and the run's recommendation |
| 13 | refused attempts and flakes | Raft's rules block: about one attempt in seven refused; the base: one flaky failure in 36 stage 5 runs on record, Part 0's ten runs setting the rate | at most one attempt in six refused, most by stage 2's stern4rust and stage 1's clippy; 0-4 flakes after Part 0's fix; 0-3 slices parked, each delivered with its entry |
| 14 | the agent's resource use | Raft's rules block, 37 commits: 345 model calls, 402 thousand output tokens, 164 million cached input tokens read, 2 context compactions -- per commit about 9 calls, 11 thousand output tokens, 4.4 million cached input | per commit 1.5-2.5 times Raft's, IBFT's files being larger, over about 180 commits: about 3,300 model calls (2,400-5,200), 3.8 million output tokens (2.9-5.6 million), 1.6 billion cached input tokens read (1.1-2.4 billion), 10-23 context compactions -- measured from the session transcripts, the cost priced at the end at the list then in force; recalibrated from the dry run. |
| 15 | peak disk | the free disk the measurement script records at the start (42 GB on the reference laptop); `etheram-ibft`'s `target` 17 GB | at most 30 GB more at the peak -- the coverage builds, the `bug-fix` checks' scratch worktrees, each removed after its check -- and never below 10 GB free |
| 16 | files added, base to end | -- | about 355 (260-470): productive about 125 -- failure types, time ports, constants' homes, about 15 records and behaviours given files of their own by `record-home` and `records-and-objects`, the seams' ports, data models and plug-ins, the observer's facets, `execution-types`' 16 moved into `types` --; test about 190 -- about 99 scenario sources for `paired-test-file`, the new productive files' mirrored tests, `node-contracts`, characterization --; tooling about 30; other about 10 -- new crates' manifests, the ADR. Summed over the commits about 370. |
| 17 | files removed, base to end | -- | about 90 (50-200): productive about 45 -- `execution-types`' 16, `ExecutionEngine`, `Ed25519SignatureScheme`, what the plug-ins take out of `node`, the two records `record-home` moves into their one user --; test about 40, the same moves' tests; the rest tooling and other, the stern4rust baseline among them. The upper end is `paired-test-file` renaming test files rather than adding sources beside them. Summed over the commits about 105. |
| 18 | files changed, base to end | -- | about 650 (500-800) of the 1,088 -- Raft changed 97% of the files that stayed, IBFT's larger tree less: the 295 files `execution-types`' merge rewrites, the 122 `ordered-impl-functions` offences, the importers of the records `record-home` moves and the readers of the objects `records-and-objects` closes. Summed over the commits about 1,700 (1,250-2,350), about 9 a commit, Raft's 8.7. |
| 19 | productive lines | 25,613 in 406 files | about 31,000 (28,500-34,500), +21% (+11% to +35%): failure types, time ports and constants 1,000-1,500; the methods objects gain for their readers, net of the accessors records lose, 200-1,000; Part B's ports, defaults, plug-ins and `evm`'s returned mutations 2,500-5,000. Raft's architecture work landed at +21%. |
| 20 | test lines | 110,934 in 510 files | about 122,000 (117,000-130,000), +10% (+5% to +17%): the failure types' tests and those of the methods objects gain, the characterization at each seam, the contract suites, `tested-public-api`'s 20; `paired-test-file` moves scenario setup from test files into sources, both counted as test. |

## Threats to validity

Named before the run, so the result is read with them:

- **The planner is the executor.** The agent that wrote these predictions runs the experiment and knows them; it
  could slice or pace to meet them. Against it: the predictions are frozen before the first commit, the counts are
  written by the guard and the measurement script rather than by the agent, and the result summary is generated
  from them.
- **The instruments are the house's own.** stern4rust, dry4rust, crap4rust, twin4rust, iceberg4rust, Grip and
  Braintax were built in this ecosystem, largely by the same agent, so better by Grip and Braintax is better by
  their own definitions. Against it: measures the house does not own -- clippy, `cargo-shear`, cargo-llvm-cov's
  coverage, lines and tests counted -- are reported beside them.
- **The test ladder is the oracle.** Behaviour no test, trace or cluster run exercises can change unseen (*What the
  guard cannot see*). The optional checks after the experiment would narrow it, outside its result; nothing closes
  it.
- **One of everything.** One codebase, one agent, one run, one machine, and no control -- nobody migrating the same
  code by hand. The result shows that such a run can or cannot succeed here; it is not a rate.
- **Calibrated on a sibling.** The predictions lean on Raft, written by the same hands under the same rules, so they
  may hold better here than they would elsewhere.
- **Measured on another machine.** The stage durations and the free disk in *Starting state* come from the laptop
  this plan was drafted on. The run's laptop, used for nothing else, times its own at the base, and the dry run
  recalibrates the predictions that rest on them; a duration measured during any use of it all the same is flagged,
  not dropped.
- **Rehearsed.** The dry run's slices are done twice, so the run's first commits may go faster than the rest would
  suggest; they are flagged in the workbook and their pace reported apart.
- **The agent can change under the run.** A harness update mid-run would change the subject; its version is held
  and recorded at both ends.

## Reporting

Six documents, in this repository, outside `etheram-ibft`, and the guard beside them:

| file | what it holds |
|---|---|
| `SETUP.md` | this document: the plan, the initial state and the predictions -- frozen at the run's start |
| `SETUP-SUMMARY.md` | twenty numbered bullets, one measure each, its value at the base and the one predicted, frozen with it |
| `WORKBOOK.csv`, `WORKBOOK-ATTEMPTS.csv` | one row per commit, and one per guard attempt, refused ones included |
| `QUESTIONS.md` | every question and open point the run raises: answered on the fly, with the answer, or left open; a security-relevant one only as a placeholder |
| `RUN-STATE.md` | where the run is, for the agent to resume from: overwritten as it goes, committed and pushed with the record |
| `RESULT-SUMMARY.md` | the same twenty, each predicted against actual, with a verdict; the questions raised, answered and open, listed; then the answers to the two questions |

**Four principles keep the comparison clean:**

- **The predictions are fixed before the run.** `SETUP.md` and `SETUP-SUMMARY.md` are frozen in the `Freeze:`
  commit, made before `etheram-ibft`'s branch carries any change; afterwards a prediction is amended only in a dated
  addendum with its reason.
- **Start and end are measured alike.** One script -- the test inventory, offences as configured and with
  everything on, `cargo-shear`'s findings, dry4rust's groups, iceberg's risk, coverage, lines per crate, files
  and lines by category,
  Grip/Braintax per crate -- produces the initial state and, unchanged, the final one. The result summary is
  generated from it and the workbook, not copied by hand.
- **The tools are pinned and listed** -- the releases recorded at the freeze --, the scorers above all: Grip/Braintax
  rows compare only within one version.
- **The record is pushed as it is made.** Each commit that lands on `etheram-ibft`'s branch is pushed, and updates
  this repository's record -- its workbook row, the attempts file, `RUN-STATE.md`, and `QUESTIONS.md` when it
  changed --, which the guard commits, signed, and pushes in turn: the record carries its own timestamps, is public
  as the run makes it, and cannot be rebuilt after the fact. Before every push the guard runs a blocklist check: it
  refuses any added line naming an entry of a private list of names that must not be published, kept outside every
  repository -- copied to the run's laptop before the start, and refreshed when its source changes. A refused push
  is reworded and pushed again before anything goes public.

**Where the run is.** Ten to twenty-three compactions will each leave the agent a summary instead of its context,
and a summary drops detail. `RUN-STATE.md` holds what resuming needs, short enough to read in one go: the part,
step and slice in progress; the last commit and its workbook row; the slices next in line and the criterion that
orders them; the parked slices; the open questions' count; the rules most easily forgotten -- the trailers, the
stages, the flake rule. The agent reads it after every compaction and at the start of every step, and updates it
before every commit; the guard writes the last-commit line itself, and refuses a commit when the file is older than
the previous commit.

**The workbook's columns**, one row per commit: ordinal, hash, part and step, subject; kind, rules, `Chosen-By`,
`Over`, planned, trigger; files added, removed and changed, lines added and removed, threshold; stages run, attempts refused and the
gates that refused them; tests before and after; offences before and after, as configured and with everything on;
Grip, Braintax, ratio; implementation start and finish; validation start and finish.

### Questions raised

A question, an open point, a suspected bug outside the slice, a choice the plan did not foresee -- each goes to
`QUESTIONS.md` as it arises, numbered, with where it arose (commit and step) and what it is. None stops the run.
Each is one of two things:

- **Answered on the fly**, when the answer stays inside the plan's goals and constraints, a later commit can undo it,
  and the house has a clear default. The entry records the answer, the reasoning and what followed; if it was a
  change, an unplanned commit carries it, its `Trigger:` the entry's number.
- **Left open**, when answering it needs what the run may not do: change behaviour beyond a bug fix, remove a test
  or relax an assertion, change another repository beyond the plan, decide a security or policy question, take a
  step that cannot be undone, or choose between options with no house default -- a design question above all, which
  stays in `QUESTIONS.md` and is settled after the experiment. The slice that raised it takes the course that keeps
  behaviour and tests as they are, or waits while the run goes on with the next; the entry records that course and
  the run's recommendation.

**A security-relevant finding is never described in this repository.** It is left open and recorded in
`etheram-ibft`'s private `docs/history/[PROPOSAL] UNATTENDED OVERHAUL FINAL CHECKS AND CONSUMER.md`; `QUESTIONS.md`
holds only a neutral placeholder entry, "a security-relevant finding, recorded privately", and the subject of the
commit that records it, which the workbook carries, says no more.

The open ones are delivered with the implementation, and the owner then takes each -- implement, discuss, discard,
postpone, or integrate into `etheram-ibft`'s `OPEN_POINTS.md` -- and may overturn any answered one. The result
summary counts both kinds.

### What unattended needs

Checked before the start, because a run of two hundred commits that waits for no one is stopped by its environment
far more often than by a question:

- **the signing key stays unlocked for the whole run**: the agent's `max-cache-ttl` above the run's cap, seven
  days -- it is 24 hours today --
  Raft's run stopped once on an expired cache, until the owner unlocked the key;
- **the run's laptop stays awake and on mains**, used for nothing else, with at least 40 GB free at the start for
  every build and the coverage runs (prediction 15);
- **push access and the network for the whole run**: to this repository and to `etheram-ibft`'s branch, since every
  commit pushes both (*Reporting*); a push that cannot go out is an environment stop;
- **the private list in place** for the blocklist check, outside every repository: copied to the run's laptop
  before the start, and refreshed when its source changes;
- **`etheram-ibft-embassy` absent** from the run's laptop: nothing in the run builds or reads it;
- **no reboot and no update**: Ubuntu's unattended upgrades set not to reboot, and nothing -- the system, the
  toolchain, the tools, the harness -- updated during the run;
- **the usage limits known**: the dry run's resource use a commit, times about 200, set against the account's limits
  before the freeze, and the run paced to them if it does not fit;
- **no prompt can stop it**: every command the run needs allowed by the harness's permission mode without asking --
  a permission prompt is a stop -- and the harness's automatic updates off;
- **the released tools installed**, at the versions recorded at the freeze;
- **every decision below answered**, or its default taken, before the freeze.

**The two intervals:** *implementation* starts when the slice is chosen -- a command records the time with
`Chosen-By` and `Over` -- and finishes when the first guard attempt starts; rework after a refused attempt counts as
implementation. *Validation* is each attempt's start to finish: the row carries the one that landed, the attempts
file every one. All times in UTC.

## Decisions for the owner, before the start

All of them before the freeze, none during the run. One left unanswered at the freeze takes its default.

| # | decision | default |
|---|---|---|
| 1 | **Objects and records under `records-and-objects`**, `Block`, `Transaction` and `Account` among them | as the rules make them: a type with behaviour is an object, its fields private and read through its methods; one without is a record with public fields; no behaviour is moved only to change a type's kind |
| 2 | **The harness's time reads**: recorded in the baseline, as Raft's are? | yes: the baseline, which only shrinks |
| 3 | **One branch for Parts A and B, or two?** | one, as Raft used |
| 4 | **`evm`'s interpreter under `returned-mutations`**: rewrite the operations to return what they change, or keep `evm`'s skip? | rewrite: the goal is every rule, nothing skipped |
| 5 | **The certifier in Part B**: the seam only, or left out until the key work? | the seam only |
| 6 | **The branch's name** in `etheram-ibft` | `unattended-overhaul`, the consumer's branch named alike after the experiment |
| 7 | **The run's machine, and its use during the run** | the owner's separate laptop, 96 GB of RAM, used for nothing else; any use recorded with its times |
| 8 | **The agent's effort setting and permission mode** | those Raft's rules block ran at, recorded at the start |
| 9 | **The bookkeepers' ports' shape**: each shaped by today's call sites, or all four in consensus's step shape -- inputs to outputs, time and randomness outside? | today's call sites for Faction, Beggar and the certifier, which keeps the characterization tests meaningful; the step shape for consensus, as agreed on 2026-10-18 |
| 10 | **The observer's composition**: the node takes five observers through its constructor, or one value implementing a composite of the five? | one composite trait over the five with a blanket implementation: an implementor of all five passes one value, the consumer's `EmbeddedObserver` among them |
| 11 | **The consumer's surface**: may Part B change the Rust API `etheram-ibft-embassy` builds on beyond what the seams need -- renames, moved paths? | only what the seams need, every break listed as it is made, for the consumer's retargeting after the experiment |
| 12 | **A suspected bug that is a protocol decision** -- a timeout's value, a quorum rule, an order of effects no document fixes | left open: only a defect against the code's own tests, documents or ADRs is fixed during the run |
| 13 | **After the dry run**: the run starts again from the base, or continues from the dry run's last commit? | from the base: the record is whole from the first commit, the dry run's branches are never merged, and its figures are reported apart |
