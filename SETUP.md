# IBFT unattended overhaul experiment -- setup

`etheram-ibft` under every stern4rust rule, then bookkeepers as plug-ins, run unattended as an experiment.

## Status

**Draft, written from discussions with the owner**, and committed as drafts while it changes. It is
frozen in a commit of its own when the run starts, its subject beginning `Freeze:`: from then on a prediction is
never edited, only amended in a dated addendum that gives its reason.
The six documents of the experiment are listed under *Reporting*.

**Nothing in it starts before both of:**

1. **The owner's current deadline elsewhere** has passed.
2. **A branch of its own** in `etheram-ibft`. `main` stays as it is: it is a tagged base other work depends on,
   and the consumer builds it by `branch = "main"`.

And one the work itself needs: **every house tool released**, and the run on those releases. stern4rust 0.20.1, the
current release, carries 39 rules, and every one is enforced here. Beside it, the releases of
dry4rust, crap4rust, twin4rust, iceberg4rust, slotgate, grip4rust and braintax4rust, their versions recorded at the
freeze.

**Then, before the freeze** (*Before the freeze*): the reference traces recorded at the base, the base measured on
the run's laptop, and Part B's design decisions answered.

**The experiment ends when all its work in `etheram-ibft` is done, with Part C's last commit, and its tests and
Part C's checks are its validation**: every test -- the base's, the new ones and the changed ones -- green at every
commit on the stages its change reaches, and all of them at the end; then the base's cluster tests against the final
binary, the reference traces replayed, and coverage followed function by function (*Part C*). **Mutation testing is
out of scope.** The consumer -- the crate that builds on `etheram-ibft`, retargeted and validated by its own
stages -- comes after the experiment: outside its scope and not its evidence, but part of the overhaul, which merges
only once the consumer has passed. It is planned privately, in `etheram-ibft`'s `docs/history/`.

**It is run as an experiment**: every commit follows the rules in *The experiment* below, and says so.

**The bet**: the run completes Parts 0 to C with no intervention from the owner, no test lost and no assertion
relaxed outside constraint 6's licence, each use of it named, and every test -- the
base's, the new and the changed -- green at every commit. Any one of these failing loses it.

**Following it.** The run starts at the date and time, in UTC, that its `Freeze:` commit names, set and announced in
advance. From then on every commit it lands on `etheram-ibft`'s branch is pushed here at once -- its row in
`WORKBOOK.csv`, its failed attempts in `WORKBOOK-ATTEMPTS.csv`, and `RUN-STATE.md` saying where the run is -- so the
branch `main` of `https://github.com/umbgtt10/ibft_unattended_overhaul_experiment` shows it as it goes, and
`https://github.com/umbgtt10/ibft_unattended_overhaul_experiment/commits/main.atom` follows it in a feed reader. The
code itself is in `etheram-ibft`, a private repository: what is public is the record of every commit, not the code.

## Goal

Three things, in this order:

- **Part A, the rules block.** IBFT holds every one of stern4rust's 39 rules, with today's skips and
  baseline left as they are.
  A record -- a type with no behaviour, derives not counting -- keeps public fields and has no accessors; an object
  keeps its fields private, read through its methods; a record lives in the module of what uses it; a trait-typed
  field is filled only from a constructor's parameter; a dev-dependency adds what the dependencies do not give.
- **Part B, the restructuring.** The protocol orchestrates its bookkeepers -- the owner's four
  accountants, Faction, Beggar, the certifier and consensus (2026-10-18) -- as plug-ins behind ports, with execution
  beside them as a fifth seam: `execution-types` goes, its port into `node`, its data model into `types`, and `evm`
  becomes a plug-in. Every rule holds, nothing skipped and nothing baselined but the cluster harness's time reads
  (decision 2).
- **Part C, the final checks.** What the per-commit checks cannot show: the base's cluster tests against the final
  binary, reference traces recorded at the base and replayed, coverage followed function by function. The experiment
  ends with its last commit. Mutation testing is out of scope.
- **Then, outside the experiment**, the consumer (*Status*).

**Zero behaviour change**: the work moves, names, types and tests what exists. A bug found on the way is
fixed test-first in a commit of its own; nothing else changes what the protocol does. A bug already recorded in
`OPEN_POINTS.md` is fixed only where a slice moves the code that carries it -- moving it unfixed would build it into
the new seam -- and the same way, test-first in a commit of its own; the rest stay open (*Open points*, below). That
keeps the deliberate behaviour changes few and named, which the experiment's first question needs. New behaviour -- the
certifier's credentials exchange, provisioned keys, join wiring -- is not in this plan: Part B makes room for it.

## Starting state (the base, `8a775a2`)

**Size.** Lines of Rust per package, and 3,440 tests in all -- `#[test]` attributes outside strings and comments,
parameterised cases not expanded --, every one of them run by stage 1 (3,042) or stage 5 (398):

| package | src lines | test lines |
|---|---:|---:|
| `node` | 13,026 | 29,456 |
| `system-tests` | 10,640 | 11,178 |
| `recovery-control` | 3,155 | 4,549 |
| `node-infra` | 3,182 | 4,346 |
| `validation` | 2,475 | 36,080 |
| `recovery-ibft` | 2,306 | 4,069 |
| `xtask` | 1,946 | 3,015 |
| `types` | 1,778 | 2,776 |
| `evm` | 1,710 | 3,383 |
| `execution-types` | 491 | 985 |

**Files and lines by category**, for predictions 14 to 18. *Productive* is the `.rs` under the seven shipped
crates' `src/`, build scripts included; *test* is the `.rs` under every `tests/` directory and every `.rs` of the
crates that exist only to test -- `validation`, `system-tests` with its build script, and `node-contracts` once it
exists -- which is why it is 29 lines above the size table's sum; *tooling*
is `xtask`; *other* is every other file -- manifests, documents, baselines, configuration. Lines are physical lines.
Files are counted as each commit's are, `git diff --no-renames --name-status` from the base to the end, so a
moved file is one removed and one added; the commits' own trailers, summed, count a file once in each commit that
touches it.

| category | files | lines |
|---|---:|---:|
| productive | 406 | 25,648 |
| test | 491 | 109,966 |
| tooling | 60 | 4,961 |
| other | 114 | -- |
| **total** | **1,071** | **140,575** |

The rules block should add files rather than lines: `records-and-objects` takes accessors out of records as it gives
objects the methods their readers need, and `record-home` moves records into the files of their users or into files
of their own.

**Grip and Braintax** over the seven shipped crates, scored with `cargo-grip4rust` 0.10.0 and
`cargo-braintax4rust` 0.14.0 on a `git archive` export of the base; the run's rows use the releases recorded at the
freeze, which score the base again (*Reporting*):

| crate | Grip | Braintax | Grip/Braintax |
|---|---:|---:|---:|
| `node` | 297.6 | 2,295.5 | 0.130 |
| `node-infra` | 150.8 | 591.4 | 0.255 |
| `recovery-control` | 62.7 | 367.7 | 0.171 |
| `recovery-ibft` | 49.7 | 247.3 | 0.201 |
| `types` | 35.5 | 440.0 | 0.081 |
| `evm` | 35.2 | 504.0 | 0.070 |
| `execution-types` | 11.1 | 64.1 | 0.173 |
| **total** | **643** | **4,510** | **0.1425** |

**The stages' durations**, on the run's laptop (*The environment*) on 2026-11-02, each round measured twice: one
stage at a time, nothing else building, in three rounds -- nothing changed; then one `node` source touched, what most
of Part A's commits change; then one `execution-types` source touched, the crate every other one builds on, the
worst case:

| round | stage 1 | stage 2 | stage 5 | all three |
|---|---:|---:|---:|---:|
| nothing changed | 4 s | 28-29 s | 43-53 s | 1 min 15 s-1 min 24 s |
| one `node` source touched | 7 s | 28-29 s | 52-55 s | 1 min 28 s-1 min 30 s |
| one `execution-types` source touched | 8 s | 28 s | 49-50 s | 1 min 25 s-1 min 26 s |

