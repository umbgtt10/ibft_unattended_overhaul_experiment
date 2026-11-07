# IBFT unattended overhaul experiment -- run state

Where the run is, for the agent to resume from. Read after every compaction and at the start of every step;
updated before every commit, the last-commit line included. Overwritten as the run goes, committed and
pushed with the record at every commit.

## Now

- **Last commit:** `d5048cc` Read and build the context snapshot through its accessors in node's tests (row 132)
- **Slices done:** Part A slice 1, time behind ports and tests off the clock -- 13 commits (`4d52581` to
  `44db935`). injected-time holds the cluster harness's 20 alone (decision 2); timeless-tests the wake signal's 6
  alone (Q9, open). Ports, all in `system-tests/src/infra` but `Clock`: `Clock` (timer_scheduler.rs,
  `SystemClock`), `WallClock`, `Sleeper`, `Pause` (async); `PollingWait` (`src/polling_wait.rs`) is every harness
  and test wait; tests compose real time through `real_time_wait()` in `tests/polling_wait_tests.rs`.
  Part A slice 2, a failure type per concern, top-down -- 19 commits, rows 45 to 63 (`85986a6` to `09ad8f1`);
  typed-errors 0 both ways. system-tests: `NodeFailure` and one type per part, `ClusterConfigFailure` last
  (`fa51f03`); `Display` = the old text word for word (constraint 7), `Debug` = that text quoted where
  `{:?}`/`expect` printed it. xtask: `StageFailure` (a record, `reason: Box<GateFailure>` -- clippy's
  `result_large_err`), `GateFailure`, `CargoTool` (`9190601`, handed to stern and twin), `LaunchFailure` (a
  record), `SourceTreeFailure`, `CrapReportFailure`, `RunLogFailure`; xtask's failures derive `Debug`. Lessons: a
  trait whose failure changes touches every implementor -- split before it starts: tests to a type-agnostic form,
  the type, then the trait; a new failure type's test file mirrors its source's folder; two gates reduced to
  variants can turn into dry4rust duplicates -- share the part, hand it in; a recorded dry4rust duplicate
  reshaped alike is re-recorded as Q7 and Q13, fingerprint by fingerprint.
  Part A slice 4, `pub` or private, each made `pub` with tests -- 5 commits, rows 64 to 68 (`48155f6` to
  `24d718d`); public-or-private 0 both ways: `RecoveryControl`'s 18 and `IbftProtocol`'s 12, all `pub(crate)`
  or `pub(super)` for their types' sibling files, each now `pub` with a direct test. Lessons: `tested-public-api`
  matches calls by name and arity, so each needs a test calling it; `ordered-impl-functions` moves a method made
  `pub` up among the `pub` ones, which counts its lines twice; `Action` has no `PartialEq` -- match it by
  pattern; a host still reads the control layer through `process` alone (the boundary gate).
