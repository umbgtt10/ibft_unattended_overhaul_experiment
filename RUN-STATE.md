# IBFT unattended overhaul experiment -- run state

Where the run is, for the agent to resume from. Read after every compaction and at the start of every step;
updated before every commit, the last-commit line included. Overwritten as the run goes, committed and
pushed with the record at every commit.

## Now

- **Last commit:** `6de5afa` Pin that the dependencies hand over the tally factory they were given (row 225)
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
  Part A slice 8, records and objects -- 49 commits, rows 93 to 141 (`3b243b5` to `ca372f1`), against SETUP's 6-10
  (the upper end being "every reader changes", and three objects -- `Block`, the two states, the context snapshot
  -- taking seven each); records-and-objects 51 to 3 both ways, the 3 waiting on questions: `IbftProtocol` and
  `RecoveryControl` on Q25 (their fields are read by sibling implementation files), `NodeDiagnostics` on Q37 (a
  constructor of 28). Lessons, kept below with the slice's detail: list the readers by compiling with the fields
  private, a compile layer at a time; split a commit through `git stash --keep-index`; move a recorded dry4rust
  twin with its pair; dry4rust re-records from the start folder with `--path .`.
  Part A slice 9, each record in the module of what uses it -- 7 commits, rows 142 to 148 (`72ebb5b` to
  `e7da3a4`), against SETUP's 4-6; record-home 23 to 1 both ways, the 1 `RecoveryMessageFields`, waiting on Q25
  (its one user is one of `IbftProtocol`'s implementation files, which `named-subject` then refuses). Two of the
  seven past the threshold by SETUP's provision (`TransactionStatus`, `TimerKey`). Lessons: `cargo xtask measure`
  reads the staged tree; a folder of records adds no file to a full directory; cargo fmt leaves over-long imports
  unsorted; never `--write-baseline` over a reported offence.
  Part A slice 10, living documents -- 2 commits, rows 149 and 150 (`520ee10`, `7ffb333`), against SETUP's 1-2;
  `support`: CLAUDE.md's six stage-2 gates, the baseline's 41 (with what they are) in CLAUDE.md, stern4rust.toml
  and OPEN_POINTS, stern4rust.toml's stale package and rule counts, the justfile's comment; the three open points
  this run closed retired verbatim into `docs/history/[IMPLEMENTED] RESOLVED-OPEN-POINTS-2026-11-07.md`, each with
  a **Closed:** line. **Part A is done.**
  Part B slice 1, `returned-mutations` in evm -- 13 commits, rows 151 to 163 (`3c21b53` to `5c6e9eb`), 05:33Z to
  06:30Z; 57 to 0 in evm, everything on 252 to 195, as configured 41 throughout; evm's stand-down out at
  `5c6e9eb` (`support`, stage 2 alone), evm applying all thirty-nine. Lessons: a body moved into an `impl` is
  re-indented, every line counted twice -- go a group of functions a commit; a function moved into an object cannot
  be called by the free functions left behind -- callers first, callees last; a test renamed and moved at once is
  not paired by test-diff -- a `Maps:` line each; rustfmt wraps a call whose arguments pass 60 columns; a staged
  change past the threshold is split by keeping the finished files in scratch, editing the tree back to the first
  half, and copying them back for the second.
  Part B seam 1, execution, with `execution-types` dissolved -- 13 commits, rows 164 to 176 (`8afa421` to
  `c83fba2`), 06:33Z to 07:44Z; counts 41 and 195 throughout, tests 3,680 to 3,703; `types` depends on
  `etheram-core` alone, `node` on no evm in production, evm on `node` for its two ports and `types` for the
  model, held by `cargo xtask evm-boundary`; `node-contracts` holds the execution contract all three engines
  pass. 3 commits past the threshold (the merge and the port's move, planned; the slot discovery's move, Q39).
  Lessons: a move across crates updates every importer at once and cannot be split without a duplicate or a
  shim -- count its importers before choosing where a type first lands; `tests` allows `tests/` alone, so a test
  crate's `src`, a manifest or a dev-dependency makes a commit `refactoring` (Q41); a contract is what every
  implementation already does -- read the default's answer first (the no-op reports nothing); a stern-clean
  rule can still be a dry4rust duplicate of a sibling's three-line idiom.
  Part B seam 2, `node`'s own dependencies -- 1 commit, row 177 (`a352577`), 07:48Z to 07:51Z, `Chosen-By:
  no-scaffolding` (the three skipped-rule slices add scenario sources and tests in validation and system-tests
  that the seams to come reassemble -- Faction's cluster port, Beggar's recovery port, the observer's facets --
  so after them each is written once; among the seams the set order puts step 2 next, not decided again):
  `Ed25519SignatureScheme` and its ten tests into node-infra's implementations, `ed25519-dalek` with it; `node`
  depends on `etheram-core`, `types`, `faction`, `recovery-control` and `recovery-ibft`. Past the threshold, 25
  files and 470 lines: SETUP counted about 16, the importers are 14 (Lesson: count a move's importers, registries
  and manifests before trusting an estimate). No document said where the scheme lives; README's node-infra row
  names it since `a4f3033`.
  Part B seam 3, Faction -- 18 commits, rows 178 to 195 (`951d746` to `a4f3033`), 07:58Z to 09:30Z, against
  SETUP's share of the seams' 75-100; `Chosen-By: owner-order`, the design Q42, the observer facet and the
  one move past the threshold Q43. `node` depends on no bootstrapping crate: its port
  (`BootstrappingCoordinator::process(BootstrappingCommand) -> Vec<BootstrappingOutcome>`) and the readiness event
  (`ReadinessTransitionObserved { command, transition: &ReadinessTransition }`) speak node's mirror of Faction's
  names, so the readiness log reads as before; `node-faction` holds the translation, the coordinator wrapper, the
  bridge and the factory, the one crate on `faction`; node-infra's `BootstrapperBuilder` chooses Faction (the
  default) or `MembershipVariant::Fixed`, `FixedMembershipBootstrappingCoordinator` answering as Faction does but
  refusing joins; `BootstrappingCoordinatorContract` in node-contracts, eight standard scenarios, passed by both.
  Counts 41 and 195 throughout; tests 3,703 to 3,757; dry4rust 196. 1 commit past the threshold (`d7d2c45`).
  Lessons: learn a black box's answers by driving it in scratch, never its sources -- the contract's scenarios
  were each checked against those answers before the code; a mirror vocabulary with the same variant names keeps
  every `{:?}` log line byte for byte, pinned by a Debug-identity test per variant; a new observer facet taken
  first lets a plug-in's tests implement one method instead of 78; rustfmt wrapping a signature can push a
  three-line getter into dry4rust's reach -- name the type instead; a type's test file wants its scenario helpers
  to differ in data, not in code, or dry4rust pairs them.
- **Slice in progress:** Part B seam 6, consensus (SETUP, *Part B*, step 6), started 11:29Z, `Chosen-By:
  owner-order`, `Over:` the three skipped-rule slices. The bug first (Q48, answered): (a) the control -- done at
  `11bfec3` (`tests`; `transfer_cluster()`, `transfer_block_for_round_one()` and
  `pre_prepare_carrying_a_transfer_after_its_new_view_commits` in validation's
  `cluster/consensus/ibft_cluster_message_ordering_tests.rs`). (b) the bug-fix -- done at `8e1ca00` (red on
  `11bfec3`'s sources at height 0, green after; traces identical, the corpus holding no held proposal; three tests
  that drove the protocol directly now deliver what it re-queued, their claims kept; OPEN_POINTS' entry still to
  retire with the seam's documents). (c) the port's design -- Q49 (answered): `VoteTally::step(TallyInput) ->
  TallyOutput`, one per phase; the volatile state the adapter, its methods unchanged; node-infra's `QuorumTally`
  default by a `VoteTallyFactory` the dependencies take. (d) the port and its data model -- done at `e329250` (the
  220th, all three stages): `node/src/traits/{vote_tally,vote_tally_factory}.rs`, `node/src/ibft/tally/{vote_key,
  tally_input,tally_output}.rs` (`Cast{key,voter}`, `Read{key}`, `Cleared`, `Resized{quorum}`, `Exported`,
  `Restored{votes}`; `Tallied{voters,quorum_reached}`, `Exported{votes}`, `Done`; `VoteKey{height,round,block_hash}`).
  (e) node-infra's `QuorumTally` (`node-infra/src/tallies/`) delegating to node's `VoteTracker`, and
  `QuorumTallyFactory` (`factories/`, a unit struct) -- done at `bd41a48`, nine tests; the port's export and restore
  now in the WAL's tuple-keyed form, so nothing converts. (e2) the read -- done at `97441de` (unplanned, Q49
  amended: `read(&self, TallyQuery::{Key, Exported})` beside the step, since the certificate and the volatile
  snapshot read from `&self`; two tests renamed, `Maps:`). (f0) volatile_state_tests through helpers -- done at
  `1cd555a` (`fresh_state(quorum)`, `restored_state(quorum, snapshot)`; the constructor-named test and the two
  `from_snapshot` sites direct). Lesson: stage 1's checks and stage 2's dispatch went in one batch here -- they
  passed, but dispatch the next stage only in a later turn. (f) the wiring -- done at `58844eb` (16 files, 333
  lines; the factory after the recovery factory, all 30 sites `Box::new(QuorumTallyFactory)`; the volatile state
  the adapter through a private `key_tally` and `exported_votes`; 8 dry4rust pairs re-keyed, 195 both sides; the
  dependencies' new test parked for the next commit, past the threshold together: 391). (f2) the test
  `into_parts_hands_over_the_tally_factory_it_was_given` with its `QuorumOfOneFactory` stub -- done at `6de5afa`
  (`tests`, 58 lines, stages 1 and 2). Next: (g) `VoteTracker` (`node/src/ibft/vote_tracker.rs`, tested in
  `node/tests/ibft/vote_tracker_tests.rs`) moved wholly into node-infra's `QuorumTally` -- its dry4rust group
  `0630c58f045165e4`, "VoteTracker::new / EtheramOutputDispatcher::new", moves with it. The
  plan of (f) was: the dependencies take the factory (30 sites, one line each, the seven dependency-helper pairs re-keyed as
  Q46), the volatile state steps two tallies; (g) `VoteTracker` moved into `QuorumTally`; (h) `VoteTallyContract`;
  (i) the documents and the replayed-proposal open point retired. The plan of (b) was: the two red tests are in
  `overhaul-scratch/c219-ibft_cluster_message_ordering_tests.rs` (the whole file with them;
  `future_round_pre_prepare_carrying_a_transfer_after_timeout_round_replays_and_commits` and
  `unjustified_pre_prepare_carrying_a_transfer_replays_on_its_new_view_and_commits`, both failing at height 0 on
  `11bfec3`'s sources) -- `cp` it back, then `Action::RequeueMessage { source, message }` (types), the partitioner
  to executions, `EtheramNode`'s served-first queue, `ActionKind::RequeueMessage`, the blocker `Requeued`, both drains
  re-queuing a `PrePrepare` and replaying votes in place; every exhaustive `Action` match (output dispatcher,
  etheram_state, action_kind, partitioner, node's and validation's action helpers); then the tests that saw the old
  replay updated. Then the port's design, its own entry. The certifier, done: Part B seam 5 (SETUP, *Part B*, step
  5), started 11:15Z, design Q47 -- its steps below, after Beggar's. Beggar, done: Part B seam 4 (SETUP, *Part B*, step 4) -- `Chosen-By: owner-order` (Faction,
  Beggar, the certifier, consensus), `Over:` the three skipped-rule slices, the certifier, consensus; started
  09:31Z. SETUP's order: characterization, `#[must_use]`, peer knowledge out of `recovery_state.rs`, the
  snapshot apply split, three bugs test-first, the port shaped by today's `recovery-control` (its default),
  whose trait must not fix who drives a transfer. Gate 8 wants `node` on `etheram-core` and `types` alone, so
  `recovery-control` and `recovery-ibft` leave node's manifest by the seam's end. Steps: (a) characterization --
  done at `187b65c` (`tests`): in `ibft_cluster_recovery_tests.rs`, five validators, an eight-block window
  (`RETAINED_BLOCKS`), node 4 partitioned for 24 heights, healed and left to its own check and retry timers
  (`recovery_messages_delivered_to(cluster, node, passes)` collects what it received): refused `CannotServe {
  oldest_available: 17 }` by all four peers (0, 3, 2, 1, two of them twice) before peer 1's `SnapshotResponse`
  (head 23) lands it at 24 with the majority's root; then it follows height 24 by blocks to 25. (b)
  `#[must_use]` -- done at `f2a36a8`: `RecoveryCoordinator`'s seven state-deciding bools and `RecoveryState`'s
  seven, `EpisodeTracker::start`/`start_untargeted`, node's `MessageBuffer::try_push`/`make_room_from`; no
  production site discarded one, 38 test sites took `let _ =` from clippy's list through a scratch tool
  (`overhaul-scratch/must-use-fixer -- <root> <clippy short output>`). (c) peer knowledge -- done at `9b496e6`:
  `PeerKnowledge` (`recovery/control/src/peer_knowledge.rs`; `observe_status`, `highest_peer_height`,
  `peer_status`, `known_peer_count`, `peers_ahead` an iterator, `statuses`, `broadcast_attempts`,
  `take_broadcast_attempt` must-use), `RecoveryState` holding it; the inherent `observe_status` gone (a
  delegation twin of `note_blocks_recovered` that reshaped a recorded dry4rust group), seven test files importing
  `RecoveryCoordinator`; five of its tests (290 lines with the rest would have been 373). (c2) its other
  seven tests -- done at `fcb922a` (`tests`). (d) the snapshot apply split -- done at `7508819` (the 200th,
  all three stages): `IbftProtocol::adopt_snapshot_jump(height, remaining_updates, adopted_validators)` in
  `consensus.rs`, plain data in; the host still fast-forwards with recovery-ibft's `ScheduledValidatorFastForward`
  (moving it, 385 lines counted twice, would pass the threshold) and builds the outcome. (e1) the bug-fix --
  done at `5013d0e`: after `AdoptSnapshot` the apply pushes `RecoveredBlockImporter::round_entry_actions(height,
  peer)`; the test failed on the parent's sources at `TimeoutRound { height: 400, round: 0 }`; traces identical
  (they hold no timers); bug fixes now 3. (e2) recovery's message kinds -- done at `1e7ac29` (`bug-fix`):
  `PeerMessageKind` (`node/src/observer/peer_message_kind.rs`, bytes 0-9, `Unknown { byte }` else) on
  `DuplicateMessageRejected`/`PeerSequenceInvalid`; three tests compared `{:?}`, compiling on the parent and
  failing there with "PrePrepare". (e3) `requests_blind` -- done at `643bfd5` (`bug-fix`): the
  beacon-level test (written while 202's stage 5 ran, in a node test file outside that build; 202 committed its
  staged index alone; Lesson: edit nothing while a stage runs) failed with 0 for 1; the fix pushes
  `recovery_request_diagnostic(current_height)` in `force_request_as_last_resort`. The three recorded bugs are
  fixed. (d3) the unavailable path's cancel -- done at `a5e9b26`:
  `ControlEvent::TransferUnavailable` -> `handle_transfer_unavailable` -> `CancelRetry` at the layer's height,
  `RecoveryEvent::SnapshotUnavailable` translating, the host asking `process_recovery`; the direct-call test
  realizes the outcome in its Act. (d4) the jump re-arms the layer's own check -- done at `4fbf853`:
  `ControlEvent::ChainJumped { to }` (a decision: `handle_chain_jumped` stands the layer at `to`, `ArmCheck`),
  `RecoveryEvent::SnapshotAdopted { height }`; the host pushes `round_entry_timers` and asks `process_recovery`.
  The split is done. The port's design is Q44 (answered): the port is today's call site, `RecoveryProcess`
  (it admits a pushed transfer: pinned, not changed); its vocabulary stays in recovery-control until gate 8, after
  the observer split; `node` stops building the default. (f1) the push pinned -- done at `e248472` (`tests`,
  `process_range_imported_with_nothing_asked_still_marks_the_import`). (f2) `#[must_use]` on
  `RecoveryProcess::process` and `IbftRecoveryProcess::process` -- done at `a8a1d57` (no site discarded one).
  (f3) the factory port and node-infra's default -- done at `835af20`: `RecoveryProcessFactory::build(self:
  Box<Self>, timing: RecoveryTimingConfig) -> Box<dyn RecoveryProcess>` in `node/src/ibft/assembly/` (Q45: the
  first attempt, in `node/src/traits`, was refused by stage 1's recovery boundary, whose node rule refuses
  `RecoveryProcess` outside the assembly; the gate stands, the trait moved), node-infra's
  `RecoveryControlFactory::new(coordinator, recorder)` in a new `node-infra/src/factories/`, three tests. Lesson:
  dispatch a stage only after the previous one's check -- the refused attempt's stages 2 and 5 were dispatched in
  its stage 1's batch and ran on a commit already refused. (f4a) the factory's default -- done at `0d46800`
  (unplanned, Trigger Q46: `RecoveryControlFactory::default()`, the one-line form for the 28 default sites; the
  dry4rust group `cf60ee414b625be5` of seven `empty_state` helpers recorded with it as an eighth member, as Q33 did).
  Lesson: `cargo dry4rust baseline --path <absolute>` writes closure names with the checkout's path -- restore
  `closure at ./` (Edit, replace all) and read the diff before staging. (f4b) the wiring -- done at `e04403c` (the
  210th, all three stages): every site through `RecoveryControlFactory::default()` but node's
  `setup_dependencies_with_recorder` and `dependencies_around`, which take `new`; the dry4rust baseline's seven
  dependency-helper pairs re-keyed and the recorder helpers' near pair paid, 196 to 195. The plan's (f4), now
  done: `IbftProtocolDependencies` takes
  the factory in place of the coordinator and the decision recorder (13 files build it: node-infra's protocol
  builder, node's three test helpers and its dependencies test, system-tests' protocol factory, validation's five),
  `into_parts(timing)` building through it; (f5) the documents (CLAUDE.md's recovery-boundary paragraph -- the
  assembly exemption -- RECOVERY-AS-BUILT §9, README) -- done at `29ef8e4` (`support`, all three stages: xtask
  changed), with the recovery boundary's first rule exempting nothing now, checked by a planted `RecoveryState`. The
  port is done. (f6) -- done at `f3932b0` (`support`, stage 2; verbatim checked by exporting HEAD's OPEN_POINTS to
  scratch and grepping `-v -x -F -f` against both new files: one line left, the one rewritten): retire the open
  points the seam closed, moved verbatim into
  `docs/history/[IMPLEMENTED] RESOLVED-OPEN-POINTS-2026-11-07.md` as `7ffb333` did, each followed by what closed
  it -- the snapshot apply (`7508819`, `5013d0e`, `a5e9b26`, `4fbf853`), the duplicate `PrePrepare` (`1e7ac29`),
  `requests_blind` (`643bfd5`), with the two pointers (OPEN_POINTS' counter class, RECOVERY-AS-BUILT §9's queue)
  repointed. (f7) -- done at `455df75` (`support`, stage 2; the retention ADR's "cannot claim" gains a bracketed
  note; the move checked the same way, nothing missing). (f8) `#[must_use]` on
  `IbftProtocol::accept_peer_message` -- done at `52bc729` (SETUP's "any bool whose falsity means 'I declined to
  change state'", the one the old entry named; four dispatch tests took `let _ =`; the history's closing note
  updated). **The Beggar seam is done**: 19 commits, rows 196 to 214 (`187b65c` to `52bc729`), 09:33Z to 11:14Z.
  **The certifier's seam** (SETUP Part B step 5), `Chosen-By: owner-order`, design Q47 (answered): the port is today's
  `SignatureScheme`, no height; the boundary characterized already; Ed25519 stays in node-infra, the no-op in node.
  (a) the contract suite -- done at `0476b72`: `SignatureSchemeContract::new(BTreeMap<PeerId, BoxedSignatureScheme>)`,
  `check(data)`, `check_standard_scenarios()` (empty, `b"prepare"`, a 32-byte hash, 1 KiB), breach
  `SignatureContractBreach::{MessageSignatureRefused, CertificateSignatureRefused} { signer, verifier }`, in
  `node-contracts/src/certification/`, five tests over a scripted scheme. (b) Ed25519 (node-infra's test file),
  the no-op and the switched scheme, verified and bypassed (node's test files; `node` takes `node-contracts` as a
  dev-dependency), run it -- done at `1f3b0f3`, the members built inside each test body. (c) -- done at `b0756e2`
  (`support`, stage 2; CERTIFICATE-CHECK-SCHEME.md gained a bracketed note too). **The certifier's seam is done**: 3
  commits, rows 215 to 217, 11:15Z to 11:28Z. Next: consensus (SETUP Part B step 6), last of the bookkeepers -- the
  step shape agreed on 2026-10-18 (decision 9), and the replayed-proposal bug-fix test-first. The plan of (c) was: the documents (CLAUDE.md's node-contracts paragraph, README's node-contracts row). The plan of (f7) was: the `#[must_use]` entry (`f2a36a8`, `a8a1d57`) and the retention-and-snapshot join, and the peer
  knowledge entry updated (`9b496e6`); then the certifier's seam.
- **Faction's detail:** Part B seam 3, Faction (SETUP, *Part B*, step 3) -- `Chosen-By: owner-order` among the
  bookkeepers (Faction, then Beggar, the certifier, consensus last), over the skipped-rule slices for seam 2's
  reason; the design is Q42. `node` names faction in the port (`BootstrappingCoordinator::process(Command) ->
  Vec<Outcome>`), the readiness event (`ReadinessTransitionObserved`, rendered field by field with `{:?}` by
  system-tests' logging observer), and three implementations (Faction as coordinator, the observer bridge, the
  factory); 15 files build a bootstrapper with `NoOpBootstrappingCoordinator` + `FactionCoordinatorFactory`.
  Faction's vocabulary, from the compiler (`overhaul-scratch/faction-probe`): Command -- ParticipationObserved,
  ReadyObserved, LocalParticipationCompleted, DeadlineExpired, JoinRequested, JoinApproved, JoinRejected (each a
  `peer_id: u64` where it has a field), Probe; Outcome -- ParticipationAccepted, ReadyAccepted,
  DuplicateParticipationIgnored, DuplicateReadyIgnored, NonMemberIgnored, LocalParticipationCompleted,
  BroadcastLocalReady, AcknowledgeRejoin, DeadlineMissed { confirmed_count: usize }, EmitJoinRequest,
  MemberAdmitted, DuplicateMemberIgnored, JoinDenied, Concluded { mode }; Conclusion -- Bootstrapped; PeerState --
  Fresh, Pinging, Collecting, Bootstrapped, TimedOut. Steps, each within the threshold: (a) characterization --
  done at `951d746` (`tests`, eight tests of what Faction answers through the port); (b) node-infra's
  `BootstrapperBuilder` (`new`, `with_config`, `inactive`, `build() -> Bootstrapper`, Faction by default) with
  node-infra's protocol builder, system-tests' protocol factory and validation's three sources through it -- done at
  `73cb8eb` (333 lines; dry4rust 196 to 193, Q33 taken again); (c) node's three test helpers through it -- done
  at `b31593c` (dry4rust 193 to 196, the six exact pairs re-keyed, Q35; `bootstrapper_tests.rs` keeps the
  constructors it tests, stern's `tested-public-api` refusing `inactive` untested); validation's five tests -- done
  at `f2f5e31` (171 lines; the two together were 372); the factory is now named by node-infra's builder, its own
  file and test, and `bootstrapper_tests.rs` (and NoOp's own file and test); (d) node's data model in
  `bootstrapping/` -- done at `a59c662` (six files, no tests: records and enums alone, twin4rust passing
  type-only files; `ReadinessView { peer_state, concluded, required_count }`, `ReadinessTransition {
  previous_view, outputs, new_view }`); (e) the `node-faction` crate (`etheram_ibft_node_faction`, `no_std`, in
  stage 1's lists and crap4rust's, twin4rust's and iceberg4rust's) with `FactionTranslation`, its tests asserting
  `{:?}` alike for every variant -- the commands both ways done at `17fd986`, the outcomes and the transition at
  `4b8a7a1` (`Transition::previous_view()` returns its view by value); (f) the factory into it -- done at `c43c0f2`
  (15 files; node-faction on etheram-core too, node-infra on node-faction, node and node-faction dev-depending on
  each other's neighbours); (g0) the peer-discovery facet (Q43) -- done at `a2f5f0e` (`PeerDiscoveryObserver` with
  the readiness event, `ProtocolObserver` its subtrait, six implementors; `Chosen-By: dependency`, unplanned); (g)
  the bridge and (h) the coordinator in one commit -- done at `d7d2c45` (past the threshold, 1,125 lines, Q43; the
  coordinator `FactionBootstrappingCoordinator`, a wrapper; the bridge on `Arc<dyn PeerDiscoveryObserver>`;
  node-faction's tests record readiness through a dozen-line `RecordingObserver`, `#[derive(Default)]` so its
  `new` is no dry4rust twin of `PartitionTable::new`; node keeps faction for the port's and event's types); (i) the
  port's commands and outcomes in node's vocabulary -- done at `d1246b3` (8 files; the wrapper translates through
  `FactionTranslation`, the bootstrapper imports no faction; 13 assertions renamed); (j) the event's --
  `ReadinessTransitionObserved { command: BootstrappingCommand, transition: &ReadinessTransition }`, the bridge
  translating, the logging observer reading fields -- and `faction` out of node's manifest -- done at `082c59c` (6
  files; the bridge tests' recorder tuple named `ReadinessRecord` so its getter stays three lines, outside a
  recorded dry4rust group of 11); (k) the default: Faction's answers beyond the eight pinned at 178 probed in
  `overhaul-scratch/faction-probe` (Fresh until any participation, ready or completion; a deadline refused only
  then and after the conclusion, else `DeadlineMissed` with the ready count each time; participations after
  local completion `AcknowledgeRejoin`; readies before completion counted, concluding only at or after it; after
  the conclusion readies, deadlines and completion refused; joins: `EmitJoinRequest`, `MemberAdmitted` or
  `DuplicateMemberIgnored`, `JoinDenied`); (k1) `FixedMembershipBootstrappingCoordinator::new(local, members,
  threshold)` in node-infra's implementations (19 files there now), Faction's answers but joins (`JoinDenied`,
  a member's approval `DuplicateMemberIgnored`), ten tests -- done at `c885cf9`; (k2) its other thirteen
  tests -- done at `356ecee` (`tests`, 23 in all); (k3) `FixedMembershipCoordinatorFactory` (the observer
  unused: no readiness transitions) and the builder's choice, `MembershipVariant { Faction, Fixed }` in
  `variants/`, `with_membership`, Faction the default -- done at `44a0b7a` (node-infra's implementations hold
  20 files now, `directory-file-count`'s limit); (l) `BootstrappingCoordinatorContract::new(factory,
  observer)` in node-contracts' `src/bootstrapping/`: `check(local, validators, threshold, commands,
  concluding_step)` -- the first local completion answers `BroadcastLocalReady`, `Concluded { Bootstrapped }`
  exactly at the expected step (breaches `LocalReadyNotBroadcast`, `UnexpectedConclusion`, `MissedConclusion`,
  each with its step), six tests over a data-driven scripted coordinator (node-contracts on `etheram-core`,
  dev-depending on node-infra for `NoOpObserver`) -- done at `515d818`; (l2+m) `check_standard_scenarios()`
  -- eight, each checked against Faction's probed answers: the quorum on a ready, at local completion, through a
  duplicate and a non-member, not before completion, after a missed deadline, nothing after the conclusion, the
  local peer's own ready counted once, a lone member at one -- run by both factories' test files
  (`check_standard_scenarios_<coordinator>_returns_ok`, node-faction dev-depending on node-contracts) -- done at
  `bf5c717`, both passing; (n) the documents -- done at `a4f3033` (`support`, stage 2): README (the opening's
  Faction sentence, `#![forbid(unsafe_code)]` in 10 crates not 8, `no_std` adding node-contracts and node-faction, a
  `node-faction` row, node's bootstrapping port, node-infra's row with the Ed25519 scheme, the fixed membership and
  `BootstrapperBuilder`, node-contracts' row with the bootstrapping suite, the seams sentence), CLAUDE.md (eleven
  members, node-faction among those with no section, the bootstrapping suite, a paragraph on the seam),
  `stern4rust.toml`'s comment list, OPEN_POINTS' package list and its point 3's stale
  `bootstrapping_faction_coordinator.rs:18`. The seam's last.
- **Seam 1's detail:** Part B seam 1, execution, with `execution-types` dissolved (SETUP, *Part B*, step 1) --
  `Chosen-By: dependency` (every file the three skipped-rule slices add -- scenario sources, split-out types,
  tests -- would import `execution_types::` paths the merge then rewrites; merged first, each is written once),
  `Over:` paired-test-file, tested-public-api, single-implemented-type; started 06:33:39Z. Steps: (1) the merge --
  done at `8afa421` (`refactoring`, planned past the threshold: 376 files, 5,128 lines). The former crate is one
  module, `etheram_ibft_types::execution` (`common_types` nested inside it: beside it `types/src` held six folders,
  one past `directory-subfolder-count`'s five), its tests under `types/tests/execution`; the 312-file path rewrite
  by a scratch program, `overhaul-scratch/path-rewriter` (`cargo run --manifest-path .../Cargo.toml -- <root>
  <old prefix> <new prefix> [<only under>]`), rustfmt resorting imports and four sorted by hand; xtask's three
  package lists and the justfile's stage-1 lines follow. (2) the documents naming the crate -- done at `de03fb2`
  (`support`, stage 2): CLAUDE.md, `stern4rust.toml`'s comments, README, IBFT-SPEC-COVERAGE, RECOVERY-AS-BUILT,
  two ADRs' paths; the leaf-`types` open point retired into the 2026-11-07 history file; TEST-LADDER's dated
  measurement and the history files left as written. Found: `node` depends on `evm` in production -- the storage
  access plan (`node/src/runtime/context/tiny_evm_storage_access_plan.rs`, used by `EtheramNode::execute_block`
  and the snapshot context builder's pre-prepare and propose contexts) asks `TinyEvmStorageSlotAnalyzer` -- so the
  port's move would close a cycle (Q39, answered): (3a) node's tests build the context builder through
  `node_helpers::snapshot_context_builder()` -- done at `8ef334d` (`refactoring`, unplanned, Trigger Q39; three
  dry4rust `deliver` groups re-keyed, Q35); (3b) the port `StorageSlotDiscovery` (`storage_slots(bytecode) ->
  Option<Vec<Hash>>`, `node/src/traits/storage_slot_discovery.rs`) and its default `TinyEvmStorageSlotDiscovery`
  in node-infra's implementations -- not evm: evm cannot depend on `node` while `node` depends on evm -- two
  tests, unused yet -- done at `d170c7e`; (3c) the wiring -- done at `142ce22` (15 files): the access plan taking
  `&dyn StorageSlotDiscovery`, `EtheramNode::new` (after the engine) and `SnapshotContextBuilder::new` (third)
  handed it, `node` keeping evm in dev-dependencies only, nothing in `node/src` naming evm; call sites: node-infra's two builders, validation's cluster builder and test node, two
  validation node tests, system-tests' gRPC node and context factory, node's helpers and the context builder's
  own tests with custom limits and the access plan's tests; (3d) the access plan's `TinyEvm` name kept: every
  assembly plans with the TinyEvm analysis, and a rename moves the file and its tests -- past the threshold for a
  name; (3e) `ExecutionEngine` into `node/src/traits` -- done at `4cfcce2` (`refactoring`, the second planned past
  the threshold: 28 files, 94 lines; evm depends on `node` for it now); (4) evm a plug-in -- its manifest already
  `node`, `types` and its two hashing crates; `TinyEvmStorageSlotDiscovery` into `evm/src/tiny_evm` at `2883602`
  (`refactoring`, unplanned, Trigger Q39; 21 files, past the threshold: 11 importers, system-tests taking evm);
  `NoOpExecutionEngine` stays node-infra's, the builders' defaults evm's value transfer (Q40) -- done; (5) the
  contract suite in a `node-contracts` crate, run by both engines and the no-op -- what all three satisfy
  (the no-op returns no results at all): results never outnumber the transactions, result i is transaction i's
  sender's, gas within each transaction's limit, mutations naming only a sender or recipient of the block (a
  storage write only a recipient -- the access plan loads nothing else), and the same inputs giving the same
  results; every commit of it `refactoring` (Q41: `tests` allows `tests/` alone, and a test crate's `src` counts
  as production). (5a) the crate -- `no_std`, `etheram_ibft_node_contracts`, `src/execution/` -- with
  `ExecutionEngineContract::new(engine)`, `check(block, accounts, storage) -> Result<(), ExecutionContractBreach>`
  and the first two clauses, four tests against a scripted engine (`fn(&Block) -> ExecutionResult`), in stage 1's
  no_std, test and test-layout lists -- done at `3a96314` (272 lines); (5b) the gas and mutation clauses
  (`GasAboveLimit`, `MutationOutsideBlock`, each with the result's index; `check_transaction_result` takes the
  block, a private `names_a_party_of`) -- done at `a7bec37`, four tests; (5c) repeatability (`NotRepeatable`, a
  second execution compared result by result; a `DriftingEngine` fake) and `check_standard_scenarios()` over five
  private `ExecutionScenario`s -- empty block, a transfer, a zero gas limit, a relay A to B to C, a call storing 42
  at slot 0 of a contract holding storage, 100,000 gas each -- done at `32ccbe1`, three tests; (5d) the three
  engines' test files each run it (`check_standard_scenarios_<engine>_returns_ok`), the workspace dependency and
  evm's and node-infra's dev-dependencies with them -- done at `c40ab03`, all three passing; (6) the boundary
  check -- `cargo xtask evm-boundary` in stage 1, an allow-list `SourceRule` (`xtask/src/source/allowed_paths.rs`):
  over `evm/src`, `etheram_ibft_types` only under `execution::`, `etheram_ibft_node` only under its two execution
  ports -- done at `bd30c4f` (`support`, seven tests; an injected WAL path reported at its line, the source
  restored; a first `is_allowed` duplicated `ForbiddenIdentifiers::is_exempt` exactly, so inlined); (7) the
  documents, the seam's last -- done at `c83fba2` (`support`, stage 2): CLAUDE.md's stage 1, members and two
  paragraphs (the evm boundary, `node-contracts`), README's crate table, `stern4rust.toml`'s and OPEN_POINTS'
  package lists; TEST-LADDER's dated count and the ADRs left as written.
- **Slice 1's detail:** `Chosen-By: risk` (the largest of the
  skipped rules' pieces, and the one most likely to surprise), `Over:` the other three skipped-rule slices and the
  execution seam; started 05:33:28Z. 57 offences with the rule on (list:
  `overhaul-scratch/returned-mutations-evm.txt`): 52 in `tiny_evm_ops.rs`, 5 in `tiny_evm_engine.rs`. The shape
  (Q38): the machine's changing parts -- stack, memory, `memory_words`, gas, the working contract storage and the
  mutations, not the counter -- become `TinyEvmMachineState` (`evm/src/tiny_evm/tiny_evm_machine_state.rs`), the
  operations its `&mut self` methods; arithmetic through `TinyEvmWordAlu`. A body moved into an `impl` is
  re-indented, every line counted twice, so the steps are: (0) push returns the next counter, in place -- done at
  `3c21b53` (252 to 251); (1) `refactoring`: the state with every field, `new(stack, gas, local_storage)` (memory
  starts empty -- the mload test, when it moves, writes its word with `exec_mstore` first), `gas`, `stack`,
  `take_mutations`, `charge_opcode_gas(opcode, contract)` and methods calling today's free functions with
  `&mut self.<field>`, ten tests; the machine holds it -- done at `0a38d3c` (352 lines, 8 short of the threshold:
  rustfmt wraps a call whose arguments pass 60 columns); (2..) `rule-offence`s, a group a commit, each with its
  tests moved from `tiny_evm_ops_tests.rs` to `tiny_evm_machine_state_tests.rs`: dup, swap and push -- done at
  `bc9904f` (251 to 248); the arithmetic dispatcher through `TinyEvmWordAlu` with the ten `exec_*` gone, in two
  halves by dry4rust group (their tests renamed `execute_arithmetic_opcode_*`, each `Maps:`-ed -- a rename across
  files is not paired): the dispatcher and add, mul, sub, lt, gt at `a022bd1` (248 to 242; dry4rust 199 to 197,
  the `exec_add`/`exec_mul`/`exec_sub` and `exec_lt`/`exec_gt` groups paid), then div, eq, iszero, and, or at
  `bf1b812` (242 to 237; dry4rust 197 to 196, the `exec_and`/`exec_or` group); the environment at `60b3373`
  (237 to 232; private `address_word` and `calldata_word`); sload and sstore at `78c9a21` (232 to 228; getters
  `local_storage`, `mutations`); the memory group callers first -- a function moved in cannot be called by the
  free ones left: the dispatcher, mstore and mload at `89735b7` (228 to 216; getter `memory`; private
  `continue_or_out_of_gas`, and a private `ensure_memory_window` calling the free one; the mload test stores its
  word first, from 1,003 gas); `ensure_memory_window` and `expand_memory_if_needed` at `ba4e857` (216 to 210; the
  private one made `pub`, a `memory_words` getter); sha3, `expand_memory_for_sha3` and `charge_sha3_words` at
  `6912269` (210 to 202; the 160th, all three stages; one change of 375 lines split before landing -- the finished
  four files kept in `overhaul-scratch/c161-*.rs`, the tree edited back to the first half); `sha3_range` and
  `pop_word` at `e444f5b` (202 to 200) -- `tiny_evm_ops.rs` holds no offence now; the engine's five at `36c3d9e`
  (200 to 195; a private `WorkingState` record of the two maps, taken and returned by value) -- evm holds no
  `returned-mutations` offence; the rule out of evm's skip at `5c6e9eb` (`stern4rust.toml`, CLAUDE.md,
  `docs/OPEN_POINTS.md`), the slice's last. A test
  whose `bool` and out-parameter merge into one `Option` loses one assertion: `Maps:` it to itself, Q38.
- **Slice 9's detail:** record-home 23 (SETUP: 4-6
  commits; 13 records named in a crate's public API but declared in another type's file, 8 wire types used by
  several files but not `pub`, 2 records with one user in files of their own); listed in
  `overhaul-scratch/record-home.txt` (`cargo stern4rust --rule record-home --baseline <empty> --offence-threshold
  0`); `Over: living documents`, the one other Part A slice that can come next. The 8 wire types made `pub` at
  `72ebb5b` (63 to 55 / 274 to 266) -- `cargo xtask measure` reads the staged tree, so `git add` before it.
  types' four WAL field groups into `state/wal_*_fields.rs` at `79526b2` (55 to 51 / 266 to 262; no test files --
  twin4rust asks none of a record); `ContextHeader`, `EtheramNodeInfrastructureInputsParts` and `BytecodeStep` at
  `d74cdc1` (51 to 48 / 262 to 259); `EpisodeState` private in episode_tracker.rs at `dcd2720` (48 to 47 / 259 to
  258) -- `RecoveryMessageFields` stays, its one user `dispatch.rs` being one of `IbftProtocol`'s implementation
  files with no type of its own, so `named-subject` refuses it there: it waits on Q25 (noted there);
  `TransactionStatus` into `execution/transaction_status.rs` at `3cf1ec6` (33 importers, past the threshold by
  SETUP's provision, the 7th); `TimerKey` into `types/src/io/timer_key.rs`, beside `TimerEvent`, at `3433b15` (100
  importers, the 8th past the threshold, the 95% now resting on Parts B and C's commits; cargo fmt leaves files
  with over-long import lines unsorted -- stern's `ordered-imports` finds them; never `--write-baseline` over a
  reported offence: done once here, caught and undone before the commit); system-tests' four configuration
  records into a new folder `system-tests/src/config/` at `e7da3a4` (45 to 41 / 256 to 252) --
  `system-tests/src` holds 20 files, the `directory-file-count` limit, which counts files, not folders.
- **Slice 8's detail** (records and objects, a module at a time -- `Chosen-By: no-scaffolding`; started 23:51:38Z;
  full list `overhaul-scratch/records-and-objects.txt`). Three
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
  `ContextSnapshot` at `70df3dd` (seven commits, rows 128-134: 13 getters, `as_variant`, a `with_` per set field
  and `with_account`, `with_block`, `with_pending_transaction` for in-place inserts and pushes, with the context's
  own tests at `b440217`; production readers `3867552`, `2f65d20`; tests `3c58db8`, `d5048cc`, `a28e815`; the last
  validation tests with the fields private at `70df3dd`, `rule-offence`, 65 to 64 / 276 to 275 -- dry4rust
  re-records from the start folder with `--path .`, an absolute path rewriting every closure's name, and the
  baseline holds 203 groups, not the 200 a failing check reports suppressed; `replace_all` per field, then
  `grep -n "^ *\.blocks\b\|..."` for chains rustfmt split -- patterns ending in `$` are refused; `x.with_y(x.z())`
  moves before it borrows, so bind `x.z()` first; a `with_` replacing a collection holds what inserts did only
  over a helper whose collections start empty -- check it);
  `Block` at `ca372f1` (seven commits, rows 135-141 -- eight getters and a `with_` for each written field, `with_height`,
  `with_state_root`, `with_transactions`, `with_gas_limit`, `with_post_state_root`, `with_receipts_root` beside
  `with_parent_hash`, eight tests, with evm's, types' and recovery-ibft's sources and types' tests at `23c92f9`; the
  readers surface a compile layer at a time with the fields private -- the next layer's list in
  `overhaul-scratch/block-readers-next.txt`; node's sources and node-infra's storage at `618a5d2`, with
  `with_proposer` (`apply_proposal_commitments` keeps its `&mut Block`, which returning a value would turn into an
  everything-on `returned-mutations` move, so it assigns back a `with_` chain); recovery-ibft's and node-infra's
  tests at `65a4f5e` (dry4rust 203 to 202; `commit_certificate_for` out of its group of three until its node and
  validation twins move, Q35); node's tests with validation's sources at `5f1c775` (201 groups); system-tests'
  sources and tests and the trace driver at `fd46022` (200 groups); validation's single_node/ tests at `f18f095`
  (the 140th, all three stages); cluster/ and harness/ with the fields private at `ca372f1` (`rule-offence`, 64 to
  63 / 275 to 274; the anchors' near pair paid, 199 groups));
  left in the baseline: `NodeDiagnostics` (Q37, open: a constructor of 28), `IbftProtocol` (Q25) (node),
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
- **Commits:** 225 on the branch; 13 past the threshold (6 planned -- the 5th the merge, `8afa421`, the 6th the
  execution port's move, `4cfcce2` --, + `97225b7` for the dry4rust re-record, `1d27ff1` for the two states' 36
  accessors, `3cf1ec6` and `3433b15` for the transaction status's 33 importers and the timer key's 100, `2883602`
  for the slot discovery's 11, `a352577` for the Ed25519 scheme's 14, planned within by SETUP, `d7d2c45` for
  Faction's two implementations and their test files, Q43) -- 13 of 225 is 5.8%; the 95% now needs about 260
  commits, every one from here within it: count a move's files first, and
  shape the seams so a type is born where it ends; bug fixes
  6 (`eba863f`, `e1232ca`, `5013d0e`, `1e7ac29`, `643bfd5`, `8e1ca00`); the 150th (`7ffb333`) ran all three stages, Part A's end, the 160th (`6912269`) and
  the 170th (`2883602`), the 180th (`b31593c`), the 190th (`c885cf9`), the 200th (`7508819`), the 210th
  (`e04403c`) and the 220th (`e329250`); the 230th next
- **Tests:** 3,440 at the base, 3,816 now (stage 5 465); removed under constraint 6: 1 (`2e6d484`, Q34)
- **Offences:** 41 as configured, 195 everything on, shear 0; Grip 782.93 / Braintax 4,880.81 (0.1604 at `58844eb`, the tally port, node-infra 164.60 / 698.19 and
  node 324.94 / 2,355.94; 773.51 / 4,811.48, 0.1608 at `9b496e6`, recovery/control 78.14 / 397.87 with
  `PeerKnowledge`; 770.32 / 4,797.08 at `44a0b7a`,
  node-infra 162.07 / 675.84 with the fixed membership; 160.72 / 669.63 at `c885cf9`; node-faction 10.25 / 53.45 at `082c59c`;
  node-infra
  152.08 / 598.27 at `d170c7e`; types
  110.85 / 628.07 at `8afa421`, execution-types scored inside it, the totals' Grip unchanged; evm
  41.69 / 487.19 at `36c3d9e`; 39.77 / 486.59 at `e444f5b`; 40.01 / 487.75 at `6912269`; 40.16 / 490.52 at `ba4e857`; 39.45 / 492.86 at `89735b7`; 38.06 / 495.55 at `78c9a21`; 36.70 / 495.26 at `60b3373`, the calldata loop in a private helper; 35.64 / 499.52 at `bf1b812`; 36.15 / 508.72 at `a022bd1`; 36.69 / 517.12 at `bc9904f`; 37.41 / 522.15 at `0a38d3c`, the delegation layer's price, 34.29 / 498.16 at `3c21b53`;
  execution-types 38.62 / 102.22 at `618a5d2`; 37.67 / 99.93 and types 72.23 / 518.56 at `23c92f9`, the block's
  accessors;
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
  `cec0c5b` with the factory seam); dry4rust baseline 195 groups (199 at Part A's end; 196 to 193 at `73cb8eb`,
  six exact pairs unmatched and three near pairs recorded, Q33, and back to 196 at `b31593c`, the six re-keyed, Q35; two paid at `a022bd1`
  and one at `bf1b812`, the arithmetic copies; one re-formed at `68620c3`, Q35; two paid at `07f708e`, three paid and one re-keyed at `618894b`, two paid at
  `2e93617`), all matching (re-recorded at `cec0c5b`, Q32;
  one group added at `64f562b`, Q33; one added and one paid at `1121282`; one re-keyed and one added at `8230a8a`,
  Q35, each copy kept in scratch as `dry-baseline-before-cN.json`; one re-keyed at `97cd2da`; one grown by a member
  at `0d46800`, Q46; seven re-keyed and one near pair paid at `e04403c`, Q46, 196 to 195)
- **Questions:** 46 answered, 3 open (9, 25, 37). **Interventions:** 0. **Waits:** 0.

## The run so far

- **Started (UTC):** 2026-11-06T16:02:08Z; branch `unattended-overhaul` created from `8a775a2`.
- **Part 0:** 25 commits, 16:09 to 17:50.
- **Part A:** 125 commits, rows 26 to 150 (`03744d2` to `7ffb333`), 2026-11-06T17:50Z to 2026-11-07T05:14Z, against
  SETUP's about 75 (60 to 95); offences 483 to 41 as configured and 696 to 252 everything on; tests 3,519 to 3,670;
  8 commits past the threshold of 150. Its commits: `--fix` (`03744d2`); the harness: five tests back (`43ead85`), the build fallback behind a
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
  fields private (`4612b79`); the context snapshot's accessors (`b440217`), its readers (`3867552`, `2f65d20`,
  `3c58db8`, `d5048cc`, `a28e815`), its fields private (`70df3dd`); the block's accessors (`23c92f9`), its
  readers (`618a5d2`, `65a4f5e`, `5f1c775`, `fd46022`, `f18f095`), its fields private (`ca372f1`), the slice's
  last; record home: system-tests' wire types public (`72ebb5b`), the WAL field groups (`79526b2`), the context
  header, inputs' parts and bytecode step (`d74cdc1`), the episode state (`dcd2720`), the transaction status
  (`3cf1ec6`), the timer key (`3433b15`), system-tests' configs (`e7da3a4`), the slice's last; living documents:
  the gate documents (`520ee10`), the three resolved open points retired (`7ffb333`), Part A's last; bugs found on
  the way: the hard state's dropped transaction data (`e1232ca`, Q36).
- **Part B:** from row 151, 2026-11-07T05:33Z. The skipped rules: evm's `returned-mutations` -- the push's
  counter returned (`3c21b53`), the machine state (`0a38d3c`), push, dup and swap moved in (`bc9904f`), the
  arithmetic's first half through the word ALU (`a022bd1`), its second (`bf1b812`), the environment opcodes
  (`60b3373`), SLOAD and SSTORE (`78c9a21`), the memory dispatcher, MSTORE and MLOAD (`89735b7`), the memory
  window and its expansion (`ba4e857`), SHA3 with its expansion and word charge (`6912269`), SHA3's window and the
  word pop (`e444f5b`), the engine's working state (`36c3d9e`), the stand-down out (`5c6e9eb`), the slice's last.
  The execution seam: `execution-types` dissolved into `types` (`8afa421`), the documents after it (`de03fb2`),
  node's test context builders through one helper (`8ef334d`, Q39), the slot discovery port (`d170c7e`), its
  wiring -- `node` off evm (`142ce22`), the execution port into `node/src/traits` (`4cfcce2`), the
  TinyEvm slot discovery into evm (`2883602`), `node-contracts` with the contract's first two clauses (`3a96314`), its gas
  and mutation clauses (`a7bec37`), repeatability and the standard scenarios (`32ccbe1`), the engines running it
  (`c40ab03`), the evm boundary gate (`bd30c4f`), the documents (`c83fba2`), the seam's last. `node`'s own dependencies: the Ed25519 scheme into
  node-infra (`a352577`), the seam's one commit. Faction: what it answers through the port (`951d746`), node-infra's
  bootstrapper builder and its first six sites (`73cb8eb`), node's test helpers through it (`b31593c`), validation's tests through it
  (`f2f5e31`), node's vocabulary for the port (`a59c662`), `node-faction` with the command
  translation (`17fd986`), its outcome and transition translations (`4b8a7a1`), the factory into it
  (`c43c0f2`), the peer-discovery facet (`a2f5f0e`), the
  coordinator and bridge into node-faction (`d7d2c45`), the port in node's vocabulary (`d1246b3`), the event's, `node` off
  faction (`082c59c`), the fixed-membership coordinator (`c885cf9`), its other answers' tests (`356ecee`), the builder's
  choice of it (`44a0b7a`), the bootstrapping contract's check (`515d818`), its standard scenarios
  run by both coordinators (`bf5c717`), the documents (`a4f3033`), the seam's last. Beggar: the
  pruned-cluster join pinned (`187b65c`), `#[must_use]` on the state-deciding bools (`f2a36a8`), the peer knowledge
  (`9b496e6`), its observations' tests (`fcb922a`), consensus's reaction to a snapshot jump
  (`7508819`), the timers a snapshot apply left unarmed (`5013d0e`, a bug-fix), recovery's replays under
  their own kind (`1e7ac29`, a bug-fix), the last resort counted as blind (`643bfd5`, a bug-fix), the unavailable
  path's retry cancelled by the control layer (`a5e9b26`), the jump re-arming the layer's check (`4fbf853`), a pushed range pinned (`e248472`), the port's outcome must-use
  (`a8a1d57`), the factory port and its default (`835af20`, Q45), the factory's
  `Default` (`0d46800`, Q46), the dependencies taking the factory (`e04403c`), the documents and the
  narrowed recovery boundary (`29ef8e4`), three closed open points retired (`f3932b0`), two more and
  peer knowledge marked (`455df75`), `accept_peer_message` must-use (`52bc729`), the seam's last. The certifier:
  node-contracts' signature scheme contract (`0476b72`), run on every scheme the house builds (`1f3b0f3`), the
  documents (`b0756e2`), the seam's last. Consensus: a proposal after its NewView pinned (`11bfec3`), a held
  proposal requeued (`8e1ca00`, a bug-fix), the tally port (`e329250`), node-infra's tally behind it (`bd41a48`),
  the port's read (`97441de`, Q49 amended), the volatile state's tests through helpers (`1cd555a`), the wiring
  (`58844eb`), the dependencies' hand-over pinned (`6de5afa`).

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
| 8 | ~~records and objects, a module at a time~~ -- done at `ca372f1`, 49 commits; 3 left (Q25, Q37) | records-and-objects 51 | no-scaffolding (last) |
| 9 | ~~each record in the module of what uses it~~ -- done at `e7da3a4`, 7 commits; 1 left (Q25) | record-home 23 | no-scaffolding (last) |
| 10 | ~~living documents~~ -- done at `7ffb333`, 2 commits | -- | no-scaffolding |

Part B's, as SETUP lists them (*Part B: the restructuring*; to be chosen one at a time by *Choosing the next
slice*): the skipped rules, 20 to 30 commits -- ~~`returned-mutations` in `evm`~~ (done at `5c6e9eb`, 13
commits, `Chosen-By: risk`), `paired-test-file` in `validation`
and `system-tests` (each `<scenario>_scenario_tests.rs` paired with a `<scenario>_scenario.rs` in `src` that hands
its tests the cluster they start from), `tested-public-api` in `system-tests` (tests for the 20 functions no test
calls), the eight `single-implemented-type` entries split; then the seams, 75 to 100 commits, in the owner's
order -- ~~execution with `execution-types` dissolved into `types`~~ (done at `c83fba2`, 13 commits, `Chosen-By:
dependency`), ~~`node`'s own dependencies~~ (done at `a352577`, 1 commit, `Chosen-By: no-scaffolding`), ~~Faction~~
(done at `a4f3033`, 18 commits, `Chosen-By: owner-order`), ~~Beggar~~ (done at `52bc729`, 19 commits, `Chosen-By:
owner-order`), ~~the certifier~~ (done at `b0756e2`, 3 commits, `Chosen-By: owner-order`), consensus (in progress, `Chosen-By:
owner-order`), the observer split by concern, the gates, an ADR and the living documents.

Owed, for Part B's `tested-public-api` slice: `RuntimeTimerHarness::next_port` has no test (everything-on
`tested-public-api`); nor has `RuntimeTimerHarness::new`, which the tool stopped reporting at `3868a69` only because
it matches calls by name and arity (seven arguments, like some other `new` a test calls) -- test it all the same.

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