Stage 5 is most of it, its rebuild of the node after a change included.

**Stage 5 thirty times**, on the run's laptop on 2026-11-02, one run after another with nothing else running, on
`db7cf6a`, whose `system-tests` the base carries byte for byte: 11,940
test executions, no failure. Stage 5 took 41 to 54 seconds, 47 the median; the slowest test, one of retention's, 33.5
seconds -- within a third of a wait's 120 seconds, so the parallelism stays at 8.

Every figure in this section is re-measured at the branch base, in Part C, the same way as the end (*Part C*,
*Reporting*); where the two differ, the base's figure is the one the predictions are judged against.

**Offences, under the 39 rules of stern4rust 0.20.1.** Read-only, on a `git archive` export of the base, 955 files
scanned, the two generated ones excluded.

| | offences |
|---|---:|
| as configured -- today's skips, reported and baselined | **483**, every one in the baseline |
| everything on -- no skip, no baseline, the generated code still excluded | **696** |

The base reports none: its stern4rust baseline holds all 483, every entry matching an offence, so stage 2 is green
at the base and fails on what a change adds. The baseline only shrinks overall: a `rule-offence` commit
re-records it, dropping what it paid, and may add an offence only when its total still falls -- each one added named
in `QUESTIONS.md`. `--write-baseline` records whatever it finds, an offence a commit adds included, so before a
re-record the run reads the offences reported against the old baseline: each is one the commit adds. It ends holding
the cluster harness's time reads alone (decision 2).
Offences are counted as reported plus baselined, so prediction 6's figures keep their meaning.

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
| `single-implemented-type` | 8 | `system-tests` 7, `validation` 1: a second type beside the one its file is named for |
| `injected-dependencies` | 5 | `node` 3, `evm` 2 |
| `implemented-in-one-file` | 2 | `node`, `recovery-control` |
| `test-file-structure` | 1 | `node`: an import out of order; `--fix` |
| `leading-constants` | 1 | `node` |

What `records-and-objects`, `record-home` and `injected-dependencies` find:

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

`distinct-dev-dependencies` and `behaviour-tests` find nothing.

The three rules the configuration skips add, with everything on: `returned-mutations` 94 (`evm` 57, 52 of them in the
interpreter's `tiny_evm_ops.rs`; `node` 20; `validation` 17), `paired-test-file` 99 (`validation` 60,
`system-tests` 39) and `tested-public-api` 20 (`system-tests`).

**What IBFT does not have yet:**

- no `cargo xtask test-inventory`, so no record that a commit lost no test;
- no dry4rust gate and no baseline of today's duplication;
- no dependency gate keeping a bookkeeper out of `node`'s tree;
- no gate on unused dependencies. `cargo-shear` 1.14.0 finds 13, every one real -- removed from a scratch clone,
  the workspace builds and 3,042 tests pass: `primitive-types` in `execution-types` and `node`, `tiny-keccak` in
  `node`, `faction` in `node-infra`, `system-tests` and `validation`, `evm`, `proptest` and `rstest` in
  `system-tests`, `postcard` and `serde` in `validation`, and `eframe` and `rstest` in
  `[workspace.dependencies]` with no member using them;
- no Grip/Braintax table;
- `node` depends on `faction` directly. Recovery is already split -- `recovery-control` and `recovery-ibft`,
  `RECOVERY-AS-BUILT.md` section 9 -- which is the furthest any bookkeeper has got towards a port.

**What IBFT already has:** a `types` crate, `etheram_ibft_types` -- messages, certificates, the hard state and the
WAL, the validator set -- depending on `etheram-core` and `execution-types` only.

**`execution-types`**, 16 files, holds two things: the chain's data model -- the base types (`types.rs`),
`Account`, `Block`, `Transaction`, `StateRoot`, `HashDigest`, `CanonicalHasher` -- and the execution seam: the
`ExecutionEngine` trait, its results, mutations and receipts. It was cut out of `node` by
`[IMPLEMENTED] EXECUTION-TYPES AND EVM WORKSPACE CRATES` so that `evm` holds no IBFT code, and `evm` still depends
on nothing else of IBFT's. `ExecutionEngine` is a port in all but its place: `evm`'s `ValueTransferEngine` and
`TinyEvmEngine` implement it, `node-infra`'s `NoOpExecutionEngine` is the default, and six test fakes in four
files stand in for it. Every package but `recovery-control` and `xtask` imports the crate -- about 289 files, the
base types alone 179 and `block` 148 -- and so does the consumer.

## The environment, recorded at the start

The result belongs to a machine, a toolchain and an agent, so all three are recorded at the start and again at the
end; a difference between the two is reported. What is known
today:

**The machine.** The owner's separate laptop, used for nothing else during the run (decision 7): a Lenovo ThinkBook
16 G7 ARP, an AMD Ryzen 7 7735HS with 8 cores and 16 threads, 96 GB of RAM (87 GiB usable), a Samsung 1 TB NVMe
drive, 627 GB of it free, and Ubuntu 26.04.1 LTS on Linux 7.0.0-38 -- recorded on 2026-11-02, and again at the start.
Its kernel marked the processor's TSC unstable at boot and keeps time with the HPET, a slower clock to read: every
duration in *Starting state* is under the HPET, and a reboot that gives it the TSC back is recorded, the base's
durations measured again after it. The consumer is not on it: its work happens elsewhere, after the experiment.

**The toolchain.** Rust 1.99.1, pinned by `etheram-ibft`'s `rust-toolchain.toml` with `rustfmt`, `clippy` and
`llvm-tools`; just 1.58.0; cargo-llvm-cov 0.9.1; cargo-shear 1.14.0; and every house tool at a release made before
the start: cargo-stern4rust 0.20.1, cargo-dry4rust 0.5.0, cargo-crap4rust 0.9.0,
cargo-twin4rust 0.7.0, cargo-iceberg4rust 0.4.0, slotgate 0.7.0, cargo-grip4rust 0.10.0 and cargo-braintax4rust
0.14.0, recorded again at the freeze. No development build is used, and none is upgraded during the run.

**The agent** is what the experiment tests, so it is recorded as closely as the machine:

- the model, `claude-opus-5-5`, Opus 5.5, at effort `xhigh`;
- the harness, Claude Code 2.1.314, its version held for the run with automatic updates off, and its permission
  mode, `dontAsk`: a call no rule allows is refused without a prompt, and the settings that allow the rest are
  recorded here word for word;
- the starting prompt, word for word, in this repository;
- the instructions in effect: `etheram-ibft`'s `CLAUDE.md`, the one the run reads -- there is no user's, and this
  repository has none -- its SHA-256 at the base
  `4288deff79e59f27fbb1372567dbc309176373c76fe0f85ee60d804b08ff100b`; and the agent's memory directory, empty at
  the start.

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
- **Logging changes**, four: each changes what the node's logs say, and the consumer's validation after the
  experiment compares like with like.
- **Design questions**: `NewView` growing as q², execute-and-release during import, CI's credential, the lifecycle
  proposal, restart rehydration, the known feature gaps.
- **Copies on the hot path**: behaviour-neutral, but memory work rather than structure -- taken only where a slice
  already rewrites the line.
- **The gate for a diagnostic counter that reads zero by construction**: the consumer's counters, and a tool of its own.

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
  Behaviour changes only in a bug-fix commit, test-first, its test seen failing before the fix. Part C's checks go
  further, inside the experiment: the base's cluster tests against the final binary, the reference traces replayed,
  coverage followed function by function. Mutation testing is out of scope, and the consumer comes after
  the experiment, not part of its evidence (*Status*).
- *No lost test*: the inventory's diff at every commit is empty or mapped -- a test removed under constraint 6's
  licence mapped to its `QUESTIONS.md` entry --, and no assertion relaxed outside the licence.

**The evidence for the second:**