- **Slice waiting:** Part A slice 3, one file per implemented type -- open as Q25 (merge or split, a design
  question with no house default), measured before any commit: a merge carries each handler file's tests
  (`paired-test-file` refuses an orphaned test file; probed on an export), runs to dozens of commits within the
  threshold, and iceberg would refuse the merged files. Both offences stay in the baseline.
  Part A slice 5, named values for tuples -- 7 commits, rows 69 to 75, row 74 the move (`b954ec4` to `abe8d70`);
  distinct-tuple-elements 0 both ways. `BlockCommitments` (`node/src/ibft/block_commitments.rs`, Q26),
  `UtcTimestamp` (`system-tests/src/infra/utc_timestamp.rs`) and xtask's private `CivilDate`, `ReceiptSummary`
  (`execution-types/src/execution/receipt_summary.rs`, Q27), `LocalCompletion` (private in `bootstrapper.rs`),
  `Sha3Window` (`evm/src/tiny_evm/sha3_window.rs`, Q28), `NodeHeight` (`system-tests/src/cluster_wait/`, Q29,
  after `WaitProgress` moved there at `1f0b856`). Lessons: a record used by one file stays private there
  (`record-home`), one used by several gets its own file, named after the type (`named-subject` -- no folder
  prefix); a directory holds 20 files at most (`directory-file-count`) -- room is made by a move of its own, as
  a move counts every line twice; a record whose list is printed keeps the old text with a hand-written `Debug`,
  pinned by a test; run stern before `--write-baseline`, never after a refusal.
  Part A slice 6, constants given one home -- 12 commits, rows 76 to 87 (`8727d28` to `8fd7429`);
  shared-constants 91 to 1 both ways, the 1 `DIAGNOSTIC_FIELDS` (Q31, staying). The rule's corrections (full list:
  `overhaul-scratch/shared-constants.txt`, from `--rule shared-constants --baseline <empty> --offence-threshold
  0`): a public constant nothing else reads goes private; one a single other file reads moves into that file,
  private -- even out of a constants-only file; one several files read moves to a constants file, "imports,
  constants, statics and type aliases only", `<topic>_constants.rs`. Homes made: `evm/src/tiny_evm/gas_constants.rs`
  and `opcode_constants.rs`, `types/src/state/wal_constants.rs` and `hard_state_constants.rs`,
  `types/src/signature_constants.rs`, `node/src/observer/diagnostic_constants.rs`,
  `node/src/ibft/buffers/bounded_buffer/bounded_buffer_constants.rs`,
  `node-infra/src/implementations/pending_pool_constants.rs`, `execution-types/src/common_types/block_constants.rs`;
  the EVM engine's `pub use` shim gone (Q30). Lessons: stern counts a re-export as a reader, and matches readers
  by name -- two "readers" were same-named private constants elsewhere (check an import exists); a constants
  file's own reads do not count (Q31); a constant read by 58 files goes a module at a time behind a one-source
  alias (`pub const X: T = SHARED_X;`, imported `as`), the count paid when the alias goes; grep for grouped
  imports (`block::{Block, X}`) as well as single ones.
  Part A slice 7, a trait-typed field only from a constructor parameter -- 5 commits, rows 88 to 92 (`e4de9df`
  to `cec0c5b`); injected-dependencies 5 to 0 both ways. The EVM engines' `Default` out, every caller composing
  `X::new(Box::new(DefaultValueTransfer))`; the bootstrapper handed its coordinator (Q32):
  `BootstrappingCoordinatorFactory`/`FactionCoordinatorFactory`, `new(config, coordinator, factory)`,
  `inactive(coordinator, factory)`, `BootstrappingLifecycle::discovery_coordinator(..)` then `init(coordinator)`.
  Lessons: stern refuses anything but a parameter assigned to a trait-typed field, an injected factory's call
  included -- split build from install; a delegating trait-impl stub can be a dry4rust near duplicate of
  another, so let the impl hold the body; a planned past-threshold commit says so in `Threshold: exceeded --`.