- the Grip/Braintax ratio over the shipped crates -- `node`, `node-infra`, `types`, `execution-types`, `evm`,
  `recovery-control`, `recovery-ibft`, and the crates Part B adds -- at the first commit and the last, with every
  commit's row between. A crate that comes or goes is in the rows where it exists: `execution-types` until Part B
  merges it into `types`, a move between two crates in scope, which leaves the total as it was;
- the test count at the start and at the end, by package, and how many tests each part added.

### The rules for every commit

1. **Small.**
2. **Reversible.**
3. **Of one kind**, marked in its description -- **a hard rule**: a commit never mixes two kinds (*What each kind
   allows*, below):

   | kind | what it is |
   |---|---|
   | `refactoring` | structure changes, behaviour does not: Part B's moves and ports |
   | `bug-fix` | behaviour changes, test-first, the test seen failing |
   | `rule-offence` | fixes offences of the rules it names -- some or all of them; how a rule's offences are sliced across commits is the run's choice |
   | `tests` | adds tests and changes no production code: characterization before code moves |
   | `support` | the plan, documents, scores, baselines, gate tooling |

4. **Counted**: the number of files it removes, adds and changes.
5. **No ignored tests** -- a hard rule: no `#[ignore]` anywhere, and every `test result` line of every stage reads
   `0 ignored`. IBFT has none today, and none is added.
6. **One agent, nothing in parallel** -- a hard rule: one agent does the whole run, one step at a time. No
   subagents, no workflows, no second session; no slice implemented while another is being validated; no two
   stages or repositories' gates at once; no work split across worktrees.

How each is made concrete:

- **Small.** The threshold is **18 files and 360 lines** -- files added, removed and changed together, lines added
  and removed together -- and **at least 95% of the commits stay within it**. A strong suggestion rather than a
  gate: a commit past it lands when its gates pass, and says why, as a `--fix` across the workspace would. Each
  commit is marked `Threshold: within` or `Threshold: exceeded`. A change that brings a port or a failure type to
  several adapters at once goes a gate, an adapter, a module at a time.
- **Reversible.** A commit made only when the stages its change reaches pass, on one concern, reverts newest-first to a green
  parent by construction: the revert is the parent's tree. What the experiment can measure beyond that is how many
  commits revert on their own, out of order -- a sample reverted singly at the end, each run through stage 1.
- **One kind.** A trailer: `Kind: refactoring`, `bug-fix`, `rule-offence`, `tests` or `support`, and for a
  `rule-offence` a `Rule:` trailer naming its rules. Tests that come with a change are that change's. `tests` means
  tests alone. A refactoring that an offence's fix needs goes first, in
  its own commit. A bug found while fixing an offence is its own commit.
- **Planned or not.** Every commit says whether the plan foresaw it: `Planned: yes`, or `Planned: no` with a
  `Trigger:` naming what caused it -- a bug found, a gate's unexpected finding, or a raised
  question the run answered on the fly, by its number in `QUESTIONS.md`. The workbook says how far the plan held.
- **One agent, nothing in parallel.** The workbook shows it: no two of its intervals overlap -- implementation and
  validation, commit after commit, and every attempt in the attempts file -- so the wall-clock is their sum and the
  gaps between them. The gates keep the concurrency they are defined with -- cargo's parallel compilation, stage
  5's eight slotgate slots -- because that is the gate, measured alike at the base and the end, not the agent
  working on two things at once. What the experiment times is one agent working serially; prediction 9 assumes it.
- **Counted.** Trailers written from the staged index -- `git diff --cached --no-renames --name-status` --
  `Files-Added`, `Files-Removed`, `Files-Changed`, a moved file counting as one removed and one added, with
  `Lines-Added` and `Lines-Removed` beside them and the `Threshold` mark they decide; and re-derived from the
  history at the end (*Part C*), so a trailer that is wrong shows as a mismatch.

### What each kind allows

The agent applies these before every commit, with the files the commit touches, the offence count rule by rule --
as configured and with everything on, both taken from the staged tree -- and the inventory's diff, before the
stages run: they take seconds, and a commit that fails its kind wastes every stage run before it. At the end it checks
every commit against them again, from the history (*Part C*). A commit
that contradicts the kind it declares is not made; one found at the end is reported, not corrected:

| kind | allowed only if |
|---|---|
| `tests` | only `tests/` directories change, and `validation/examples/`, where the reference-traces driver lives -- no shipped crate's `src/`, no documents, no tooling; the inventory only grows; no offence count moves |
| `support` | only documents, baselines, configuration and gate tooling (`xtask`) change; no shipped crate's `src/` |
| `rule-offence` | its `Rule:` trailer names the rules, and their counts fall -- not necessarily to zero; the total falls, as configured and with everything on, and another rule's count rises only within that fall, named in `QUESTIONS.md`; no test is lost but under constraint 6's licence |
| `refactoring` | every offence count stays as it was -- a count that falls makes the commit a `rule-offence`, or two commits; an assertion of an existing test changes only to say the same of renamed code, or to be stronger (constraint 6); no default, no builder's choice, no numbering and no log line changes -- such a change is a `bug-fix`, or a question in `QUESTIONS.md` |
| `bug-fix` | a test of the commit is red on the parent: run against the parent's sources in a scratch worktree, it compiles and fails, and passes on the commit; no offence count rises; the one kind whose traces may differ from its parent's, each difference named in its `Traces:` trailer |

`validation/src` and `system-tests/src` count as production for these checks: `system-tests/src` holds the gRPC
runtime beside the scenario support, so a characterization test that needs a new scenario source lands in a commit
of that source's kind.

**What these rules cannot see**, named in the final report as the experiment's limits:

- **behaviour no test pins.** A `refactoring` can still change what no test checks. That is the experiment's premise
  -- a test ladder that covers every layer -- and Part C's checks a further net under it;
- **a stowaway**: an unrelated change inside a commit of the right kind that moves no count. The threshold bounds
  how much can hide; nothing mechanical goes further.

**What it costs:** the `bug-fix` check builds the parent in a scratch worktree, minutes per fix, before the commit
and again at the end. The assertion check compares existing tests by name between the parent and the commit, a piece
of `xtask` built and tested in Part 0.

### Which stages a commit runs

Set by the owner on 2026-10-23: not the whole suite blindly, but every stage a change can reach. A stage is skipped
only when none of the files it reads has changed, so its last green result still stands -- what keeps a revert's
parent green and the first question's evidence sound.

| the commit touches | stages |
|---|---|
| a `src/` folder, a manifest, `Cargo.lock`, `rust-toolchain.toml`, a build script, a `.proto`, `xtask/` | 1, 2, 5 |
| `tests/` folders or `validation/examples/` only, in stage 1's packages | 1, 2 |
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
- **A commit touching the cluster harness runs stage 5 three times.** Every cluster test runs through
  `cluster_process_harness.rs` and its builder, so a change there is the likeliest to make a test flaky rather than
  red: it lands after three green runs, all three in its `Stages:` trailer.

The agent decides from the staged file list it reads for the counts, and writes the stages it ran in a `Stages:`
trailer beside the kind; at the end, every commit runs all three again (*Part C*). The saving is real but modest -- stage 5 is the long stage, and the commits that
skip it are the ones that touch tests alone, outside `system-tests`.

### When a stage is red without a cause, a slice is stuck, or the run must stop

Set before the start: an unattended run that meets these without a rule makes one up on the way, and a rule made up
on the way is the weakest evidence the experiment can have.

**A red without a cause: one rerun.** When a stage fails on a test the change cannot plausibly reach, or with a
message about the environment -- a lock, a port, a timeout -- the stage is rerun once, unchanged. Green on the
rerun, the red is a flake: the attempt is logged with outcome `flake` and the test's name, and the commit lands on
the green. Red again, it is real, and the commit is not made. Never a second rerun, never a test rerun alone, never a
test skipped. A test that flakes twice in the run goes to `QUESTIONS.md`, and its fix -- a race in the test -- is
an unplanned `tests` commit when no assertion changes. The base's rate: no failure in stage 5's thirty runs
(*Starting state*).

**A slice that will not land: parked.** After five failed attempts on one slice, or an hour of implementation
without a green attempt, the slice is parked: its work kept on a local branch, `parked/<n>`, never pushed; the
working tree back at the last commit; an open entry in `QUESTIONS.md` saying what failed it; and the run goes on
with the next slice (*Choosing the next slice*). A parked slice is tried once more, after the other slices of its
step; failing again, it stays parked and is delivered with the result.

**The run stops**, and reports what it has, on the first of:

- seven days of wall-clock, more than twice the predicted range's upper end;
- twelve hours without a commit landing;
- less than 10 GB of free disk that clearing build caches cannot restore;
- an environment stop -- a key, the machine, the network -- that has not cleared in an hour.

**A pause with a known end is a wait, not a stop**: a usage limit with its reset time, above all. It is recorded with
its start and end, the run resumes when it ends, and its time is reported apart from the run's active wall-clock
(prediction 9). Whether the harness resumes after a limit without anyone typing is established before the freeze;
if it cannot, each resume is an intervention, and the run is paced to stay inside the limits instead.

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
| 6 | `no-scaffolding` | the order that needs no temporary code, or the least: typed errors taken bottom-up need a bridge back to the old type, the bridge gets a test, and removing it later removes a test; top-down, no bridge is needed. Likewise a file is not touched twice where one pass will do -- `records-and-objects` and `record-home` come last in Part A, so that Part B moves types that already own their state and live beside their users |
| 7 | `threshold` | one that fits 18 files and 360 lines; one that does not is split before it starts, not halfway through |
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
- its traces against its parent's: identical, or the differences its `bug-fix` names;
- the offence count before and after, as configured and with everything on;
- its Grip/Braintax row;
- how many attempts failed before it landed, and the stage that failed each;
- the time each attempt took;
- the stages it ran, and the time saved against running all three;
- whether the re-derivation at the end agrees with all of the above (*Part C*).

The agent records every attempt, a failed one too, straight into this repository's workbook -- outside
`etheram-ibft`, so no `support` commit has to carry a row, and a commit never needs its own hash. With each commit
that lands, it commits the record, signed, and pushes it (*Reporting*).
At the end the result summary answers the two questions against the setup summary (*Reporting*).

## Constraints, at every step

What holds every commit together:

1. **One concern per commit**, whole on its own, so reverting newest-first leaves the tree green. Signed.
2. **The stages a commit's change reaches, before it lands** (*Which stages a commit runs*): a commit made only if
   every one it runs passes; the tools at their releases, recorded at the freeze -- no development
   build, never `cargo install --path`.
3. **The test inventory** before and after each commit, its diff empty or mapped: no test leaves without a trace.
   And **the reference traces** recorded, in release, and compared with the parent's, the result in a `Traces:`
   trailer: `identical`, or -- on a `bug-fix` alone -- the scenarios and the first lines that differ (*Part C*, item
   2).
4. **A Grip/Braintax row per commit**, in the workbook. A dip is explained in
   its row, not a reason to stop.
5. **Bugs fixed test-first, each in a commit of its own.** Nothing else changes behaviour.
6. **Tests and assertions are kept, and made good.** The run fixes every issue it finds and covers every hole --
   a missing assertion added, a test that cannot fail made to, a weak check strengthened (one that only checked
   `is_err()` names the error) -- and reports each such choice in `QUESTIONS.md`. **With extreme care, and only when
   nothing else lets the work go on, a test may be removed or an assertion relaxed** -- a test a gate refuses once a
   slice lands, a test of a record alone that `behaviour-tests` refuses, above all. The agent does it without asking,
   in the commit whose change forces it. Each removal or relaxation is an answered entry in `QUESTIONS.md` -- the
   test, what forced it, why no move, rewrite or other route keeps it, and what still covers its contract --, named
   in that commit's message and mapped in the inventory's diff, and the result summary lists every one. Every test
   that leaves without a mapping to the same contract, and every assertion that says less, is such a use -- a
   getter's test that `behaviour-tests` refuses included. Paying an offence is never the reason: an offence left
   in the baseline is always another way.
7. **Error types print what was printed**, word for word, panic text included.
8. **Questions and open points raised on the way go to `QUESTIONS.md`**: answered on the fly and recorded, or left
   open and delivered with the implementation. None stops the run (*Questions raised*).
9. **The node's outside stays as it is**: the wire protocol, the gRPC client API, the configuration file and the
   command line -- what Part C's black-box check runs the base's tests through.

## Before the freeze

Done after the preconditions in *Status* and before this document is frozen; none of it counts in the run's
figures.

**The reference traces, recorded at the base** (*Part C*, decision 13): the scenario driver written against the
base's API, in a worktree at `db7cf6a` on a branch of its own, and the corpus recorded from it -- committed beside the driver on
that branch, `reference-traces` at `a1460ca`, and never edited; the run lands the driver in Part 0 and
replays the traces at every commit (*Part C*). Eighteen scenarios, six families of three seeds, in 18 traces of
24,032 lines and 8.0 MB, from a driver of 937 lines in four files. Two recordings are byte for byte the same, and
every trace replays byte for byte at the base. A replay takes about a second in release and 220 seconds in debug; a
release rebuild after a change, about 5.

**The base measured on the run's laptop**: its stages' durations, and stage 5 thirty times, the parallelism kept at
8 only while the slowest test stays within a third of a wait's 120 seconds (*Starting state*).

## Part 0: the tooling (about 20 commits)

Before any rule, what makes the rest checkable:

- **a harness wait that gives up says so, before slotgate's kill**: each wait keeps its budget -- 15 seconds for
  startup, 120 for the others -- and fails naming the wait, its context and target, each node's last height, the
  time elapsed and the last progress seen; slotgate's per-test timeout goes from 120 to 160 seconds, so a wait always
  fails first, with its message, and a kill means a test hung outside every wait. Today both are 120, and a wait that
  runs out is reported as a kill, with nothing in its logs;
- **the single-node harnesses' ports from the slot's range**: `test_config.rs` and `runtime_timer_harness.rs` take
  `allocate_port()` rather than binding port 0 and releasing it, which bypasses slotgate's ranges and competes with
  the outgoing connections of every node on the machine -- and so does the cluster harness's own `next_port`, which
  some 30 test call sites reach;
- `cargo xtask test-inventory` and its diff, and the assertion comparison -- every existing test's assertions
  compared by name between a commit and its parent: the checks the agent runs before every commit, and again over
  the whole history at the end. They follow a move or a rename by themselves, and refuse a commit in which a test
  disappears or a test's assertions say less unless its message names the `QUESTIONS.md` entry that answers for it
  (constraint 6);
- `cargo xtask measure`, printing every figure a commit's row owes -- offences rule by rule both ways, `cargo-shear`'s
  findings, Grip and Braintax --, which by hand take some twenty commands a commit;
- a dry4rust gate, `cargo dry4rust check --exclude-tests --min-lines 5 --max-exact 0 --max-near 0`, against a
  baseline of the base recorded at the same settings: from there it fails on what a change adds. Test functions and
  code under five lines are out of its reach -- at dry4rust's defaults it would refuse nearly every accessor and test
  Part A adds --, production code and test helpers in it. The tool records the baseline whole, so that commit is
  past the threshold, planned;
- **a dependency gate, `cargo-shear`** pinned at 1.14.0, run with `--locked` right after stern4rust -- its
  corrections only edit manifests, so they move nothing the later gates measure. The 13 unused dependencies go
  first, in `rule-offence` commits, and then the gate. A dependency `cargo-shear` cannot see used -- a crate linked
  only for what it provides, one declared only to switch on a feature, one used only by generated code reached
  through `include!` -- is listed in `[package.metadata.cargo-shear] ignored`, and the gate refuses an entry
  without its reason beside it, in the same table's `reasons`;
- the coverage baseline per module;
- **the environment recorded** (*The environment*);
- **the three conflict-detection tests made to test what they are named for** (`tests`): their conflicting
  proposals are built on blocks valid but for the conflict, so detection -- not validation -- refuses them. If one
  then fails, it is a bug, fixed test-first. Each is then run with detection disabled; for one that still passes,
  the run plans a clear fix -- an assertion only detection explains, or the setup that makes detection's effect
  visible: a second honest view change to reach quorum, a snapshot for a restart to restore -- and lands it when it
  judges best, at once or a few commits later, each choice in `QUESTIONS.md`;