- **Slice in progress:** Part A slice 8, records and objects, a module at a time -- records-and-objects 5 left
  (`Chosen-By: no-scaffolding`; started 23:51:38Z; full list `overhaul-scratch/records-and-objects.txt`). Three
  kinds. (A) record with private fields -> make them `pub`: all 14 done (`3b243b5`). (B) getter-only -> delete
  the getters and constructor, `pub` fields, built by literal: all 11 done (`EtheramGrpcNodeTimerConfig`,
  `FleetProtocolConfig` at `64b9ab9`, `ConsensusAdmissionConfig`, `ConsensusTimingConfig`, `DiagnosticsConfig`
  at `c72cdf3`, `BootstrappingConfig` at `64f562b`, `NewViewMessageFields` at `2e6d484` -- one getter test removed
  under constraint 6, Q34; `ValidatorSetUpdate` at `ef5f4d9`, 63 literals in 10 files, 336 lines;
  `RecoveryRequest` at `a247cf2`, 33 literals, 270 lines; `RecoveryFacts` at `df27ed7`, 32 literals, 214 lines;
  `PeerRecoveryStatus` at `12b0c09`, 99 places, 271 lines -- the recovery state's 88 through a private test helper,
  `peer_status(height, last_hash)`, a literal, since rustfmt lays a two-field literal argument out over seven
  lines) -- a test that only read a getter keeps its assertion on
  the field unless `behaviour-tests` then refuses it as a record-alone test (constraint 6, QUESTIONS); it did not
  for the three at `c72cdf3`, and refused one of five at `2e6d484` -- removed, mapped to the behaviour test that
  checks its contract (`Maps: <test> -> <test>, QUESTIONS.md entry N`), the other four kept. Lessons there: a getter gone can un-test a same-named method elsewhere
  (`tested-public-api` matches by name: `IbftProtocol::propose_interval_ms` needed a node test); a `Default`
  turned literal can become a dry4rust exact duplicate of a test helper building another record by literal --
  the helper, a fixed value, became a `const`; where neither side has another form -- two three-field `Default`s
  at `64f562b` -- the group is recorded in the dry4rust baseline, named in its message and in Q33 (209 to 210),
  which serves later literals of the shape, (C)'s constructors among them; one with a part to share is shared. (C)
  object exposing `pub` fields -> private fields, a getter for each reader, a constructor for each builder (26,
  not 23 as first counted; xtask's three done at `d09acfa` -- serde builds the report, so getters only; xtask now
  holds no offence; system-tests' `ClusterConfig` and `FleetBlockLimits` at `6c83656` -- the config's tests now
  build each invalid config through `new`; 21 left. Lessons: a getter no test calls raises `tested-public-api`
  with everything on even where it is stood down, and a rule-offence's everything-on total must fall -- test it
  in the same commit; `ordered-impl-functions` wants constructors first, fewest parameters first; a getter can
  surface a dry4rust duplicate that was always there -- `node_ids`/`honest_node_ids`, one now delegating;
  node's `PrunePlan` and `RetainedCounts` at `1121282` -- the counts' one mutation now `with_pending_transactions`,
  a value returned; two `with_` methods and the block's `with_parent_hash` a dry4rust group recorded under Q33
  (210 kept, one paid); node's Grip 285.50 to 295.95, the getters' price; 19 left; next: the smaller node and
  recovery objects -- `InFlightSlot` at `8230a8a` (`is_targeted()`, since `targeted` names a constructor; `new`
  public with a retry count; `request()`; a recorded pair re-keyed, Q35); `RecoveryContext` at `97cd2da` (a
  recorded pair of test recorders re-keyed); `IbftProtocolLimits` at `d8dc9d5`; `TransactionReceipt` at
  `07f708e` (28 test literals now `new(..)`, the trace driver reading the getters, two dry4rust groups paid, 211 to
  209; `tested-public-api` counts calls per package -- the declaring package's own tests must call each getter);
  node-infra's three in-memory states at `618894b` (`open_queue`/`open_inbox`, `pop_*`, `push_response`, eight
  tests; three dry4rust groups paid, one re-keyed, 209 to 206); `IbftProtocolConfig` at `a2cfb60` (handed over as
  `into_parts()` -> `IbftProtocolConfigParts`, the dependencies' shape); `Account` at `a449311` (four commits,
  rows 111-114); `Transaction` at `e39ac2c` (rows 115-118, the bug-fix among them): its getters (six `const fn` -- the pool's `const fn of` reads them --
  and `data() -> &[u8]`) and the production readers but the hard state's at `ac456b9` (`refactoring`; `from()`
  shadows `From::from` in a path, so `.map(Into::into)`; evm's two all-gas results folded into `gas_spent_result`,
  a dry4rust pair paid, 205 to 204); the bug-fix at `e1232ca` (Q36: the hard state dropped transaction data --
  both encoders write it, the cursor reads it through a private `counted_bytes`, the layout bytes 0x03 to 0x05
  and 0x04 to 0x06; one test renamed, `Maps:`; checked red on the parent in `wt-parent`); system-tests' sources and tests and the trace driver at
  `cbbd2f6` (`refactoring`; the wire pair of `From`s paid, 204 to 203); the fields private with node's,
  node-infra's, types' and validation's tests at `e39ac2c` (split from `cbbd2f6` through `git stash --keep-index`
  so the stages ran on exactly the commit's tree); `RecoveryStateSnapshot` at `525d620` (an eight-argument
  `new` under `#[allow(clippy::too_many_arguments)]`, the house's way -- five constructors carry it; stern's
  `tested-public-api` misses a call written inside `assert_eq!`, so build the expected value in the arrange);
  `WorldStateSnapshot` at `3afac18` (two commits: `new`, getters and every production reader at `439551b` -- the
  in-memory storage copies the adopted maps, an `into_parts` tuple having tripped clippy's `type_complexity`; a
  dry4rust pair re-keyed, Q35 -- then the fields private with the remaining tests, split through
  `git stash --keep-index`); `IbftHardState` and `IbftVolatileSnapshot` together at `4612b79` (six commits, rows
  122-127, they share five test files): a getter and a `with_` per field, no constructor (the tests set one to
  four fields over `Default`) -- `1d27ff1` every production reader and the types crate's tests (`refactoring`, past
  the threshold, 859 lines -- tested-public-api wants the crate's own tests calling all 36 methods in the same
  commit), node's tests `278fb05`, validation's `a7f097b`, `8d4b1cf`, `8843e5c`, system-tests' with the fields
  private `4612b79` (the last three split out of one 505-line commit -- a seventh past the threshold would break
  the 95%); a test-only commit runs stages 1 and 2;
  5 left: `Block` (execution-types; ~148 importers, tests mutate its fields), `NodeDiagnostics` (Q37, open: a
  constructor of 28 -- left in the baseline), `ContextSnapshot` (node; in progress: 13 getters, a `with_` per
  set field -- `as_variant` for the variant, `with_variant` being the constructor -- and `with_account`,
  `with_block`, `with_pending_transaction` for the 84 in-place inserts and pushes in tests, all at `b440217` with
  the context's own tests; `consensus.rs` at `3867552`; the other ten production readers and the builder at
  `2f65d20` (three dry4rust groups re-keyed, members unchanged, Q35 -- re-record with `--path .` from the start
  folder: an absolute path rewrites every closure's name; the baseline holds 203 groups, not the 200 a failing check
  reports suppressed); node's `snapshot_context_builder_tests.rs` at `3c58db8` (`replace_all` per field, then
  `grep -n "^ *\.blocks\b\|..."` for chains rustfmt split across lines -- patterns ending in `$` are refused);
  node's other tests with validation's `scenario_helpers.rs` (its dry4rust twin) at `d5048cc`; left, listed with the
  fields private in `overhaul-scratch/context-readers-2.txt`: 168 reads in 16 validation test files -- next
  consensus/ and messaging/ (9 files, 63), then prop_tests/, protocol/, recovery/ (7 files, 105), then the fields
  private (`rule-offence`, 65 to 64 configured, 276 to 275 everything on)), `IbftProtocol` (Q25) (node),
  `RecoveryControl` (Q25) (recovery). SETUP's 6-10 for the slice is its commit range, not what is left
  (read wrongly here until row 114): the stern baseline is to end holding the cluster harness's time reads alone,
  so every object closes, the upper end of the range being the protocol's and control's types (Q25) and
  `Block`/`Transaction`/`Account` ("every reader changes"); decision 1:
  no behaviour moved only to change a type's kind -- so `NodeDiagnostics` (27 fields and one `const fn`) cannot be
  made a record by moving its function out. How `Account` went: its accessors and every production reader
  plus execution-types', evm's and types' tests at `2e93617` (`refactoring`, fields still `pub`); the readers left
  listed by compiling with the fields private (`cargo check --workspace --all-targets --keep-going
  --message-format short > overhaul-scratch/account-readers.txt`, then restoring `pub` -- struct literals'
  E0451 surface only once the field reads' E0616 are gone, so grep for `Type {` too); node's, node-infra's and
  system-tests' tests and the trace driver at `68620c3`; validation's cluster tests at `31dea7c`; the single-node
  tests and the fields private at `a449311` (`rule-offence`, 11 files with `account.rs` and the stern baseline --
  all of validation in one would have been 19, past the threshold); `RecoveryStateSnapshot` has eight fields, past clippy's
  seven-argument limit for a constructor -- later; execution-types' `Account`, `Block`, `Transaction` read in
  some 70 files -- getters added first, readers moved a directory at a time in `refactoring`s, fields private last):
  `Account`, `Block` (~148 importers), `Transaction`, `TransactionReceipt` (execution-types),
  `IbftProtocolConfig`, `IbftProtocolLimits`,
  `IbftProtocol` (with Q25), `NodeDiagnostics`, `ContextSnapshot`, `PrunePlan`, `RetainedCounts` (node),
  `InMemory{ClientInterface,Timer,Transport}State` (node-infra), `InFlightSlot`, `RecoveryContext`,
  `RecoveryStateSnapshot`, `RecoveryControl` (with Q25) (recovery), `IbftHardState`, `IbftVolatileSnapshot`,
  `WorldStateSnapshot` (types). The `c75-pending` scratch folder can go.