- the branch's base scored by Grip and Braintax, at the tool versions every later row will use;
- **the reference-traces driver landed**, last (`tests`): taken from `reference-traces` at `a1460ca` and split into
  six files for stern4rust -- `ScenarioRun` and `Draw` given files of their own --, about 940 lines in one commit
  past the threshold, planned, since its files compile only together. Its first replay matches the corpus byte for
  byte, and from then on every commit replays the traces (*Part C*, item 2);

## Part A: the rules block (about 75 commits, 60 to 95)

The 351 offences fixed by hand are clustered -- two `evm` tables hold 66 -- so a commit often fixes several:

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
| a trait-typed field filled only from a constructor's parameter | `injected-dependencies` | 3-6 |
| the harness: the cargo-build fallback behind a trait and the five tests removed on 2026-08-22 back; waits that fail on stalled progress rather than elapsed time | -- | 3-5 |
| bugs found on the way | -- | 1-3 |
| living documents in `etheram-ibft`, any baseline re-recorded | -- | 1-2 |

Known before starting:

- **Typed errors** follow the house pattern -- `LaunchFailure`, `GateFailure`, `StageFailure` in `etheram-core`'s
  `xtask` -- and a `Display` that prints the old text. A failure holding a type without `PartialEq`
  turns `assert_eq!(result, Ok(..))` into `assert_eq!(result.expect(..), ..)`, the same check.
- **Time** comes in through ports declared by the package -- a sleeper, a monotonic clock, a wall clock, a pause --
  and implemented by an adapter in `main.rs`'s wiring. The cluster harness's own time reads stay, recorded in the
  baseline (decision 2).
- **A gRPC adapter can fail to stop its server when dropped**, when the server holds the adapter's state and the
  state holds the server. IBFT's gRPC transport and client interface are worth that test early.
- **dry4rust's baseline matches by fingerprint.** Reshaping a recorded duplicate that stays a duplicate fails the
  gate; remove the duplication, or leave the bodies byte-identical.
- **clippy's `result_large_err`** refuses a private function returning tonic's `Status`; box it, or keep the
  conversion in the trait methods.
- **`records-and-objects`, `record-home` and `injected-dependencies` go a type or two at a time**: getter-only
  structs made records -- their getters and their constructor gone, each built by literal where it is made, or the
  offence stays --, objects' fields kept private, records moved to their users. Passing the bootstrapper its
  coordinator is the largest single piece, likely past the threshold; `evm`'s two engines need several, their
  `Default` called from some 200 lines, nearly all in `validation/tests` -- split by directory, or given one helper
  for their callers first, in a `refactoring` that moves no count.
- **A record gets no test of its own**: it is tested through what uses it. stern4rust's `behaviour-tests` refuses a
  test that calls nothing of its package but a record's accessors and derives, and twin4rust a mirrored test file
  whose source earns none, so a record a slice extracts or creates brings no test with it.
- **A record loses its accessors, and a test that only read one is kept**, reading the field, its assertion the
  same -- unless that leaves it testing the record alone, which `behaviour-tests` refuses: then it is a blocking
  test, moved to the behaviour that uses the record where it can be, removed under constraint 6's licence where it
  cannot.
- **`cargo stern4rust --fix` rewrites every package it reaches**, not only the one it is pointed at: it is kept for
  the `--fix` slice, and whatever else it touches is restored.
- **An object the whole workspace reads** -- `Block`, `Transaction` and `Account` in `execution-types`, `Block` alone
  imported by 148 files -- is what the rules make it (decision 1): with behaviour of its own, its fields go private
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
`paired-test-file` in `validation` and `system-tests`: each `<scenario>_scenario_tests.rs` pairs with a `<scenario>_scenario.rs` in `src` that hands its tests the cluster they start from. `tested-public-api`
in `system-tests` is tests for the 20 functions no test calls. The eight baselined `single-implemented-type` offences
are split into files of their own, and the baseline is left holding the cluster harness's time reads alone
(decision 2).

**The seams, 75 to 100 commits** -- execution first, then the bookkeepers, then the observer -- one vertical
slice each, in the owner's order, each slice shaped alike: characterization tests at the protocol's boundary first, then the port and its data model in `node`, today's
logic as the default in `node-infra`, a contract suite in a `node-contracts` crate that every implementation runs,
and the translation in a plug-in crate that depends on `node`.

1. **Execution, and `execution-types` dissolved.** First, because every later move builds on the data model:
   - **one mechanical commit merges the crate into `types`**: its modules become `types`' own, and every import of
     `execution_types::` is rewritten to its new path, about 289 files -- a path rewrite with no logic in it,
     marked `Threshold: exceeded`. Without re-export shims a module's move updates every importer at once, and
     seven of its modules have more than 18 of them; moved one by one, they would be seven commits past the
     threshold, most of the 5% allowance;
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
     keep it through the dev-dependency on `node-infra` that `node` already has. About 16 files, within the
     threshold.

   What is left is `etheram-core` and `types`, and the two bookkeepers the next steps take out: `faction` and
   `recovery-control`.
3. **Faction**, presence and bootstrapping: `node` stops depending on `faction`; the cluster port and its faction
   plug-in, with a fixed-membership default.
4. **Beggar**, recovery: a port shaped by today's `recovery-control`, which becomes its default. Its trait must not
   fix who drives a transfer -- IBFT's lagging node pulls, but a protocol whose leader pushes must fit the same port.
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
   later work, and the derived variant stays so that what the consumer already built from it can be rebuilt exactly.
6. **Consensus**, last: the quorum, the tallies and the decision behind a port, in the shape agreed on 2026-10-18 --
   a step from inputs to outputs, time and randomness outside it. A proposal held in a buffer and replayed is judged
   today under its releaser's context, without the accounts its transactions touch; it is re-queued so the runtime
   builds it its own -- the one recorded bug in the code this step moves, fixed test-first.
7. **The observer, split by concern**: `ProtocolObserver`'s methods, one per kind of event, into consensus, recovery, retention, peer
   discovery and the output dimensions' refusals, composed where a node needs all of them by a composite trait each
   implementor implements explicitly, not by a blanket implementation (decision 10). Every implementor follows,
   the consumer's own with its retargeting after the experiment, so the break
   is one, batched with the integration as its own entry asks. If a facet cannot be implemented without threading state from another, the split is wrong
   there.
8. **The gates**: `node` depends on `etheram-core` and `types` and nothing else -- no bookkeeper and no crypto
   implementation in its tree --, no plug-in depends on another -- `evm` included --; every rule on, nothing
   skipped, and nothing baselined but the cluster harness's time reads (decision 2).
9. **An ADR and the living documents** in `etheram-ibft`.

Expect bugs to surface while code moves, and the commits that fix them to outnumber the plan's.

## Part C: the final checks (about 4 commits, 2 to 10)

The per-commit checks show that every step passed every test that existed before it, and that no test left without
a trace; item 5 below makes them evidence rather than the agent's account. They cannot show that what no test pins
stayed the same, nor that the tests still cover what they covered: items 1 to 3 can. The run does all five itself,
item 2 at every commit as well, after Part B's last commit, and the experiment ends with their last commit, when all its work in `etheram-ibft` is
done:

1. **Behaviour, black-box.** `system-tests` built from the base, `8a775a2` -- its tests, its harness, its
   expectations -- drives the final node binary, through the harness's `ETHERAM_NODE_PROCESS_BIN`. Every cluster test
   that spawns a node then checks the new code against the old expectations, no test rewritten. A failure is
   accepted only where a `bug-fix` commit of the run explains it, the expected ones listed before the check runs.
   Constraint 9 keeps the node's outside as it was so that this check can run.
2. **Behaviour, at the protocol tier.** A corpus of seeded, deterministic scenarios at the validation tier --
   election, view change, partition and heal, recovery, retention, snapshot -- and what each produces: every message
   one node queues for another, with the bytes its signature covers, committed blocks' hashes, state roots, persisted
   state.
   - **Recorded before the freeze** (decision 13), in a worktree of its own at `db7cf6a`, by a scenario
     driver written against the base's API on a branch of its own, `reference-traces` at `a1460ca`. It is setup, not
     counted. The corpus never leaves that commit: the run reads an export of it, and its files are never edited --
     the commit's hash says so.
   - **The driver lands on the run's branch** as Part 0's last commit, split for stern4rust; its first replay
     matches the corpus byte for byte.
   - **Kept compiling, and nothing more.** A commit that changes what the driver reads updates it in the same commit,
     to read the same values under their new names -- an accessor for a field, a path for a path. Any other change to
     it is a `tests` commit of its own that says why: what keeps a port from hiding a drift.
   - **Replayed at every commit** (constraint 3), in release: the traces recorded and compared with the parent's --
     identical, or the commit is a `bug-fix` whose `Traces:` trailer names the scenarios and the first lines that
     differ; any other commit whose traces differ is not made. A drift is caught at the commit that makes it, while
     that commit is the one being worked on.
   - **At the end, against the corpus**: the final code's traces differ from the base's only where the run's `bug-fix`
     commits said they would.
3. **Coverage, function by function.** Prediction 5 compares line coverage, overall and per crate. This goes further:
   `cargo llvm-cov` over the shipped crates at the base and at the end; every function covered at the base still
   covered where its code still exists, followed through moves by name and by the test mappings; every function
   uncovered at the end listed with its reason; and the inventory's diff, base to end, with every removed test
   mapped.
4. **Start and end, measured alike.** The base, `8a775a2`, and the last commit, each on a `git archive` export, by
   the same commands both times: the test inventory; offences as configured and with everything on; `cargo-shear`'s
   findings; dry4rust's groups; iceberg's risk; coverage, overall and per crate; lines per crate, and files and lines
   by category; Grip/Braintax per crate; and the machine, the toolchain and the agent (*The environment*). Every
   prediction is judged against these, not against *Starting state*'s figures.
5. **Every commit, re-derived from the history.** Last, after the checks above and their commits, for each commit of
   the branch in order, from the history alone and not from the record: its files and lines and the threshold mark;
   its offence counts both ways; its inventory's diff and its assertion comparison; its traces replayed and compared
   with its parent's; its Grip/Braintax row; its
   declared kind checked against its diff (*What each kind allows*); each `bug-fix`'s test run on the parent, where
   it must fail, and on the commit, where it must pass; all three stages; and every test that left or said less,
   checked against the `QUESTIONS.md` entry its commit names (constraint 6). The report gives each commit's figures
   beside what the record says, and every mismatch as found, never corrected: a commit found red here loses the
   bet's third condition (*Status*).

**Mutation testing is out of scope.** `cargo-mutants` is not run, at the base or at the end, and nothing here stands
in for it: whether the tests still notice a change is not measured.

**Its findings are the run's.** A trace that does not reproduce, a base test that fails against the final binary, a
function no longer covered: each is fixed in a commit of its own on the branch -- a `bug-fix` where behaviour
drifted, `tests` where coverage was lost -- under every rule of the run, and counted with it.

**What stays unverified** is behaviour no scenario, test or cluster run exercises. These checks narrow it; nothing
closes it.

**Its commits:** the black-box run and its list of expected failures, 1-2; the coverage report, 1; and whatever the
checks find -- the driver landed in Part 0, and the corpus stays where it was recorded. About 8 hours of checking on top of them, most of it the three
stages at every commit (*Estimate*).

## Estimate

| part | commits |
|---|---:|
| 0, the tooling | about 20 |
| A, the rules block | about 75 (60-95) |
| B, the restructuring | about 100 (80-130) |
| C, the final checks | about 4 (2-10) |
| **the experiment** | **about 200 (165-260)** |

The consumer, after the experiment, is not counted here (*Status*).

The open points taken in add about twenty: the observer's split, Beggar's characterization, split and three fixes,
the harness's coverage and waits, the conflict tests, the replayed proposal. The threshold adds to all of it --
holding 95% of the commits within it costs perhaps a tenth more commits --, so read the total as nearer 220. Four of
the commits past the threshold are planned: Part 0's dry4rust baseline, which the tool records whole, and its landing
of the trace driver, and Part B's two moves out of `execution-types`. The bugs found on the way and the raised questions the run answers with a change are the
unplanned share.

**Wall-clock: about a day of continuous running (20-65 hours)**, one agent working serially (rule 6):

| per commit | minutes | basis |
|---|---:|---|
| implementation | about 1 (0.5-7) | a change here reaches many call sites, `Block` alone imported by 148 files |
| validation | about 1.5 (1.3-3) | the three stages measured at 1 minute 25 seconds to 1 minute 30 seconds after a change on the run's laptop at the base (*Starting state*); a commit touching tests alone skips stage 5 |
| failed attempts, flakes, the checks, scoring and the record's push | about 1 | failed attempts most often early, in stage 2; a flake costs a stage's rerun; the inventory, the assertion comparison, the offence counts and the Grip/Braintax row; the trace replay about 5 seconds with its release rebuild; the record's commit and push, through the blocklist check, seconds |
| **a commit** | **about 3.5 (3-10)** | |

| over the run | hours |
|---|---:|
| about 200 commits at about 3.5 minutes | about 12 (8-43 over 165-260 commits) |
| fixed work: the environment recorded; the `bug-fix` checks' scratch builds of the parent; coverage at both ends | about 3 |
| Part C's checks: the base's `system-tests` against the final binary, the traces compared with the corpus and replayed at every commit, coverage followed function by function, start and end measured alike, and every commit re-derived from the history -- its three stages at every commit about 5 hours of it | about 8 |
| a run of a day rather than hours -- the agent's context compacted several times, each costing re-orientation | 10-20% |
| **total** | **about 26 -- a day (20-65)** |

With validation measured, the agent's own pace is most of the uncertainty: only the run shows it on code this
size.

## Predictions

Draft until the freeze. Each is a measure, its value at the base, and the value predicted at the end, with a range
where one is honest; `SETUP-SUMMARY.md` carries the eighteen as its bullets, and `RESULT-SUMMARY.md` answers them in the
same order.