- **Commits:** 132 on the branch; 6 past the threshold (4 planned, + `97225b7` for the dry4rust re-record,
  `1d27ff1` for the two states' 36 accessors); bug fixes
  2 (`eba863f`, `e1232ca`); the 130th (`2f65d20`) ran all three stages, as every commit has; the 140th next
- **Tests:** 3,440 at the base, 3,661 now (stage 5 465); removed under constraint 6: 1 (`2e6d484`, Q34)
- **Offences:** 65 as configured, 276 everything on, shear 0; Grip 734.29 / Braintax 4,676.19 (0.1570;
  node 325.06 / 2,354.12 at `b440217`;
  types 72.32 / 518.56 and node 300.17 at `1d27ff1`; types 39.26 / 453.13 at `439551b`;
  recovery/control 74.95 / 383.47 at `525d620`;
  types 35.46 / 447.16 at `e1232ca`; execution-types 24.37 / 75.83 and evm 34.29 / 498.07 at `ac456b9`;
  execution-types 17.72 / 66.79 at `2e93617`;
  node 301.27 / 2,300.34 at `a2cfb60`;
  node-infra 151.68 / 596.73 at `618894b`;
  execution-types 14.87 / 62.11 at `07f708e`;
  node 297.47 / 2,294.30 at `d8dc9d5`; recovery/ibft 51.62 / 249.41 at `97cd2da`;
  recovery/control 66.97 / 370.73 at `8230a8a`, up from 61.27 / 363.83 with seven accessors; node
  295.95 / 2,288.56 at `1121282`, up from 285.50 / 2,268.11 with eleven getters and setters; xtask and system-tests are
  not scored;
  recovery/control 61.27 / 363.83 at `12b0c09`; recovery/ibft 48.77 / 245.23 and node 285.50 / 2,268.11 at `df27ed7`; recovery/control 62.22 / 365.78 at `a247cf2`; node 285.44 / 2,268.11 at `ef5f4d9`, 286.39 / 2,270.20 at `2e6d484`, 290.57 / 2,277.38 at `64f562b`, 292.63 / 2,284.79 at `c72cdf3`; before it 643.14 / 4,502.39, 0.1428;
  execution-types 64.14 to 56.14 at `069fd4c`, evm 503.97 to 500.25 at `1618598`, node 2295.50 to 2299.52 at
  `cec0c5b` with the factory seam); dry4rust baseline 205 groups (one re-formed at `68620c3`, Q35; two paid at `07f708e`, three paid and one re-keyed at `618894b`, two paid at
  `2e93617`), all matching (re-recorded at `cec0c5b`, Q32;
  one group added at `64f562b`, Q33; one added and one paid at `1121282`; one re-keyed and one added at `8230a8a`,
  Q35, each copy kept in scratch as `dry-baseline-before-cN.json`; one re-keyed at `97cd2da`)
- **Questions:** 34 answered, 3 open (9, 25, 37). **Interventions:** 0. **Waits:** 0.

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
  `PartitionFailure` (`c42e574`), `ClientInterfaceFailure` (`d95a05c`),
  `ClientInterfaceCodecFailure` (`1f0a989`), `SledStorageFailure` (`1f59d35`),
  `StorageCodecFailure` (`52d7f53`), `ProtocolAssemblyFailure` (`dd886cc`),
  `ClusterConfigFailure` (`fa51f03`); xtask's `StageFailure` (`ba9d113`); the gate tests read failures' text
  (`21b4b54`); `GateFailure` (`b3503c4`); `CargoTool` for stern and twin (`9190601`); `Gate::run` typed
  (`dc44447`); `LaunchFailure` (`c79dacc`); `SourceTreeFailure` (`8ecd1d0`); `CrapReportFailure` (`a60063e`);
  `RunLogFailure` (`09ad8f1`), the slice's last; slice 3 waits on Q25; `pub` with tests: the control layer's
  queries, verdicts and request steps (`48155f6`), its message and timer handlers (`4cd3941`); the protocol's
  recovery host steps (`09ecd8d`); three of consensus's steps (`f458322`); the protocol's recovery drain and
  certificate verifier (`24d718d`), the slice's last; named tuples: `BlockCommitments` (`b954ec4`),
  `UtcTimestamp` and xtask's `CivilDate` (`7ee8472`), `ReceiptSummary` (`069fd4c`), `LocalCompletion` (`d5b9214`),
  `Sha3Window` (`72ac407`); `WaitProgress` to `cluster_wait/` (`1f0b856`), `NodeHeight` (`abe8d70`), the slice's
  last; constants: the three nobody else reads, private (`8727d28`), the gas costs (`57b4222`), the
  opcodes (`c931cfe`), the engine's re-export shim out (`e660a94`), the state's layout bytes (`8f01aff`), the
  beacon's byte bounds (`efd7ee5`), three singles and the test node's peer (`59b99a1`),
  the runtime-timer harness's wait budget (`a2ad8dd`), the block gas limit's first readers (`75d9503`),
  its node and system-tests readers (`b455252`), validation's single-node tests (`2aded31`), the alias out
  (`8fd7429`), the slice's last; injected dependencies: the EVM engines composed in evm and node-infra
  (`e4de9df`), and in validation's helpers and single-node tests (`c576a8a`), their `Default` out (`1618598`); the bootstrapper's coordinator factory (`3505d8d`), the bootstrapper handed its coordinator (`cec0c5b`), the
  slice's last; records and objects: fourteen records' fields public (`3b243b5`), two
  system-tests configs by literal (`64b9ab9`), the node's admission, timing and diagnostics configs (`c72cdf3`), the bootstrapping config (`64f562b`), the NewView message's fields (`2e6d484`), the validator set updates (`ef5f4d9`), the recovery requests (`a247cf2`), the recovery facts (`df27ed7`), the peers' recovery statuses (`12b0c09`), the last getter-only one; xtask's report and command outcome
  (`d09acfa`), the first objects; the cluster config and the fleet's block limits (`6c83656`), the prune plan and the retained counts (`1121282`), the in-flight slot (`8230a8a`), the recovery context (`97cd2da`), the protocol limits (`d8dc9d5`), the transaction receipt (`07f708e`), the in-memory states (`618894b`), the protocol config (`a2cfb60`), the account's accessors and its production readers (`2e93617`), its node, node-infra and system-tests
  test readers (`68620c3`), validation's cluster tests (`31dea7c`), its fields private (`a449311`); the transaction's accessors and
  production readers (`ac456b9`), its system-tests and trace-driver readers (`cbbd2f6`), its fields private (`e39ac2c`); the recovery state snapshot (`525d620`); the world state snapshot's accessors and production readers
  (`439551b`), its fields private (`3afac18`); the hard state's and volatile snapshot's accessors, production
  readers and types tests (`1d27ff1`), their test readers (`278fb05`, `a7f097b`, `8d4b1cf`, `8843e5c`), their
  fields private (`4612b79`); bugs found on the way: the hard state's dropped transaction data (`e1232ca`,
  Q36).