| # | measure | at the base | predicted at the end |
|---|---|---|---|
| 1 | commits on the `etheram-ibft` branch | -- | about 200 (165-260), read as nearer 220: Part 0 about 20, A about 75, B about 100, C about 4; of them 10-30 unplanned. |
| 2 | commits within 18 files and 360 lines | -- | at least 95%; planned exceptions the dry4rust baseline, which the tool records whole, the trace driver's landing, the two moves out of `execution-types` and any mechanical `--fix` |
| 3 | behaviour | -- | no change outside `bug-fix` commits, as far as the tests see: every test -- the base's, the new ones and the changed ones -- green at every commit on the stages its change reaches, and all of them at the end; every `bug-fix` with a test red on its parent; and Part C's checks hold: the base's cluster tests pass against the final binary and every reference trace replays byte for byte, each exception a named `bug-fix`. Mutation testing is out of scope |
| 4 | `bug-fix` commits | 4 recorded bugs taken in | the 4, and 10-20 found on the way, the protocol decisions decision 12 fixes among them, each reported at the end -- most of them in Part B, where code moves |
| 5 | tests | 3,440 (the inventory at the base sets it); coverage per crate (Part C measures it) | none lost and no assertion relaxed outside constraint 6's licence, every use of it named; 10-18% added; coverage not lower, overall or per crate, and by Part C every function covered at the base still covered where its code still exists |
| 6 | offences | 483 as configured, all in the baseline, 696 with everything on, under stern4rust 0.20.1's 39 rules; 13 `cargo-shear` findings | zero, all three -- the cluster harness's time reads excepted, held in the baseline (decision 2) |
| 7 | Grip/Braintax | 0.1425 (643 / 4,510) | 0.150 (0.145-0.160), +5% (+2% to +12%) -- the architecture work gaining, the rules block close to nothing |
| 8 | autonomy | -- | the run never stops for the owner; every question it raises in `QUESTIONS.md`, answered on the fly or left open; stops the environment forces counted with their cause -- predicted none; interventions -- any message from the owner during the run -- none |
| 9 | wall-clock | the three stages 1 minute 25 seconds to 1 minute 30 seconds after a change, on the run's laptop at the base | about a day, continuous (20-65 hours), one agent working serially, nothing in parallel (rule 6): about 3.5 minutes a commit (3-10) -- implementation about 1, validation about 1.5, failed attempts, flakes, the checks, scoring and the record's push about 1 -- over about 200 commits, plus about 11 hours of fixed work, Part C's checks 8 of them, and 10-20% for a run of a day, about 26 hours (*Estimate*); active time, a usage limit's waits reported apart. |
| 10 | questions and open points raised and answered on the fly, or implemented | -- | 25-50, each with its answer and, where it caused a change, its commit -- a rough figure |
| 11 | questions and open points raised and left open, delivered as a result of the experiment | -- | 10-25, each with the course its slice took meanwhile and the run's recommendation |
| 12 | failed attempts and flakes | none in 30 stage 5 runs at the base, 11,940 test executions | failed attempts reported, not predicted: every attempt that did not land as validated -- refused by a gate or a kind's rule, or green and then split or reworked --, by cause; 0-4 flakes; 0-3 slices parked, each delivered with its entry. |
| 13 | the agent's resource use | -- | over about 200 commits: about 1,500 model calls (1,050-2,600), 2.2 million output tokens (1.6-3.6 million), 0.75 billion cached input tokens read (0.4-1.5 billion), 3-8 context compactions -- measured from the session transcripts, the cost priced at the end at the list then in force. |
| 14 | files added, base to end | -- | about 360 (265-475): productive about 125 -- failure types, time ports, constants' homes, about 15 records and behaviours given files of their own by `record-home` and `records-and-objects`, the seams' ports, data models and plug-ins, the observer's facets, `execution-types`' 16 moved into `types` --; test about 195 -- about 99 scenario sources for `paired-test-file`, the new productive files' mirrored tests, `node-contracts`, characterization, the trace driver's 6 --; tooling about 30; other about 10 -- new crates' manifests, the ADR. Summed over the commits about 375. The corpus stays on `reference-traces` and counts nowhere. |
| 15 | files removed, base to end | -- | about 90 (50-200): productive about 45 -- `execution-types`' 16, `ExecutionEngine`, `Ed25519SignatureScheme`, what the plug-ins take out of `node`, the two records `record-home` moves into their one user --; test about 40, the same moves' tests; the rest tooling and other. The upper end is `paired-test-file` renaming test files rather than adding sources beside them. Summed over the commits about 105. |
| 16 | files changed, base to end | -- | about 650 (500-800) of the 1,071: the 289 files `execution-types`' merge rewrites, the 122 `ordered-impl-functions` offences, the importers of the records `record-home` moves and the readers of the objects `records-and-objects` closes. Summed over the commits about 1,700 (1,250-2,350), about 9 a commit. |
| 17 | productive lines | 25,648 in 406 files | about 31,000 (28,500-34,500), +21% (+11% to +35%): failure types, time ports and constants 1,000-1,500; the methods objects gain for their readers, net of the accessors records lose, 200-1,000; Part B's ports, defaults, plug-ins and `evm`'s returned mutations 2,500-5,000. |
| 18 | test lines | 109,966 in 491 files | about 122,000 (116,500-130,000), +11% (+6% to +18%): the failure types' tests and those of the methods objects gain, the characterization at each seam, the contract suites, `tested-public-api`'s 20, the trace driver's about 940; `paired-test-file` moves scenario setup from test files into sources, both counted as test. |

## Threats to validity

Named before the run, so the result is read with them:

- **The planner is the executor, and the auditor.** The agent that wrote these predictions runs the experiment,
  knows them, and produces its evidence; it could slice or pace to meet them, or record what it did kindly. Against
  it: the predictions are frozen before the first commit; the evidence is re-derived at the end from the branch's
  history, not taken from the record the agent kept, so a wrong claim made during the run shows as a mismatch; and
  the result summary is generated from that re-derivation. What it cannot rule out is the same agent erring the
  same way twice -- no second party checks either pass.
- **The instruments are the house's own.** stern4rust, dry4rust, crap4rust, twin4rust, iceberg4rust, Grip and
  Braintax were built in this ecosystem, largely by the same agent, so better by Grip and Braintax is better by
  their own definitions. Against it: measures the house does not own -- clippy, `cargo-shear`, cargo-llvm-cov's
  coverage, lines and tests counted -- are reported beside them.
- **The test ladder is the oracle.** Behaviour no test, trace or cluster run exercises can change unseen (*What
  these rules cannot see*). Part C's checks narrow it; nothing closes it. Mutation testing, the one check of whether the
  tests still notice a change, is out of scope, so that question stays unanswered.
- **One of everything.** One codebase, one agent, one run, one machine, and no control -- nobody migrating the same
  code by hand. The result shows that such a run can or cannot succeed here; it is not a rate.
- **Calibrated on the house's own work.** The predictions lean on earlier work by the same hands, under the same
  rules, so they may hold better here than they would elsewhere.
- **The agent can change under the run.** A harness update mid-run would change the subject; its version is held
  and recorded at both ends.

## Reporting

Six documents, in this repository, outside `etheram-ibft`:

| file | what it holds |
|---|---|
| `SETUP.md` | this document: the plan, the initial state and the predictions -- frozen at the run's start |
| `SETUP-SUMMARY.md` | eighteen numbered bullets, one measure each, its value at the base and the one predicted, frozen with it |
| `WORKBOOK.csv`, `WORKBOOK-ATTEMPTS.csv` | one row per commit, and one per validation attempt, failed ones included |
| `QUESTIONS.md` | every question and open point the run raises: answered on the fly, with the answer, or left open; a security-relevant one only as a placeholder |
| `RUN-STATE.md` | where the run is, for the agent to resume from: overwritten as it goes, committed and pushed with the record |
| `RESULT-SUMMARY.md` | the same eighteen, each predicted against actual, with a verdict; each commit as re-derived from the history beside the record, and every mismatch; the questions raised, answered and open, listed; then the answers to the two questions |

**Four principles keep the comparison clean:**

- **The predictions are fixed before the run.** `SETUP.md` and `SETUP-SUMMARY.md` are frozen in the `Freeze:`
  commit, made before `etheram-ibft`'s branch carries any change; afterwards a prediction is amended only in a dated
  addendum with its reason.
- **Start and end are measured alike**, by the same commands, in Part C (item 4): the test inventory, offences as
  configured and with everything on, `cargo-shear`'s findings, dry4rust's groups, iceberg's risk, coverage, lines per
  crate, files and lines by category, Grip/Braintax per crate. The result summary is generated from those and from
  the history's re-derivation (item 5), not copied by hand.
- **The tools are pinned and listed** -- the releases recorded at the freeze --, the scorers above all: Grip/Braintax
  rows compare only within one version.
- **The record is pushed as it is made.** Each commit that lands on `etheram-ibft`'s branch is pushed, and updates
  this repository's record -- its workbook row, the attempts file, `RUN-STATE.md`, and `QUESTIONS.md` when it
  changed --, which the agent commits, signed, and pushes in turn: the record carries its own timestamps, is public
  as the run makes it, and cannot be rebuilt after the fact. Before every push the agent runs a blocklist check over
  the added lines: none may name an entry of a private list of names that must not be published, kept outside every
  repository -- copied to the run's laptop before the start, and refreshed when its source changes. A push the check
  refuses is reworded and pushed again before anything goes public.