## Next in line

Part A's slices, in order, with the criterion that put each there (`SETUP.md`, *Choosing the next slice*).

| # | slice | rules | `Chosen-By` |
|---|---|---|---|
| 1 | ~~time behind ports, tests off the clock~~ -- done at `44db935`; left: the cluster harness's 20 (decision 2), the wake signal's 6 (Q9) | injected-time 19 to pay, timeless-tests 46 | risk |
| 2 | ~~a failure type per concern, top-down~~ -- done at `09ad8f1`, 19 commits | typed-errors 49 | no-scaffolding |
| 3 | one file per implemented type (with the protocol's and control's fields) -- waits on Q25, open | implemented-in-one-file 2 | risk |
| 4 | ~~`pub` or private, each made `pub` with tests~~ -- done at `24d718d`, 5 commits | public-or-private 30 | fastest-feedback |
| 5 | ~~named values for tuples~~ -- done at `abe8d70`, 7 commits | distinct-tuple-elements 11 | fastest-feedback |
| 6 | ~~constants given one home~~ -- done at `8fd7429`, 12 commits; `DIAGNOSTIC_FIELDS` left (Q31) | shared-constants 91 | fastest-feedback |
| 7 | ~~a trait-typed field only from a constructor parameter~~ -- done at `cec0c5b`, 5 commits | injected-dependencies 5 | no-scaffolding |
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
- the trailers' line counts are the sums of the staged numstat, added and removed; check them against `git commit`'s
  own summary before the push, and amend the unpushed commit if they differ (row 104 did).
- the harness refuses `git switch -c` (deny `git * -c *`), `cp -r`, `awk`, `slotgate`, and a grep whose pattern
  holds backticks (read the file instead): use `checkout -b`, single-file
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