**Where the run is.** Three to eight compactions will each leave the agent a summary instead of its context,
and a summary drops detail. `RUN-STATE.md` holds what resuming needs, short enough to read in one go: the part,
step and slice in progress; the last commit and its workbook row; the slices next in line and the criterion that
orders them; the parked slices; the open questions' count; the rules most easily forgotten -- the trailers, the
stages, the flake rule. The agent reads it after every compaction and at the start of every step, and updates it
before every commit, the last-commit line included: no commit is made while the file is older than the previous
commit.

**The workbook's columns**, one row per commit: ordinal, hash, part and step, subject; kind, rules, `Chosen-By`,
`Over`, planned, trigger; files added, removed and changed, lines added and removed, threshold; stages run, traces, attempts failed and the
stages that failed them; tests before and after; offences before and after, as configured and with everything on;
Grip, Braintax, ratio; implementation start and finish; validation start and finish. A row copies its commit's
trailers word for word, `Over` and `Trigger` included; every attempt has its start, its finish and its counts; and a
commit is exactly the tree its landed attempt validated -- nothing added after, documents included. A harness change
that moves a time bound -- a wait's ceiling, a poll's interval, a timeout -- is named in `QUESTIONS.md`, whatever its
kind.

### Questions raised

A question, an open point, a suspected bug outside the slice, a choice the plan did not foresee -- each goes to
`QUESTIONS.md` as it arises, numbered, with where it arose (commit and step) and what it is. None stops the run.
Each is one of two things:

- **Answered on the fly**, the default: every issue found is fixed and every hole covered (constraint 6), when the
  answer stays inside the plan's goals and constraints, a later commit can undo it, and the house has a clear
  default. The entry records the answer, the reasoning and what followed; if it was a
  change, an unplanned commit carries it, its `Trigger:` the entry's number. A suspected bug that is a protocol
  decision is answered this way too, house default or not: fixed test-first, and reported at the end (decision 12).
- **Left open**, when answering it needs what the run may not do: change behaviour beyond a bug fix, remove a test
  or relax an assertion beyond constraint 6's licence, change another repository beyond the plan, decide a security or policy question, take a
  step that cannot be undone, or choose between options with no house default -- a design question above all, which
  stays in `QUESTIONS.md` and is settled after the experiment. The slice that raised it takes the course that keeps
  behaviour and tests as they are, or waits while the run goes on with the next; the entry records that course and
  the run's recommendation. A suspected bug that is a protocol decision is not one of these: decision 12 fixes it.

**A security-relevant finding is never described in this repository.** It is left open and recorded in
a private proposal in `etheram-ibft`'s `docs/history/`; `QUESTIONS.md`
holds only a neutral placeholder entry, "a security-relevant finding, recorded privately", and the subject of the
commit that records it, which the workbook carries, says no more.

The open ones are delivered with the implementation, and the owner then takes each -- implement, discuss, discard,
postpone, or integrate into `etheram-ibft`'s `OPEN_POINTS.md` -- and may overturn any answered one. The result
summary counts both kinds.

### What unattended needs

Checked before the start, because a run of two hundred commits that waits for no one is stopped by its environment
far more often than by a question:

- **the signing key stays unlocked for the whole run**: the agent's `max-cache-ttl` above the run's cap, seven
  days -- it is 8 days, the idle limit the same --: an expired cache stops the run until the owner unlocks the key;
- **the run's laptop stays awake and on mains**, used for nothing else, with at least 40 GB free at the start for
  every build and the coverage runs;
- **the run's scratch on the disk**, not in a memory-backed `/tmp`: every export, scratch worktree and scratch build
  under one directory on the run's laptop's disk, the scratch builds in a target directory of their own, each
  removed when its check is done, and the free space read before every stage 5 and every coverage run;
- **push access and the network for the whole run**: to this repository and to `etheram-ibft`'s branch, since every
  commit pushes both (*Reporting*); a push that cannot go out is an environment stop;
- **the private list in place** for the blocklist check, outside every repository: copied to the run's laptop
  before the start, and refreshed when its source changes;
- **the consumer absent** from the run's laptop: nothing in the run builds or reads it;
- **no reboot and no update**: Ubuntu's unattended upgrades set not to reboot, and nothing -- the system, the
  toolchain, the tools, the harness -- updated during the run;
- **the usage limits known**: the run's expected resource use set against the account's limits before the freeze,
  and the run paced to them if it does not fit;
- **no prompt can stop it**: the permission mode, `dontAsk`, refuses what its rules do not allow instead of asking,
  and the harness's automatic updates are off;
- **the login valid for the whole run**: an expired login stops the session, and its warning comes only three days
  ahead;
- **the released tools installed**, at the versions recorded at the freeze;
- **every decision below answered**, or its default taken, before the freeze.

**The two intervals:** *implementation* starts when the slice is chosen -- a command records the time with
`Chosen-By` and `Over` -- and finishes when the first validation attempt starts; rework after a failed attempt counts
as implementation. *Validation* is each attempt's start to finish: the row carries the one that landed, the attempts
file every one. All times in UTC.

## Decisions for the owner, before the start

All of them before the freeze, none during the run. One left unanswered at the freeze takes its default.

| # | decision | default |
|---|---|---|
| 1 | **Objects and records under `records-and-objects`**, `Block`, `Transaction` and `Account` among them | as the rules make them: a type with behaviour is an object, its fields private and read through its methods; one without is a record with public fields; no behaviour is moved only to change a type's kind |
| 2 | **The harness's time reads**: recorded in the baseline? | yes: the baseline, which only shrinks |
| 3 | **One branch for Parts A to C, or more?** | one |
| 4 | **`evm`'s interpreter under `returned-mutations`**: rewrite the operations to return what they change, or keep `evm`'s skip? | rewrite: the goal is every rule, nothing skipped |
| 5 | **The certifier in Part B**: the seam only, or left out until the key work? | the seam only |
| 6 | **The branch's name** in `etheram-ibft` | `unattended-overhaul`, the consumer's branch named alike after the experiment |
| 7 | **The run's machine, and its use during the run** | the owner's separate laptop, 96 GB of RAM, used for nothing else; any use recorded with its times |
| 8 | **The agent's effort setting and permission mode** | the owner's, recorded at the start |
| 9 | **The bookkeepers' ports' shape**: each shaped by today's call sites, or all four in consensus's step shape -- inputs to outputs, time and randomness outside? | today's call sites for Faction, Beggar and the certifier, which keeps the characterization tests meaningful; the step shape for consensus, as agreed on 2026-10-18 |
| 10 | **The observer's composition**: the node takes five observers through its constructor, or one value implementing a composite of the five? | one composite trait over the five, its supertraits and no body, implemented by an empty `impl` in each implementor's own file -- no blanket implementation: one body for every implementor at once is what `pure-traits` exists to refuse, though its ADR (R014) records that the rule cannot see one, so stern4rust 0.20.1 reports none --: an implementor of all five passes one value, the consumer's observer among them, and the node hands each component only its facet, by trait upcasting |
| 11 | **The consumer's surface**: may Parts A and B change the Rust API the consumer builds on beyond what the rules and the seams need -- renames, moved paths? | only what the rules and the seams need -- Part A's reach public signatures too --, every break listed in `QUESTIONS.md` as it is made, for the consumer's retargeting after the experiment |
| 12 | **A suspected bug that is a protocol decision** -- a timeout's value, a quorum rule, an order of effects no document fixes | fixed along the way, test-first, in a `bug-fix` commit of its own like any other bug -- the test pinning the decision taken, red on the parent -- and reported at the end: the decision, its test and its commit (`RESULT-SUMMARY.md`). A security-relevant one keeps its own rule (*Questions raised*), and the open points the plan leaves out stay out (*Open points*) |
| 13 | **When the reference traces are recorded** -- the scenarios run on the old code, the base, whose outputs the new code must match byte for byte in Part C: before the run, or by the run itself when it reaches Part C? | before the run, at the base, as preparation: not counted in the run's commits or hours, and fixed before any code changes, so the agent that makes the changes does not also choose the scenarios that judge them -- the same reason the predictions are frozen. Recorded from the start, they are in the record for anyone to replay against any commit of the run |
