# IBFT unattended overhaul experiment -- questions raised

Every question and open point the run raises, as it arises. None stops the run. Each is one of two things:

- **answered** -- on the fly, when the answer stays inside the plan's goals and constraints, a later commit can
  undo it, and the house has a clear default: the entry records the answer, the reasoning and what followed, and an
  unplanned commit that carries it names the entry as its `Trigger:`;
- **open** -- when answering needs what the run may not do (`SETUP.md`, *Questions raised*): the entry records the
  course the slice took meanwhile and the run's recommendation, and is delivered with the implementation.

A design question is left open and settled after the experiment. A security-relevant finding is never described
here: it is recorded privately, and its entry here reads only "a security-relevant finding, recorded privately".

After the run the owner takes each open entry -- implement, discuss, discard, postpone, or integrate into
`etheram-ibft`'s `OPEN_POINTS.md` -- and may overturn any answered one.

## Index

| # | raised (commit, step) | what | status | answer, or the course taken meanwhile |
|---|---|---|---|---|
| 1 | commit 15, Part 0 | slotgate's per-test timeout, 120 -> 160 seconds: a harness time bound moved | answered | the plan's own change, named here as *Reporting* asks; no wait's budget moves |
| 2 | commit 16, Part 0 | which kind carries the cluster harness's richer wait-failure message, which the plan asks for and no kind fits exactly | answered | `refactoring`: no budget, poll, default or outcome moves, only the text a failing wait panics with; the new message type is tested |
| 3 | commit 17, Part 0 | which kind carries the single-node harnesses' ports moving into the slot's range | answered | `refactoring`: a test's ports were arbitrary and stay arbitrary; under slotgate they now come from the slot's range, as the cluster harness's already did; outside it, the same port-0 fallback |
| 4 | commit 18, Part 0 | five conflict tests, their blocks now valid, still pass with conflict detection disabled | answered | each gets the setup or assertion that only detection explains, in `tests` commits of their own after this one; none failed with detection on, so no bug |
| 6 | commit 31, Part A | the cluster harness's eventual waits fail on 60 seconds without progress, beside their 120-second budget: a time bound added | answered | the plan's own change, `OPEN_POINTS.md`'s preferred option 1; the budget kept as the backstop below slotgate's kill; `refactoring`, as entry 2 |
| 7 | commit 34, Part A | the logging observer's 74 one-line methods, a duplicate group the dry4rust baseline records, are reshaped by the clock they now pass, and the gate refuses the reshaped group | answered | re-record the dry4rust baseline: the same group under its new fingerprint, and the two groups paid since the base dropped -- 217 to 215, none added |
| 5 | commit 27, Part A | the five harness tests restored lower a count -- `tested-public-api`, stood down in `system-tests` as configured -- so no `tests` commit can carry them | answered | `rule-offence`, `Rule: tested-public-api`: calling an untested public function from a test is the rule's own fix; the count falls with everything on and cannot move as configured, where the rule is stood down |
| 8 | commit 41, Part A | the two gRPC buses' wake tests timed a wait to learn it was notified; they now assert the notification the wait reports | answered | the same contract -- the bus notified its signal -- stated directly; the wake signal's own promptness stays held by its timed tests (entry 9) |
| 9 | commit 41, Part A | the wake signal's six tests time a timed wait -- promptness and duration are what they check -- and no timeless rewrite says as much | open | they stay in the stern baseline, `timeless-tests`; recommended: accept them beside decision 2's exception, the one place the subject under test is a timed wait |
| 10 | commit 42, Part A | three test waits change shape as they move onto `PollingWait`: a count of polls becomes a deadline, two fixed 40 ms sleeps become a wait for the server, forty connect attempts become one | answered | every ceiling kept or replaced by the event it stood for; no assertion moves |
| 11 | commit 43, Part A | five fixed delays in single-node tests -- 20, 100, 120 (three times) and 250 ms -- have no event to wait for in their place, and move onto the sleeper port unchanged | answered | the tests compose real time and sleep through `PollingWait::sleep`, as long as before; the rule stops seeing them, the delay does not go -- each is named, so a later change can replace it with the event it stands for |
| 12 | commit 44, Part A | the cluster tests' last four waits move onto `PollingWait`: two deadline loops now take their last look just after the deadline rather than just before, and a 3 s partition settle goes through the sleeper | answered | each window grows by at most one poll -- 200 ms and 100 ms in 60 s; the settle is as long as before, as entry 11 |
| 13 | commit 45, Part A | six factories that cannot fail returned `Result<_, String>`; returning what they build leaves their tests' `is_ok()` with nothing to check | answered | five tests now check what was built; the observer's, whose observer returns nothing a test can read, hands each observer an event -- its `is_ok()` became the signature, mapped here; the two client-interface factories' recorded duplicate is re-recorded, as entry 7 |
| 14 | commit 47, Part A | two assertions change with the transport's failure type: the transport-choosing outgoing factory cannot fail either, and the node failure's `Debug` test named a `String` the transport no longer returns | answered | the factory's test checks a send instead of `is_ok()`, as entry 13; the `Debug` test checks the same quoting on a transport failure that exists -- the same contract |
| 15 | commit 48, Part A | five assertions change with partitioning's failure type: four read a failure's text through `to_string()`, one `is_err()` now names the failure | answered | the four check the same words as before; the fifth says more, as constraint 6 asks of an `is_err()` |
| 16 | commit 50, Part A | the client-interface codec's four refusal tests checked `is_err()` alone | answered | each now names its failure -- the address length, the value and its parse error, the request and the response that did not match it -- the strengthening constraint 6 asks for |
| 17 | commit 51, Part A | `NodeFailure::Storage` now carries the sled store's failure, so the node failure's test of a part's words builds one rather than a `String` | answered | the same contract, as entry 14: the node prints the part's own words, now those of a directory that could not be made |
| 18 | commit 52, Part A | five storage-codec assertions change with its failure type: four compared the refusal's text, one checked `is_err()` | answered | the four compare the failure itself, whose text its own tests pin; the fifth names it; two recorded duplicate pairs in the codec are removed, not re-recorded |
| 19 | commit 54, Part A | the cluster config's eight refusal tests compared the `String` it failed with to a text | answered | each compares the failure's text, through `to_string()`, to the same text -- the same words checked the same way |
| 20 | commit 55, Part A | xtask's gate sequence fails with a `StageFailure`; its two failure tests compared a `String` | answered | one compares the same text through `to_string()`, the other the failure's three parts, whose text the new type's own test pins -- the same claim both |
| 21 | commit 56, Part A | the gate tests' 32 assertions on a gate's failure compared the `String` it failed with, ahead of the gates' failure type | answered | each compares the failure's text, through `to_string()`, to the same words -- the same check of a `String` today and of the failure type, whose text is the old one |
| 22 | commit 59, Part A | `Gate::run` fails with a `GateFailure`: the fake gate fails with one, not with free text, and three assertions name what it fails with | answered | each says what it said of the text it was given, now of a failure a gate gives; one expected text grows by the exit code the fake's failure carries |
| 23 | commit 60, Part A | the system runner's launch-failure test read the `String` it failed with; it fails with a `LaunchFailure` | answered | the same check -- the failure's text starts "failed to launch" -- read through `to_string()` |
| 24 | commit 62, Part A | the CRAP report parser's three failure tests read the `String` it failed with; it fails with a `CrapReportFailure` | answered | each reads the same words through `to_string()`; the new type's tests pin the whole text |
| 25 | after commit 63, Part A, slice 3 | one file per implemented type: `IbftProtocol` (13 files, 4,857 lines) and `RecoveryControl` (5 files) -- merge every `impl` into the type's file, or split the type into types of their own? | open | the slice waits, both offences stay in the baseline, and the run goes on with slice 4; recommended: split, a type per concern, with the protocol's split taken into Part B's seams |
| 26 | commit 69, Part A | a block's two commitment roots become a record, which reshapes three copies of `build_block_with_commitments` the dry4rust baseline records as one exact group | answered | re-recorded as entries 7 and 13 did, checked fingerprint by fingerprint: the group out under `f74317da987ed69e` and in under `40298922b91b8ae9`, three paid groups dropped, none added -- 213 to 210 |
| 27 | commit 71, Part A | `summarize_receipts` returns a `ReceiptSummary` where it returned four counts as a tuple; its two tests compared the tuple | answered | each compares the record with the same four counts, now named -- the same claim and the same check |
| 28 | commit 73, Part A | the EVM's `sha3_range` returns a `Sha3Window` where it returned offset, size and end as a tuple; one test compared the tuple | answered | it compares the record with the same three values, now named -- the same claim and the same check |
| 29 | commit 75, Part A | the cluster harness's waits return `NodeHeight` where they returned `(node, height)` pairs, which their log lines and one wait's panic text print, and one test read the pairs by position | answered | the record prints as the pair did, pinned by a new test, so no line and no panic text changes; the test reads the same values by name |
| 30 | commit 79, Part A | the EVM engine's `pub use` re-exports of the opcodes and `TinyEvmOpcode` -- a shim CLAUDE.md forbids -- counted as a second reader of ten opcodes commit 78 moved to a constants file; without the shim each has one reader | answered | nine go private into `tiny_evm_opcode.rs`, which alone matches on them, and `OPCODE_PUSH2` into the one test that assembles it, as stern's corrections say; no count moves, no test changes but for its imports |
| 31 | commit 81, Part A | the diagnostic beacon's field table, `DIAGNOSTIC_FIELDS`: in a constants file, the only thing production reads it for -- `DIAGNOSTIC_MAX_BYTES` -- does not count, and stern's correction is to move the table into its test | answered | it stays beside `NodeDiagnostics`, its offence in the baseline as it was; the three byte bounds move to `diagnostic_constants.rs` and are paid |
| 32 | commit 92, Part A | the bootstrapper's coordinator is built from what only peer discovery knows, and stern refuses anything but a parameter assigned to it, a factory's call included; the change reshapes seven recorded dry4rust groups | answered | `init` takes the coordinator; the bootstrapper's injected factory builds it, the protocol installs it; the dry4rust baseline re-recorded as entries 7, 13 and 26 did -- seven groups re-keyed, one paid group dropped, none added, 210 to 209 |
| 33 | commit 96, Part A | `records-and-objects`' correction builds a getter-only config's `Default` by literal, and dry4rust pairs two such `Default`s -- the bootstrapping and the consensus timing configs, three fields each -- as near duplicates: alike in shape, with nothing to share | answered | the pair is recorded in the dry4rust baseline under `c2faba878ab6d286`, named here, every other group as it was -- 209 to 210 -- as the stern baseline takes an added offence named in `QUESTIONS.md`; a later commit can drop it |
| 34 | commit 97, Part A | `NewViewMessageFields` loses its five getters, and `behaviour-tests` refuses one of the five getter tests, `view_change_proofs_returns_the_proofs_the_new_view_carries`, as a test of the record alone | answered | removed under constraint 6's licence, mapped to `change_senders_collects_one_entry_per_distinct_proof_sender`, which checks that what the validator reads from the proofs is the proofs the fields were built with; the other four, which the gate does not refuse, read the fields |
| 35 | commit 105, Part A | the in-flight slot's fields go private, and reading them through getters reshapes a recorded dry4rust pair, the recovery state's `fail_request` and `refuse_request`; the slot's new `request()` pairs with `ContextSnapshot::header` | answered | the pair re-keyed as entries 7, 13, 26 and 32 did, `b4e7991104edfc28` out and `0df28f4a7502fa38` in, the same two members; the literal pair recorded under entry 33 as `f280cde3a9b5c133` -- 210 to 211 |
| 36 | commit 115, Part A | the hard state writes a pending block's transactions without their data, in both its layouts, and reads them back empty -- harmless while the block hash left data out, a block that no longer hashes as its certificate since the hash covers it | answered | a bug, fixed test-first in a `bug-fix` commit of its own: both encoders write the data, the cursor reads it, and the layout bytes move on as the house moved them when blocks gained a parent hash -- state in the old layout refused, not misread |
| 37 | before commit 120, Part A | `NodeDiagnostics` has one associated `const fn` and 28 public fields, nearly all `u32`, built in one place: its correction wants a constructor, and a positional one of 28 would be past every precedent -- the house's largest allowed constructor takes twelve -- and swaps two counters without a compiler noticing | open | the type stays as it is, its offence in the baseline, and the slice goes on with the others; recommended: group the fields into the records the type's own comments already name -- chain position, chain progress, peers, the recovery slot, recovery counters, the protocol's census -- each a record of a few fields, and give `NodeDiagnostics` a constructor of the sequence and the six records |
| 38 | commit 151, Part B, the skipped rules | the EVM interpreter's operations "become functions that return what they change" (`SETUP.md`): threaded by value, a dispatcher that declines an opcode consumes the stack it was handed; as the machine's own methods, their tests can preset and read nothing | answered | the machine's changing parts become one object, `TinyEvmMachineState`, and the operations its `&mut self` methods -- the house's own shape, the storage-slot analyzer's -- returning a value where they compute one; the counter stays with the machine, the push returning where it goes next; a `bool` and an out-parameter merged into one returned `Option` take their tests' two assertions into one, each mapped to itself |
| 39 | before commit 166, Part B, the execution seam | `node` depends on `evm` in production: its storage access plan asks evm's `TinyEvmStorageSlotAnalyzer` which contract storage a transaction touches, so moving `ExecutionEngine` into `node` -- evm depending on `node` for it -- would close a cycle | answered | the analysis becomes a port of its own, `StorageSlotDiscovery`, in a new `node/src/traits`, injected through the constructors of the node and the snapshot context builder; evm implements it with today's analyzer, so every engine keeps today's plan; `node` keeps evm as a dev-dependency only, and the execution port follows into the same folder |
| 40 | commit 170, Part B, the execution seam | `SETUP.md` keeps "`NoOpExecutionEngine` the default in `node-infra`", but both builders default to evm's `ValueTransferEngine`, so node-infra depends on evm in production | answered | read as the port's default implementation staying in node-infra; the builders' defaults stay evm's value transfer -- changing them changes what every default-built node executes; node-infra is the assembly, not a plug-in, so step 8's gate holds |
| 41 | commit 171, Part B, the execution seam | the contract suite is test code with a `src`, a manifest and gate lists, which `tests` does not allow; and the no-op reports no results, which a one-result-per-transaction contract would refuse | answered | `refactoring`, `node-contracts/src` counting as production as `validation/src` does and a crate or dev-dependency edge being structure; the contract asks only what the protocol relies on and all three engines do, fewer results than transactions allowed |
| 42 | before commit 179, Part B, the Faction seam | the bootstrapping port, the readiness event and three implementations speak `faction`'s types; 15 files build a bootstrapper with Faction's factory | answered | node mirrors the vocabulary that crosses the port, names and all, so the readiness log is unchanged; a `node-faction` plug-in holds the translation, the coordinator wrapper, the bridge and the factory; assemblies go through node-infra's `BootstrapperBuilder` first, the fixed-membership default its other choice |
| 43 | commit 186, Part B, the Faction seam | the bridge's tests need an observer node-faction can build, 360 lines behind the full observer; the bridge's and the coordinator's moves each pass 360 lines, their test files counted twice | answered | the peer-discovery facet first, holding the readiness event, `ProtocolObserver` its subtrait; the two implementations then move in one commit past the threshold, the seam's one unsplittable move |
| 44 | after commit 205, Part B, the Beggar seam | which trait is Beggar's port, where its vocabulary lives, and who builds the default, which `node`'s assembly builds today | answered | the port is today's call site, `RecoveryProcess`, which already admits a pushed transfer (pinned, not changed); its vocabulary stays in `recovery-control` until gate 8, after the observer split; `node` takes a `RecoveryProcessFactory` it hands its timing, node-infra's `RecoveryControlFactory` the default |
| 45 | commit 208, Part B, the Beggar seam | entry 44 put the factory port in `node/src/traits`, and stage 1's recovery boundary refuses `RecoveryProcess` in `node/src` outside the assembly | answered | the gate stands; the trait moves to `node/src/ibft/assembly/`, beside the dependencies that take it, and the rest of entry 44 is unchanged |
| 46 | before commit 209, Part B, the Beggar seam | the dependencies taking the factory change 30 sites in 11 files, re-key the seven pairs the dry4rust baseline records for node's and validation's dependency helpers and pay an eighth, near the threshold whole; the factory's default, the one-line form, joins a recorded group of seven `empty_state` helpers | answered | two commits: the default first, its group recorded with an eighth member as entry 33 did, then the wiring with the seven pairs re-keyed as entry 35 did |
| 47 | after commit 214, Part B, the certifier's seam | the seam SETUP names -- `node`'s `SignatureScheme` port, `Ed25519SignatureScheme` behind it in `node-infra`, every key derived from the peer id -- already stands; which of the seam's pieces are left, and whether the port takes a height | answered | the port is today's, with no height (decision 9; a height comes with the key variants, decision 5); the protocol boundary is characterized already; what is left is a contract suite every scheme runs -- what one member signs, every member accepts as that member's, for messages and certificates both -- and the documents |
| 48 | after commit 217, Part B, the consensus seam | the one recorded bug in consensus's code: a buffered proposal is replayed under the context of the message that released it, without the accounts its transactions touch; SETUP fixes it by re-queuing, so the runtime builds it its own, and no action re-queues today | answered | a new `Action::RequeueMessage`, which the partitioner routes to the node's own work; `EtheramNode` serves re-queued messages before its event source; both buffers re-queue a held proposal and replay votes in place as now; the replay report gains a `Requeued` blocker; three consumer breaks, named in the commit |
| 49 | after commit 219, Part B, the consensus seam | the port for "the quorum, the tallies and the decision", in the step shape (decision 9), when consensus is `IbftProtocol`'s own methods and its tallies two `VoteTracker`s inside the volatile state | answered | `VoteTally::step(TallyInput) -> TallyOutput`, one tally per phase, holding its quorum and answering whether a key has reached it; the volatile state the adapter, its methods unchanged, so consensus's code is not touched; the default `QuorumTally` in node-infra over today's tracker, built by a `VoteTallyFactory` the dependencies take; view changes stay evidence; a contract suite |
| 50 | after commit 231, Part B, the observer | step 7 splits `ProtocolObserver` -- 77 declared, one inherited -- into five concerns under a composite with no body (decision 10); which facet each method joins, when the diagnostic beacon fits none of the five, and in what order | answered | six facets in `node/src/observer/`: peer discovery widened, consensus with message admission, the buffers, the timers' dispatch and the pool, recovery with snapshot, retention, the refusals, and the beacon alone; `ProtocolObserver` the composite, an empty `impl` per implementor; one facet a commit from the trait's tail, consensus last; then the bootstrapper, the recovery dispatch and the reporting output each narrowed to its facet; then the documents |
| 51 | after commit 241, Part B, the skipped rules | `paired-test-file` reports 100 test files in validation and system-tests; SETUP pairs each `<scenario>_scenario_tests.rs` with a `<scenario>_scenario.rs`, but renaming the scenario tests counts about 30,000 lines twice under `--no-renames`, far past the threshold | answered | a scenario source takes the test file's own stem, no test renamed, holding the cluster or node its tests start from; a test of a source at another path moves to that path; `rule-offence` commits, the moves first, then the sources a folder at a time; the stand-down out at zero |
| 52 | commit 267, Part B, the skipped rules | entry 51's sources make the harness calls a scenario's tests made, and `tested-public-api`, stood down in system-tests, matches a call by name and arity: `with_scheduled_validator_update` lost its only direct caller, 17 to 18, and other calls only scenario setups make will follow | answered | the sources stay as entry 51 has them, since every such call is still driven by the same test one call deeper and no visibility changes; each commit names what the rule stops seeing; the `tested-public-api` slice, next, takes them with the 17 |

## Interventions

Any message from the owner during the run -- a question, an instruction, a "continue" alike. The first question's
"unattended" is judged on this table: empty, it holds.

| time (UTC) | the owner's words | what followed |
|---|---|---|

## Entries

Each entry: its number and title; where it arose -- the commit, the part and step; what it is and what made it
visible; its status; for an answered entry the answer, the reasoning and the commit that followed, for an open one
the course the slice took and the run's recommendation; after the run, the owner's decision.

### 1. slotgate's per-test timeout goes from 120 to 160 seconds

- **Raised:** commit 15, Part 0, the harness waits.
- **What:** a time bound of the cluster suite moves. `SETUP.md`, *Reporting*: "A harness change that moves a time
  bound -- a wait's ceiling, a poll's interval, a timeout -- is named in `QUESTIONS.md`, whatever its kind."
- **Status:** answered.
- **Answer:** the change is Part 0's own (*a harness wait that gives up says so, before slotgate's kill*). At 120 the
  per-test timeout equals the harness's longest wait, `CLUSTER_EVENTUAL_TIMEOUT`, and a test also spends time
  spawning its cluster before any wait starts, so a stalled wait is killed before it can fail and its message never
  reaches the logs. At 160 a wait always fails first, with its message, and a kill means a test hung outside every
  wait. No wait's own budget moves -- 15 seconds for startup, 120 for the others -- and the parallelism stays at 8.
  A passing test takes as long as before; only a hung one is killed 40 seconds later.
- **Commit:** the one this entry is raised at, `support`.

### 2. The kind of the cluster harness's richer wait-failure message

- **Raised:** commit 16, Part 0, the harness waits.
- **What:** Part 0 asks that a harness wait that gives up fail "naming the wait, its context and target, each node's
  last height, the time elapsed and the last progress seen". The six eventual waits of `cluster_process_harness.rs`
  already panic with their context, target and observed heights; the change adds the wait's name, the time elapsed,
  each node's height by id, and when the wait last saw anything change. `system-tests/src` counts as production for
  the kinds, so `support` does not fit; `bug-fix` needs a test of the commit that compiles against the parent and
  fails there, and a failing wait can only be watched after its 120-second budget, through no seam the parent has.
- **Status:** answered.
- **Answer:** `refactoring`. What every passing test sees is unchanged: no wait's budget, poll interval, condition,
  return value or order of gRPC calls moves -- the observation is recorded from values each wait already holds, and
  the heights a block wait names are read only once it has given up. What changes is the text a failing wait panics
  with, which no test reads (none matches on it) and which is the harness's, not the node's: the node's logs, the
  defaults, builders' choices and numbering the `refactoring` rule protects are untouched. The message is built by a
  new `WaitProgress` in `system-tests/src`, told the time rather than reading it, so it adds no time read and its
  format is pinned by three tests. The commit touches the cluster harness, so stage 5 runs three times.
- **Commit:** the one this entry is raised at.

### 3. The kind of the single-node harnesses' ports moving into the slot's range

- **Raised:** commit 17, Part 0, the single-node ports.
- **What:** `test_config.rs` and `runtime_timer_harness.rs` bound port 0, read the port and released it, and so
  did the cluster harness's `next_port`, which 30 test call sites reach: a port taken outside slotgate's ranges,
  which an outgoing connection of any node on the machine can take in between. Part 0 moves all three to
  `ClusterProcessHarness::allocate_port`, which the cluster configuration already uses: a cursor over the slot's
  `PORT_RANGE_BASE`/`PORT_RANGE_COUNT`, and outside slotgate the same port-0 fallback. As with entry 2, the change is
  the harness's, under `system-tests/src`, and no test can be shown red on the parent for it -- the race it removes
  is one no run has been seen to lose.
- **Status:** answered.
- **Answer:** `refactoring`. The port a test gets was arbitrary before and is arbitrary after; no node, default,
  numbering or log line changes, and no test's assertions move -- the 30 call sites change the name they call and
  nothing else. The cluster harness's `next_port` goes, its callers calling `allocate_port`. 
  `RuntimeTimerHarness::next_port` stays, its body now `ClusterProcessHarness::allocate_port()`: no test calls it, so
  with every rule on it is a `tested-public-api` offence, and removing it here would lower that count and make the
  commit a `rule-offence` mixed into a `refactoring` -- measured, 696 to 695, and put back. It is left for
  `tested-public-api`'s slice. Touching the cluster harness, stage 5 runs three times.
- **Commit:** the one this entry is raised at.

### 4. Five conflict tests still pass with conflict detection disabled

- **Raised:** commit 18, Part 0, the conflict-detection tests (`OPEN_POINTS.md`, *Three conflict-detection tests
  pass with blocks that are invalid anyway*).
- **What:** commit 18 builds every conflicting proposal of `validation/tests/cluster/faults/
  ibft_cluster_malicious_block_tests.rs` on a block valid but for the conflict -- `funded_block` over the funded
  genesis, as the test corrected on 2026-10-10 already did --, the round-one blocks the same, so that detection, not
  validation, meets them: `trackable_pre_prepare` only tracks a valid block. Every test passes with detection on,
  so none points at a bug. Then each was run with detection disabled, in the working tree and not committed --
  `VolatileState::record_pre_prepare_observation_and_check_conflict` returning `false` at once. Five fail, as they
  should: `malicious_round_then_view_change_allows_next_honest_leader_progress`,
  `malicious_sender_prepare_is_ignored_after_conflict_marking`,
  `conflicting_pre_prepare_from_leader_same_height_round_is_ignored_by_follower`,
  `duplicate_conflicting_pre_prepare_same_sequence_is_idempotent` and
  `conflicting_pre_prepare_flood_does_not_reach_finalize`. Five still pass:
  `conflict_in_round_zero_then_honest_round_one_same_leader_block_finalizes`,
  `conflict_recovery_then_next_height_honest_path_still_progresses`,
  `conflicted_node_stalls_while_other_nodes_finalize`,
  `malicious_sender_cannot_help_round_one_finalize_after_timeout` and
  `malicious_sender_view_change_does_not_unlock_round_one_pre_prepare`.
- **Status:** answered.
- **Answer:** as Part 0 plans, a clear fix for each of the five, landed as `tests` commits of their own after this
  one, no assertion weakened: the setup that makes detection's effect visible -- a vote only the malicious sender
  could supply, so that ignoring it decides the outcome -- or an assertion only detection explains, such as the
  prepare node 1 must not broadcast for the conflicting block, read from what it queued for its peers. Each fix is
  run with detection disabled too, and must then fail. The commits are listed here as they land.
- **Commits:** 18 (the blocks made valid); then, one fix each, every one seen failing with detection disabled:
  19 `conflicted_node_stalls_while_other_nodes_finalize` -- `drain_all` had run node 1 before nodes 2 and 3 voted,
  and node 0's votes were refused as replays, so nodes 2 and 3 now hand it their votes directly; 20
  `conflict_in_round_zero_...` and 21 `conflict_recovery_...` -- honest round-zero prepares for the first block,
  which without detection lock node 1 on it; 22 `malicious_sender_cannot_help_...` -- the honest view changes that
  start round one, so node 0's votes are the missing ones; 23 `malicious_sender_view_change_does_not_unlock_...` --
  an honest view change beside node 0's, a round-one timeout, and no view change queued: the one assertion added,
  the height assertion kept.

### 5. Restored tests that pay an offence: which kind

- **Raised:** commit 27, Part A, the harness slice (`OPEN_POINTS.md`, *REMOVED: the harness helper tests, and the
  coverage with them*).
- **What:** the five passing tests deleted on 2026-08-22 come back -- `allocate_port`'s two,
  `node_process_binary_path_prefers_an_override_that_exists` and `read_stderr_to_string`'s two, their child
  processes now `cargo` itself rather than Windows' `cmd`. They change only `system-tests/tests`, which reads as a
  `tests` commit, but one of the functions they call had no test: with every rule on, `tested-public-api` falls from
  572 to 571, and a `tests` commit moves no count.
- **Status:** answered.
- **Answer:** `rule-offence`, `Rule: tested-public-api`. Calling the untested function from a test is the rule's
  own fix, the one `CLAUDE.md` asks for. The rule is stood down in `system-tests` as configured, so that count
  cannot move there; with everything on it falls by one and no other count rises.
- **Commit:** 27.

### 6. The cluster harness's waits fail on stalled progress

- **Raised:** commit 31, Part A, the harness slice (`OPEN_POINTS.md`, *The retention tests race a deadline they
  barely beat*, option 1).
- **What:** a time bound of the harness is added: `CLUSTER_STALL_TIMEOUT`, 60 seconds. The five waits that wait for
  something to happen -- nodes at a height, every node at a height, convergence, one block at a height, every block
  found -- now also give up once nothing they observe has changed for that long. `wait_until_heights_settle`, which
  waits for nothing to change, keeps its budget alone.
- **Status:** answered.
- **Answer:** the plan's own change ("waits that fail on stalled progress rather than elapsed time"). A stopped
  cluster now fails in 60 seconds rather than 120, with the message commit 16 gave it, its last progress showing it
  stopped; a slow one keeps changing and runs on. The 120-second budget stays beside it, not raised: Part 0's
  "each wait keeps its budget" and slotgate's 160-second kill above it mean a kill still says a test hung outside
  every wait. So the retention tests' race is not ended by this commit -- a cluster still progressing at 120 seconds
  still fails -- but it now reads as slow, not stopped. 60 is half the budget, chosen over 30 so that no wait that
  passes today -- the longest passing tests run 25 to 33 seconds in all -- can stall into a failure. Kind
  `refactoring`, for entry 2's reasons: no passing test's outcome moves, and the change is the harness's.
- **Commit:** 31.

### 7. A recorded duplicate reshaped: the logging observer's methods

- **Raised:** commit 34, Part A, time behind ports -- the first attempt, refused by dry4rust in stage 2.
- **What:** `LoggingObserver` implements `ProtocolObserver`'s 74 methods, each one `log_event(...)` call: an exact
  duplicate group of 74 members, recorded in the dry4rust baseline at the base (`fc9ab25929438ad0`). The wall clock
  the observer now hands `log_event` changes every one of those bodies alike, so the group's fingerprint changes
  (`c3d5152558f3f893`) and the gate, which matches by fingerprint, reports it as new. `SETUP.md` names the trap:
  "remove the duplication, or leave the bodies byte-identical". Neither is open here -- the duplication is a trait's
  74 methods, and a body that does not name the clock cannot be handed one without a global.
- **Status:** answered.
- **Answer:** re-record the dry4rust baseline (`cargo dry4rust baseline --path . --exclude-tests --min-lines 5`),
  the move stern4rust's baseline makes when a file moves and re-keys its entries. Checked fingerprint by fingerprint:
  the observer's group goes out under its old fingerprint and in under its new one, 74 members both times; the
  only other changes are the two groups this branch has paid -- xtask's four `scratch_root` helpers (commit 32) and
  the three `next_port` helpers (commit 17) -- which go. 217 groups to 215, none added.
- **Commit:** 34.

### 8. The gRPC buses' wake tests assert the notification, not the time it saved

- **Raised:** commit 41, Part A, time behind ports and timeless tests.
- **What:** `new_with_wake_notifies_the_signal_when_a_message_is_enqueued` (the transport bus) and
  `new_with_wake_notifies_the_signal_when_a_request_is_enqueued` (the client-interface bus) learned that the bus had
  notified its wake signal by timing a wait on it: `wait_timeout(5 s)` had to return in under a second. Reading
  `Instant::now()` for it was a `timeless-tests` offence each. `WakeSignal::wait_timeout` now reports whether it
  returned on a notification, and both assertions read that instead: `assert!(wake.wait_timeout(5 s))`.
- **Status:** answered.
- **Answer:** an assertion changed, reported as constraint 6 asks, and judged not to say less. Each test's subject is
  the bus, and its contract is that an enqueue notifies the signal. The old form fails when no notification came --
  the wait runs its five seconds -- and so does the new; the new also names the reason the wait ended, where the old
  inferred it from the time. The one case only the old form caught, a notification the signal took more than a
  second to deliver, is the wake signal's own contract, which its timed tests keep (entry 9). The report comes with
  two timeless tests of its own: `wait_timeout_after_a_notify_returns_true`,
  `wait_timeout_without_a_notify_returns_false`.
- **Commit:** 41.

### 9. The wake signal's tests time a timed wait

- **Raised:** commit 41, Part A, time behind ports and timeless tests.
- **What:** `wake_signal_tests.rs`'s six tests each read `Instant::now()` -- six `timeless-tests` offences -- and
  what they read it for is the wake signal's own contract: that a pending or arriving notification ends a wait at
  once (`notify_before_wait_causes_immediate_return`, `notify_wakes_waiting_thread_immediately`,
  `clone_shares_signal_state`, the first three waits of `multiple_notifies_before_wait_consumes_one_notification_per_wait`),
  and that a wait with nothing pending lasts its timeout (`wait_timeout_returns_after_timeout_when_not_notified`,
  `wait_timeout_clears_flag_after_return`, that test's fourth wait). The signal is a `Condvar` with a counter; its
  waiting is the standard library's, and no clock the test drives reaches it. The notification the wait now reports
  (entry 8) proves why a wait ended, but not that it ended promptly, nor that it lasted. Each timeless rewrite
  therefore says less, and constraint 6 rules that out for paying an offence: "an offence left in the baseline is
  always another way".
- **Status:** open -- `SETUP.md`'s goal for Part B leaves nothing baselined but the cluster harness's time reads,
  and these six cannot leave without an assertion saying less.
- **Course taken:** the six stay as they are, in the stern baseline.
- **Recommendation:** accept them beside decision 2's exception: as with the cluster harness, what they measure is
  time itself. The alternative is to put the signal's waiting behind a port whose double reports the timeout it was
  handed. That would test the plumbing around the `Condvar`, and leave the waiting itself, today's subject, with
  no test.

### 10. Three test waits change shape on the way to `PollingWait`

- **Raised:** commit 42, Part A, time behind ports and timeless tests.
- **What:** the gRPC transport, client-interface and sled tests polled with `Instant::now()` and `thread::sleep`
  of their own -- seven `timeless-tests` offences. Their loops now go through `PollingWait::until`, composed with
  real time by the tests (`real_time_wait()`), the way the harnesses' waits already do. Four keep their interval
  and their ceiling exactly. Three change shape, and *Reporting* asks that a moved time bound be named:
  - `wait_for_message(retries, interval)` polled `retries` times, sleeping after each, the last sleep included.
    It now polls every `interval` until `retries × interval` has passed, so the ceiling is the same, and the last
    look comes at the ceiling instead of a sleep that looks at nothing.
  - two tests -- a remote message, a remote recovery message -- slept a fixed 40 ms before sending, so that the
    receiver's server, started without waiting, would be listening. They now wait for exactly that: until a TCP
    connection to it succeeds, every 10 ms, up to 2 s, the client-interface bus's own readiness bound. The
    neighbouring tests that send with no wait at all still cover the race this avoids.
  - the client-interface tests' `connect_client` tried 40 times, 10 ms apart. The bus they connect to does not
    return from its constructor until its server answers a TCP connection, so it now tries once and fails at
    once if that ever breaks.
- **Status:** answered.
- **Answer:** no assertion changes, and no test is added or lost. Each ceiling is kept or replaced by the event it
  stood in for, and a failing wait still fails. The two recorded near-duplicate `connect_client` helpers stop
  matching, and dry4rust's baseline matches 213 of its 215 groups.
- **Commit:** 42.

### 11. Fixed delays in single-node tests go through the sleeper, unchanged

- **Raised:** commit 43, Part A, time behind ports and timeless tests.
- **What:** five single-node tests sleep a fixed time with `thread::sleep`, `timeless-tests` offences: 120 ms after
  spawning the node in each of the three `client_interface_tests`, 100 ms after spawning it in `shutdown_tests`,
  20 ms between steps in `discovery_handoff_tests`, and 250 ms in
  `single_node_default_topology_does_not_report_committed_height_after_startup_progress`, where the wait is the
  test: a node that cannot commit is given the time it would have needed. None of the first five stands in for
  an event a test could wait on as readily as commit 42's two did. Behind three of them the client connect
  retries for 1.2 s anyway, and behind the shutdown one the process reads a piped stdin. The 250 ms window has no
  event at all.
- **Status:** answered.
- **Answer:** the tests compose real time (`real_time_wait()`) and sleep through `PollingWait::sleep`, each delay as
  long as before; the polls beside them move onto `PollingWait::until`, and the async connect retry onto the
  `Pause` port's `TokioPause`, with their intervals and ceilings unchanged. Plainly: the rule stops seeing these
  delays, and the delays stay. That is the plan's model -- time reaches what uses it through ports -- applied to the
  tests' own waits. What it buys is that every remaining delay is named in one place. Recommended after the run:
  replace the three 120 ms sleeps and the 100 ms one with a wait for the node's client port to answer, which
  would shorten them, and keep the 250 ms window, which is the test's subject.
- **Commit:** 43.

### 12. The cluster tests' last four waits on `PollingWait`

- **Raised:** commit 44, Part A, time behind ports and timeless tests.
- **What:** four cluster tests read `Instant::now()` or slept, the last four `timeless-tests` offences outside the
  wake signal's:
  - `no_committed_block_carries_more_transactions_than_the_fleet_allows` polled every 200 ms while 60 s had not
    passed, and `wait_until_the_transfer_lands` every 100 ms while 60 s had not passed. Both checked the deadline
    before each look. `PollingWait::until` looks first and gives up once the deadline has passed. The last look
    now comes on the far side of the deadline, so each window grows by at most one poll: 200 ms, and 100 ms, in
    60 s.
  - `wait_until_every_node_passes` gave up at `now >= deadline`, and now gives up at `elapsed > timeout`. The two
    differ only at the deadline instant itself.
  - `split_partition_induces_round_changes_and_cluster_reconverges_after_heal` holds its partition for 3 s, the
    time in which rounds must change, and that is the test's arrangement. It sleeps through `PollingWait::sleep`,
    as long as before (entry 11).
- **Status:** answered.
- **Answer:** no assertion changes and no test is added or lost. The two widened windows are named here, as
  *Reporting* asks of a moved time bound. Each widens by one poll at most. Neither test's verdict for a
  cluster that commits, or one that never does, can move. `timeless-tests` is left holding the wake signal's
  six alone (entry 9).
- **Commit:** 44.

### 13. Factories that cannot fail stop saying they might

- **Raised:** commit 45, Part A, a failure type per concern -- its first commit.
- **What:** `build_observer`, `build_partitioner`, `build_client_interface_incoming`,
  `build_client_interface_outgoing`, and the local transport's `build_transport_incoming` and
  `build_transport_outgoing` each returned `Result<_, String>` and only ever `Ok` -- six `typed-errors` offences
  with no failure to type. They now return what they build. Each one's test asserted `is_ok()`, a check the
  signature now makes at compile time.
- **Status:** answered.
- **Answer:** five tests now assert on what was built, each saying more than `is_ok()` did. The partitioner sorts a
  cancelled timer into outputs. The incoming adapter hands on a request queued on its bus. The outgoing adapter
  files a response on its bus. The local incoming transport receives nothing, and the local outgoing transport
  accepts a send. The sixth, `build_observer_hands_back_an_observer_whatever_log_level_it_is_given`, cannot follow:
  `ProtocolObserver`'s methods return nothing and the logging observer only prints. It now hands each observer, at
  both log levels, an event, and its two `is_ok()` assertions go. The inventory reports it as saying less, two
  assertions to none. The claim those assertions made, that the factory hands back an observer, is now the
  function's signature, which no build can break. That is the test's mapping (constraint 6), and it is not a
  relaxation of what is checked. The two client-interface factories were an exact-duplicate pair the dry4rust
  baseline recorded, and dropping `Result` reshapes both alike, so the baseline is re-recorded as entry 7 did:
  the pair goes out under `d654de7c2f38ef52` and comes in under `fcde559ae97fd319`. The two groups paid in commits
  38 and 42 drop, none is added, 215 to 213.
- **Commit:** 45.

### 14. Two assertions move with the transport's failure type

- **Raised:** commit 47, Part A, a failure type per concern.
- **What:** the gRPC transport's three failures -- two poisoned locks and a server thread that would not spawn --
  become `TransportFailure`. Two existing assertions change with them:
  - `transport_factory::build_transport_outgoing`, the factory that picks the local or the gRPC transport, never
    failed either. It now returns what it builds, as entry 13's six do, and its test,
    `build_transport_outgoing_on_the_local_backend_ignores_the_peer_addresses`, checks that a send to a peer it
    was given an address for succeeds, where it checked `is_ok()`.
  - `NodeFailure::Transport` now carries a `TransportFailure`, not a `String`.
    `format_of_a_failure_debug_quotes_its_text_as_the_string_it_replaces_did` built one from the text "invalid
    listen addr", and now builds it from a poisoned server-state lock, with the quoted text to match.
- **Status:** answered.
- **Answer:** the first says more than before, as in entry 13. The second checks the same thing, that the failure's
  `Debug` is its text quoted, on a value that can still be built.
- **Commit:** 47.

### 15. Five assertions move with partitioning's failure type

- **Raised:** commit 48, Part A, a failure type per concern.
- **What:** the partition table's environment loading, and the partition control's lines and thread, failed with
  `String`s. They now share `PartitionFailure`, one type for the partitionable transport's one concern. The first
  try gave the two halves a type each. dry4rust then found the table's two-variant `Display` a near-duplicate of
  xtask's `TreeFailure` `Display` at 93%: two unrelated enums, alike in shape. There was nothing shared to
  extract across the two crates. The one type, six variants, is the better shape and resembles nothing else. Five
  assertions change:
  - four of `partition_control_tests` checked `result.unwrap_err().contains("...")` on the `String`, and now check
    the same words in the failure's text, `result.unwrap_err().to_string().contains("...")`;
  - `initialize_from_env_refuses_a_pair_that_is_not_two_numbers` checked `loaded.is_err()`, and now checks
    `Err(PartitionFailure::InvalidPair("banana"))`: which pair, and why.
- **Status:** answered.
- **Answer:** the four say the same of the retyped failure; the fifth is the strengthening constraint 6 asks for --
  "one that only checked `is_err()` names the error". A spawn failure keeps the system's reason as text, which
  is all it was ever used for; that keeps the type comparable, so the three tests that compare `Ok(..)` stand
  unchanged.
- **Commit:** 48.

### 16. The client-interface codec's refusals named

- **Raised:** commit 50, Part A, a failure type per concern.
- **What:** the client-interface codec's six fallible functions failed with `String`s and now fail with
  `ClientInterfaceCodecFailure`. Four of its tests checked a refusal with `is_err()` alone:
  `decode_transaction_refuses_a_value_that_is_not_a_number`, `decode_transaction_refuses_an_address_of_the_wrong_width`,
  `encode_balance_response_refuses_a_response_of_the_wrong_kind` and
  `encode_height_response_refuses_a_response_of_the_wrong_kind`.
- **Status:** answered.
- **Answer:** each now asserts the failure itself. That is the value and the error it failed to parse with, the
  address's length, and the request named with the response that did not match it. This is the strengthening
  constraint 6 asks for ("one that only checked `is_err()` names the error"). The gRPC handlers hand the
  failure's text to `Status` as they handed the `String`, so a client sees the same message.
- **Commit:** 50.

### 17. The node failure's words test builds a storage failure

- **Raised:** commit 51, Part A, a failure type per concern.
- **What:** `SledStorage::new` now fails with `SledStorageFailure`, and `NodeFailure::Storage` carries it.
  `to_string_of_a_part_that_refused_in_words_is_those_words` built `NodeFailure::Storage` from the text "failed to
  open sled db". It now builds it from a directory that could not be made, and expects that failure's words.
- **Status:** answered.
- **Answer:** the same contract, as in entry 14: a node failure prints the refusing part's own words, unchanged.
  The value it is checked on is one the part can now produce.
- **Commit:** 51.

### 18. The storage codec's refusals compared as failures

- **Raised:** commit 52, Part A, a failure type per concern.
- **What:** the storage codec's twelve fallible functions failed with `String`s and now fail with
  `StorageCodecFailure`: a value that would not decode, one that would not encode, or bytes of the wrong length,
  each naming the value. Five assertions change. Four compared the refusal's text --
  `Err("invalid address length: 21")`, and the same for a contract storage key, a hash and a height -- and now
  compare the failure, `Err(StorageCodecFailure::InvalidLength("address", 21))`. The fifth,
  `decode_block_refuses_bytes_that_are_not_a_block`, checked `is_err()`, and now checks that it is a block that
  would not decode.
- **Status:** answered.
- **Answer:** the four check the same refusal. Its text, `invalid {what} length: {length}`, is pinned by the
  failure's own tests, word for word as the codec printed it. The fifth is the strengthening constraint 6 asks for.
  Typing the codec reshaped two duplicate pairs the dry4rust baseline records: `decode_address` with
  `decode_hash`, and `decode_account` with `decode_commit_certificate`. dry4rust refuses a reshaped recorded
  duplicate, so the duplication went: each pair now calls one private generic helper, a fixed-width read or a
  wire type's decode. The baseline is not re-recorded. It matches 211 of its 213 groups, and the two it no longer
  matches are paid.
- **Commit:** 52.

### 19. The cluster config's refusals read through their text

- **Raised:** commit 54, Part A, a failure type per concern.
- **What:** `ClusterConfig::load_from_path`, `find_node` and `validate` failed with `String`s and now fail with
  `ClusterConfigFailure`. Eight tests compared the refusal with `assert_eq!(result.unwrap_err(), "...")`, for
  example "node id 99 not found in config" and "fleet.validator_set must not be empty". They now compare
  `result.unwrap_err().to_string()` with the same text.
- **Status:** answered.
- **Answer:** the same words, checked the same way, of the retyped failure. `validate`'s three empty-field checks
  now run in one loop over the fields, in the same order and with the same text. `main.rs`, which printed the
  `String`, prints the failure's text.
- **Commit:** 54.

### 20. The gate sequence's failure, and its two tests

- **Raised:** commit 55, Part A, a failure type per concern -- xtask's first.
- **What:** `GateSequence::run` failed with the `String` "{stage} -- {gate} ({reason})". It now fails with
  `StageFailure`, a record of the three. stern's `records-and-objects` refused it as a type with a constructor and
  accessors' worth of behaviour, so it is built by literal. `run_names_the_sequence_that_failed_rather_than_any_other`
  now compares the same text through `to_string()`. `run_reports_the_sequence_label_the_failing_gates_label_and_the_reason`
  now compares the failure's three parts, and `to_string_names_the_stage_the_gate_and_the_reason` pins the text
  they make.
- **Status:** answered.
- **Answer:** both claims are unchanged. The reason a gate gives is still text until the gates get their own failure
  type, top-down, as entry 13 onwards did for the node. `main.rs`, which printed the `String`, prints the failure's
  text.
- **Commit:** 55.

### 21. The gate tests read a failure through its text

- **Raised:** commit 56, Part A, a failure type per concern -- the first of three commits that give `Gate::run` a
  failure type.
- **What:** `Gate::run` has ten implementations, and their tests compared the `String` each fails with. Changing the
  trait changes all ten at once; with their tests it is some thirty files, past the threshold, so the tests go
  first, alone. Of the 32 assertions, 24 compare `result.map_err(|failure| failure.to_string())` to the same
  `Err(String::from(..))` they compared `result` to, and eight read `error.to_string()` where they read `error` in
  `is_err_and`. A 33rd test, the long-output CRAP one, takes its failure's text before its three assertions,
  unchanged, read it.
- **Status:** answered.
- **Answer:** every claim is unchanged: the same words, compared the same way. `to_string()` of a `String` is the
  string itself; of the failure type to come it is its `Display`, which prints the old text word for word
  (constraint 7). No test is added, removed or renamed; 3,580 before and after.
- **Commit:** 56.

### 22. The fake gate fails with a gate's failure

- **Raised:** commit 59, Part A, a failure type per concern -- the third of the commits for `Gate::run`.
- **What:** `FakeGate::failing` took a free text for its reason; a `GateFailure` has no variant for free text, so it
  now takes the failure itself. Three assertions change with it:
  `gate_tests::run_on_a_failing_gate_returns_its_reason` compares `Err(GateFailure::RuleBroken)` where it compared
  `Err(String::from("because"))`;
  `gate_sequence_tests::run_reports_the_sequence_label_the_failing_gates_label_and_the_reason` expects the stage
  failure's reason to be `Box::new(GateFailure::RuleBroken)` where it was the text "a rule was broken";
  `gate_sequence_tests::run_names_the_sequence_that_failed_rather_than_any_other` gives its fake
  `SystemTestFailed(Some(1))` and expects "Stage 5 -- System tests (a system test failed, exit code Some(1))" where
  it gave the text "a system test failed" and expected that text in the same place. The stage failure's own test
  keeps its expected text: `RuleBroken` prints "a house coding rule was broken", the text it gave before.
- **Status:** answered.
- **Answer:** every claim is the same -- a failing gate returns what it failed with, and the stage names the stage,
  the gate and that failure -- made of a failure a gate can give rather than of words no gate prints. The longer
  expected text is the fake's failure printed whole; the stage's part of it is unchanged. The reason is boxed
  because clippy's `result_large_err` refused a stage failure of 128 bytes.
- **Commit:** 59.

### 23. The runner's launch failure, read through its text

- **Raised:** commit 60, Part A, a failure type per concern -- `CommandRunner`'s two methods.
- **What:** `run_capturing_a_program_that_does_not_exist_returns_a_launch_error` asserted
  `result.is_err_and(|error| error.starts_with("failed to launch"))` of the `String` the runner failed with. The
  runner fails with a `LaunchFailure` now, and the assertion reads `error.to_string().starts_with("failed to
  launch")`. The three tests that built a `Launch` variant from a text -- the gate's, the tree's, the measure's --
  build a `LaunchFailure` whose text is the one they gave, and their assertions are unchanged.
- **Status:** answered.
- **Answer:** the claim is the same -- a missing program is a launch failure, not an exit code -- and it is checked
  the same way, on the same words. `LaunchFailure`'s own test pins the whole text.
- **Commit:** 60.

### 24. The CRAP report parser's failures, read through their text

- **Raised:** commit 62, Part A, a failure type per concern -- `CrapReportParser::parse`.
- **What:** `parse_a_truncated_report_returns_a_parse_error`, `parse_empty_output_returns_a_not_found_error` and
  `parse_output_carrying_no_report_returns_a_not_found_error` asserted `result.is_err_and(|error|
  error.contains(..))` of the `String` the parser failed with. It fails with a `CrapReportFailure` --
  `NoReport` or `Unparsable` -- and each now reads `error.to_string().contains(..)`, the same words.
- **Status:** answered.
- **Answer:** the three claims are unchanged and checked the same way. The new type's two tests pin each variant's
  whole text, and the gate failure's report test builds a `NoReport`, its expected text unchanged.
- **Commit:** 62.

### 25. One file per implemented type: merge, or split?

- **Raised:** after commit 63, Part A, slice 3, one file per implemented type -- before any of its commits.
- **What:** `implemented-in-one-file` holds two offences. `IbftProtocol` is implemented in 13 files, 4,857 lines
  (`consensus.rs` 1,447, `dispatch.rs` 873, `validation.rs` 664), and `RecoveryControl` in 5 (`recovery_control.rs`
  and four handler files, about 700 lines). stern offers two fixes and prefers neither: "move every `impl` into the
  type's file, or split the type into types of their own". *Known before starting* priced the slice at 3-6
  commits. Measured before starting, a merge costs much more than that:
  - **A merge carries the tests.** Each handler file has a test file named for it. With the source gone, stern's
    `paired-test-file` refuses the test file ("named for src/process/query_handling.rs, which does not exist" --
    probed on an export, not on the branch). Moving the tests first breaks `twin4rust` instead, so each piece of
    source moves with its tests: about 700 lines of source and 1,400 of tests for `RecoveryControl`, into one file
    each.
  - **The threshold makes it long.** A move counts its lines twice. Moved a few handlers at a time, with their
    tests, `RecoveryControl` alone is some 14 commits, and `IbftProtocol` -- some 4,500 lines of source and its
    tests -- several dozen. The alternative is a handful of commits past the threshold, against a 5% allowance
    four commits have already drawn on.
  - **iceberg refuses the result.** It counts every non-`pub` method as private implementation. Merged with its
    handlers private, `recovery_control.rs` scores about 19 against the ceiling of 20 (today's per-file scores
    summed: 7.70, 4.42, 2.55, 1.96, 1.65, 0.99), and a merged `protocol.rs` would be many times over it.
  - **The split is a redesign.** Every handler of `RecoveryControl` but the queries mutates the one coordinator, so
    with no `&mut` parameters allowed (`returned-mutations`) a type of its own cannot borrow it. Only the queries
    split off cleanly, as a type borrowing the coordinator and the facts. The others need the coordinator's
    ownership redesigned. `IbftProtocol`'s split is the protocol's core, which Part B's seams reshape anyway --
    execution, the bookkeepers, the observer.
- **Status:** open -- a design question with no house default (*Questions raised*): the two fixes lead to
  different code, and the cheaper one in commits leaves the worse code.
- **Course taken:** the slice waits. Both offences stay in the stern baseline, and behaviour and tests are as they
  are. The run goes on with slice 4, `pub` or private. All 30 of its offences are methods these two types keep
  `pub(crate)` for their sibling files, and the plan's fix -- each made `pub`, with tests of its own -- holds
  whichever answer this entry gets.
- **Recommendation:** split. In `recovery-control`: the folded-in facts (its own id, the height, the membership)
  become an object of their own, and the queries a type borrowing the coordinator and the facts; for the
  handlers that mutate, the coordinator's ownership is decided first. `IbftProtocol` is split along the lines
  Part B's seams already draw, slice by slice, each split taking its tests with it.
- **Also waiting on it:** two `records-and-objects` offences (slice 8) -- `IbftProtocol` exposes 27 fields and
  `RecoveryControl` 5 to their sibling implementation files, which a private field would put out of reach -- and,
  from commit 145 (slice 9), one `record-home`: `RecoveryMessageFields` is used by `dispatch.rs` alone, one of
  `IbftProtocol`'s implementation files, which declares no type of its own; moved there, `named-subject` refuses
  the file as named after none of its types (tried on the branch and reverted before the commit). A merge
  answers all three; so does a split that gives the dispatch its own type.

### 26. A recorded duplicate reshaped: the three block builders with commitments

- **Raised:** commit 69, Part A, named values for tuples -- its first commit.
- **What:** `BlockCommitmentsVerifier::compute` and `IbftProtocol::execute_and_compute_commitments` returned
  `(Hash, Hash)`, two `distinct-tuple-elements` offences; they now return `BlockCommitments`, the post-state and
  receipts roots by name. `build_block_with_commitments` exists three times -- `node/tests/ibft/scenario_helpers.rs`,
  `validation/src/scenario_helpers.rs` and `validation/src/ibft_cluster_test_helpers.rs` -- and the dry4rust
  baseline records the three as one exact group, `f74317da987ed69e`. Each now destructures the record where it
  destructured the tuple, so all three change alike, the group's fingerprint changes, and the gate reports it as
  new. The duplication cannot be removed here: the node's tests cannot use the validation crate's helper.
- **Status:** answered.
- **Answer:** re-record the dry4rust baseline (`cargo dry4rust baseline --path . --exclude-tests --min-lines 5`),
  as entries 7 and 13 did. Checked fingerprint by fingerprint: the group goes out under `f74317da987ed69e` and comes
  in under `40298922b91b8ae9`, three members both times. The only other changes are the three groups this branch
  has paid -- the storage codec's two at commit 52 and the stern and twin gates' pair at commit 58 -- which go, and
  three closure names re-keyed to their shifted lines. 213 groups to 210, none added. A first re-record run with an
  absolute `--path` wrote closure names with the machine's absolute paths. It was restored and run again from the
  repository root, so the names keep their `./`.
- **Commit:** 69.

### 27. The receipt summary's two tests compare it by name

- **Raised:** commit 71, Part A, named values for tuples.
- **What:** `TransactionReceipt::summarize_receipts` returned `(usize, usize, usize, usize)` -- successes, out of
  gas, reverted, invalid opcode, in that order. It returns `ReceiptSummary`, the four counts by name.
  `summarize_receipts_counts_each_status_variant` compared the result with `(1, 1, 1, 1)` and
  `summarize_receipts_empty_slice_returns_zero_counts` with `(0, 0, 0, 0)`. Each now compares it with a
  `ReceiptSummary` holding the same four counts. The node's two callers and its two tests destructure the record
  under the names they gave the tuple's elements, and their assertions are unchanged.
- **Status:** answered.
- **Answer:** the claims are the same counts, checked the same way, field for field. Both tests expect every count
  equal (all one, all zero), so neither form could tell two counts swapped; the node's storage test, whose counts
  differ (two successes), keeps its four assertions on the named values.
- **Commit:** 71.

### 28. The SHA3 window's test compares it by name

- **Raised:** commit 73, Part A, named values for tuples.
- **What:** the EVM's `sha3_range` returned `Option<(u64, u64, u64)>` -- the offset, the size and the end of the
  memory SHA3 hashes, in that order. It returns `Option<Sha3Window>`, the three by name.
  `sha3_range_returns_the_offset_size_and_end_of_the_window` compared the result with `Some((64, 4, 68))`; it now
  compares it with `Some(Sha3Window { offset: 64, size: 4, end: 68 })`. The rejection test's `None` is unchanged.
- **Status:** answered.
- **Answer:** the same three values, checked the same way, field for field. They differ from one another, so the
  tuple already told a swap apart, and the record does too.
- **Commit:** 73.

### 29. The cluster harness's node heights print as the pairs they were

- **Raised:** commit 75, Part A, named values for tuples.
- **What:** `get_height_tolerating_in_flight_commit`, `get_heights_tolerating_in_flight_commit` and
  `wait_until_heights_settle` returned `Vec<(u64, u64)>` -- a node and a height, in that order -- and
  `wait_until_all_nodes_height_stays_at_most` took one. They now return and take `Vec<NodeHeight>`
  (`system-tests/src/cluster_wait/node_height.rs`). `wait_until_heights_settle` prints the list on its
  `cluster_wait_heights_settle` lines and hands its `{:?}` to `WaitProgress`, whose failure text ends "observing"
  it -- a panic text. A derived `Debug` would print `NodeHeight { node_id: 1, height: 12 }` where `(1, 12)` was
  printed. `get_heights_tolerating_in_flight_commit_returns_entry_per_node` read `heights[0].0` and `(_, h)`.
- **Status:** answered.
- **Answer:** `NodeHeight`'s `Debug` is written by hand to print `(1, 12)`, so the log lines and the panic text
  are the old ones word for word (constraint 7's reason, and the log lines `refactoring` keeps); a new test,
  `fmt_a_list_prints_each_node_and_height_as_the_pair_it_replaced`, holds it to the tuple's own `{:?}`. The
  harness test reads `heights[0].node_id` and `reached.height` -- the same five checks of the same values, by name.
  The tests that build the limits by hand build `NodeHeight`s with the same numbers.
- **Commit:** 75.

### 30. Ten opcodes a re-export shim had counted as shared

- **Raised:** commit 79, Part A, constants given one home.
- **What:** commit 78 moved the 33 opcode bytes to `evm/src/tiny_evm/opcode_constants.rs` and paid their 33
  `shared-constants` offences. `tiny_evm_engine.rs` re-exported every one by `pub use` -- a re-export shim, which
  CLAUDE.md forbids -- and stern counts a re-export as a reader. Commit 79 takes the shim out and points its
  fifty-odd test imports at `opcode_constants` and `tiny_evm_opcode`. With it gone, ten opcodes have one reader:
  `OPCODE_MUL`, `DIV`, `LT`, `GT`, `EQ`, `ISZERO`, `OR`, `DUP16`, `SWAP16` are read by `tiny_evm_opcode.rs` alone,
  which decodes and names them, and `OPCODE_PUSH2` by `tiny_evm_engine_tests.rs` alone, which assembles it --
  production matches pushes as the range `PUSH1..=PUSH32`. Ten offences would come back.
- **Status:** answered.
- **Answer:** as stern's corrections say: the nine go private into `tiny_evm_opcode.rs`, and `OPCODE_PUSH2` into
  `tiny_evm_engine_tests.rs` as a private constant with its byte, `0x61`. Every byte is the one it was, the
  opcode table now lives where it is read, and no count moves: 137 as configured, 348 with everything on, the
  stern baseline unchanged -- so commit 79 is a `refactoring`. Commit 78's 33 stand paid: ten of them only
  because the shim read them, until this commit.
- **Commit:** 79.

### 31. The diagnostic beacon's field table stays beside the struct

- **Raised:** commit 81, Part A, constants given one home.
- **What:** `node/src/observer/node_diagnostics.rs` declared four public constants its test reads:
  `DIAGNOSTIC_FIELDS`, the beacon's budget table, and three byte bounds derived from it or beside it --
  `DIAGNOSTIC_MAX_BYTES` (the table's rendered width, through `NodeDiagnostics::diagnostic_max_bytes`),
  `DIAGNOSTIC_PREFIX_MAX_BYTES` and `DIAGNOSTIC_DATAGRAM_BYTES`. A `const` assertion in the file holds the three
  bounds to one UDP datagram at compile time. Moved together to `diagnostic_constants.rs`, the table is read there
  (by `DIAGNOSTIC_MAX_BYTES`) and by its test; stern does not count a constants file reading its own constant, so
  it reports the table as read by the test alone and corrects: "move it into `node_diagnostics_tests.rs` and make
  it private" (measured on the working tree, then undone).
- **Status:** answered.
- **Answer:** the table stays in `node_diagnostics.rs`, beside the struct its comment says it is kept beside, and
  its one offence stays in the baseline under the key it had. Moving it into a test would take the production
  declaration the compile-time bound is computed from out of production; making production read it a second time
  to look shared would be paying the count, not the offence. The three bounds move to
  `node/src/observer/diagnostic_constants.rs`, read by the assertion and the test, and their three offences are
  paid. Left for whoever owns the rule: a constants file's own reads could count as readers.
- **Commit:** 81.

### 32. The bootstrapper is handed its coordinator

- **Raised:** commit 92, Part A, a trait-typed field only from a constructor parameter.
- **What:** `Bootstrapper`'s `coordinator` field, a `Box<dyn BootstrappingCoordinator>`, was filled in `new` and
  `inactive` with a `NoOpBootstrappingCoordinator` they built, and replaced in `init` with a `Faction` built from
  the local peer, the validators, the quorum threshold and the observer -- what only the start of peer discovery
  knows. Three `injected-dependencies` offences. A probe on the working tree found that stern refuses any
  non-parameter assigned to the field: `coordinator_factory.idle()` in a constructor and
  `self.coordinator_factory.coordinator(..)` in `init` were both reported. The 46 constructions in 14 files and the
  protocol's call change with it, and seven dry4rust groups -- six exact pairs of protocol helpers between
  `node/tests/ibft/protocol_helpers.rs` and `validation/src/protocol_helpers.rs`, and one near pair inside the
  first -- change alike and fail under new fingerprints.
- **Status:** answered.
- **Answer:** the coordinator is a parameter everywhere it is set. `new` and `inactive` take the idle coordinator
  and a `BootstrappingCoordinatorFactory` (`FactionCoordinatorFactory`, commit 91); `BootstrappingLifecycle` gains
  `discovery_coordinator(local, validators, observer)`, which works out the threshold as `init` did --
  `required_count()`, else `2n/3 + 1` -- and asks the factory, and `init(coordinator)` installs what it is handed;
  the protocol's peer discovery builds, then installs. Tests may now install any coordinator. The two tests that
  called `init` with the old arguments build the coordinator in their Arrange and install it in their Act, their
  assertions unchanged. The dry4rust baseline is re-recorded as entries 7, 13 and 26 did, checked fingerprint by
  fingerprint: the seven groups out under `067d43e85cfa2e1a`, `2aa02f4328261bba`, `49e27685fbd6acea`,
  `524833ce0120f1c2`, `622551247d01ce55`, `976d7f815d966e2f` and `19a281f662079260`, and in, the same members,
  under `ba63aeb784810c83`, `cb09a6a75f1e5f01`, `0ab9c7fcd9432e87`, `1ff10db1564d649b`, `fea83b57040d54cc`,
  `e060de14bcb416e6` and `30fa1cefabe768d5`; the civil-date pair commit 70 paid dropped; one closure's name moved a
  line; none added -- 210 to 209. A new near duplicate the first try made -- the lifecycle impl's
  `discovery_coordinator` delegating like `handle_retry` -- was removed instead: the trait impl holds the body.
  SETUP named this piece "likely past the threshold", and it is: 19 files and 548 lines, 137 of them the baseline.
- **Commit:** 92.

### 33. Two configs' defaults, alike in shape

- **Raised:** commit 96, Part A, records and objects.
- **What:** a getter-only struct's correction deletes its constructor, so its `Default` builds it by literal.
  `ConsensusTimingConfig`'s (commit 95) and `BootstrappingConfig`'s (this commit) are each
  `fn default() -> Self { Self { a: .., b: .., c: .. } }`, three fields of fixed values, and dry4rust reports them
  as one near-duplicate group at 91% (`c2faba878ab6d286`). dry4rust normalizes names and values, so what it matches
  is the shape of a three-field literal: the two share no field, no value and no meaning, and there is no part to
  extract and hand in. The base's own baseline holds groups of this shape -- the builders' `Default`s, two groups of
  three and six, and constructors that only copy their arguments in. Commit 95 met the same shape once, an exact
  duplicate of a test helper building a `RecoveryRequest`, and removed it, the helper becoming the constant it was.
  Here neither side has another form: each is a `Default` that production and tests in several crates call.
- **Status:** answered.
- **Answer:** the group is recorded in the dry4rust baseline (`cargo dry4rust baseline --path . --exclude-tests
  --min-lines 5`), checked fingerprint by fingerprint against the copy kept in scratch: `c2faba878ab6d286` in, with
  the two `Default`s as its members, and every other entry unchanged -- 209 to 210. The plan's own rule for the stern
  baseline is the default taken: a commit whose offences fall may add one only when it is named in `QUESTIONS.md`,
  and this one is. The gate still fails on any duplicate a change adds beyond those named here. The same answer
  serves a later literal of this shape, each group named in its commit's message; one with a part to share is
  shared instead. Left for whoever owns the tool: a body that is a struct literal alone could be out of the near
  match, as code under five lines is.
- **Commit:** 96.
- **Taken again:** commit 104 -- `RetainedCounts::with_commit_certificates`, `with_pending_transactions` and
  `Block::with_parent_hash`, three one-field struct updates on two types, recorded as the exact group
  `63532d7dea94bfeb`; the same commit paid `4357ad2b28af5fbc`, so the baseline stays at 210. Commit 105 --
  `InFlightSlot::request` and `ContextSnapshot::header`, each a three-field literal of its own fields, recorded as
  `f280cde3a9b5c133` (entry 35). Commit 122 -- the hard state's and the volatile snapshot's `with_` methods, sixteen
  multi-line one-field struct updates, join `63532d7dea94bfeb`, 3 members to 19: each sets its own field of its own
  type, and the only shared part is the struct-update syntax itself. Commit 123 -- node's `sample_hard_state` and
  types' `checkpoint_state` test helpers, four-step `with_` chains in two crates' tests, recorded as
  `ce4117ea3c05a62b`; the near pair the first formed as a literal, `9e0b49ae282799b9`, paid; 202 kept. Commit 126
  -- validation's `sample_hard_state` and types' `base_state`, full seven-field chains, recorded as the near pair
  `9940b28a2a188433`, 202 to 203. Commit 128 -- seven of the context snapshot's multi-line `with_` methods join
  `63532d7dea94bfeb`, 19 members to 26. Commit 135 -- three of the block's new `with_` methods
  (`with_post_state_root`, `with_receipts_root`, `with_transactions`) join it, 26 members to 29; 203 kept. Commit
  179 -- validation's protocol helpers build their bootstrapper through node-infra's `BootstrapperBuilder`, so
  each active helper and its inactive twin differ by `inactive()` alone: three near pairs recorded,
  `6ef0847f24a9ccfa` (`setup_dependencies`, `setup_hard_state_dependencies`), `b2ccf891cc51ff07` (the two
  `_with_observer`) and `b90d9211dac07c91` (the two `_with_execution_engine`). Sharing was measured and refused: a
  delegating helper of five lines or more differs from its twin by the same `inactive()`. The same commit's six
  exact pairs between those helpers and node's test helpers of the same name no longer match, node's still building
  the old way; 196 to 193. Commit 187 -- node's recording observer gives its readiness record to node-faction's
  tests, and its `readiness_transition_observed` becomes an empty six-line method like the 42 of the group of empty
  observer methods, `d4439664a026e9d5`, which takes it as its 43rd member; the facet's members, renamed by 186, are
  named again; 196 kept.

### 34. A getter's test the record-alone rule refuses

- **Raised:** commit 97, Part A, records and objects.
- **What:** `NewViewMessageFields` -- the NewView message's parts, borrowed, as dispatch hands them to consensus
  and the validator -- had public fields and five getters that only read them: a `records-and-objects` offence
  ("getter-only"). With the getters gone, its five getter tests in `dispatch_tests.rs` read the fields, and
  `behaviour-tests` refuses one of them,
  `node/tests/ibft/ibft_protocol/dispatch_tests.rs::view_change_proofs_returns_the_proofs_the_new_view_carries`:
  "tests `NewViewMessageFields` alone: it calls nothing of its package but a record's accessors and derived
  traits"; its correction is to test the behaviour that uses the record and check there what this test checked.
- **Status:** answered.
- **Answer:** the test is removed, under constraint 6's licence -- "a test of a record alone that
  `behaviour-tests` refuses, above all". No move keeps it: moved anywhere, it still reads a field of a record and
  nothing else. No rewrite keeps it under its name: the behaviour that reads the proofs is the validator's
  `change_senders`, and a test of that is a test of the validator, in its own file. That test exists:
  `node/tests/ibft/ibft_protocol/new_view_validator_tests.rs::change_senders_collects_one_entry_per_distinct_proof_sender`
  builds the fields with proofs from senders 3, 1 and 2 and checks the validator reads exactly those three, so
  what the removed test claimed -- the fields carry the proofs they were built with, to whatever reads them -- is
  still checked, through the behaviour; the commit's `Maps:` line points there. The only other route, the getter
  kept and the offence left in the baseline, would keep a test of an accessor no code needs. The other four getter
  tests -- height, round, certificate and signature -- are as much tests of the record alone, but the gate does
  not refuse them, and the licence covers only what a gate forces, so they stay, reading the fields, their
  assertions the same. Left for whoever owns the rule: the five are built alike, through the same helper, and
  its report does not say what sets the fifth apart.
- **Commit:** 97.

### 35. The in-flight slot's getters reshape a recorded pair

- **Raised:** commit 105, Part A, records and objects.
- **What:** `InFlightSlot` -- the node's one outstanding recovery request, with what a reader of the decision needs
  beside it -- exposed its five fields. With them private, the recovery state reads the slot through getters, and
  its `fail_request` and `refuse_request`, which the dry4rust baseline records as an exact pair
  (`b4e7991104edfc28`), stay a pair under a new fingerprint: they differ, as before, only in which list the
  unanswered peer goes into. The two places that built a `RecoveryRequest` field by field from the slot now ask the
  slot, `request()`, and dry4rust pairs that literal with `ContextSnapshot::header`, three fields of its own.
- **Status:** answered.
- **Answer:** the pair is re-keyed, as entries 7, 13, 26 and 32 re-keyed theirs, checked fingerprint by fingerprint
  against the copy kept in scratch: `b4e7991104edfc28` out, `0df28f4a7502fa38` in, the same two members. Folding the
  two into one would hand a method the list it writes, which the house rules refuse (no `&mut` parameter), or make
  each a three-line wrapper around a helper that is the same shape again. The literal pair is entry 33's case and
  is recorded as `f280cde3a9b5c133`, named in the commit's message. 210 to 211.
- **Commit:** 105.
- **Taken again:** commit 106 -- the recovery context's getters reshape the recorded pair of test recorders,
  `CapturingIbftRecorder::record` in node's and in recovery-ibft's tests, byte-identical still: re-keyed,
  `554acb512b3900bf` out and `52e6f1f4d2dbe8d5` in, 211 kept. Commit 109 -- the in-memory timer's and client
  interface's constructors, which open their node's queue through the state now, stay a pair: `b616d1a56a23b852`
  out, `5482cacfc35dcbd4` in; three groups paid beside it, 209 to 206. Commits 111 and 112 -- the pair of
  `setup_after_propose_with_tx` helpers, in node's tests and validation's sources, is one group re-keyed across two
  commits: 111 moved validation's copy onto the account's constructor and the pair stopped matching
  (`79cc9577e74d4e90` out, 206 to 204 with another paid group); 112 moves node's alike, and the same two are recorded
  again as `5666a931299b8d2e`, 204 to 205. Commit 120 -- the world state snapshot's constructor reshapes the pair of
  `snapshot` test helpers in node's and recovery-ibft's tests alike, from an exact pair to a near one:
  `c983291a4180624d` out, `a17c620ba9f6cd6b` in, 203 kept; the two live in two crates' tests and share nothing they
  could import. Commit 122 -- the pair of `setup_restored_protocol_with_fallback_validators`, in validation's sources
  and node's tests, reshaped alike by `with_active_validators`: `256b6ee250ec7b90` out, `53d1f2df9ed91e42` in; a
  near pair of test helpers paid beside it, 203 to 202. Commit 127 -- system-tests' `sample_hard_state` and
  `sample_locked_wal`, the same sample state in two test files, now `with_` chains: `68a95c56852dda33` out,
  `defbb0eb7230cf71` in, 203 kept. Commit 130 -- the context snapshot's getters and `with_` chains reshape three
  groups: the snapshot context builder's consensus-reply, recovery-response and request-status contexts
  (`d0de4dcaee535de8` out, `7c1a07413735c63e` in), dispatch's commit and prepare handlers (`0f12aadf1ae33d5d` out,
  `48bb57ae90e740a3` in) and validation's commit and prepare checks (`b4cf6cb55629bbab` out, `9178a547e5c34ac8` in),
  each with the same members, 203 kept. (Commit 130's message says the baseline still holds 200 groups: 200 is what
  the failing check suppressed beside the three it could not match, and the baseline holds 203 -- counted in the
  file at commit 131.) Commit 132 -- the pair of `setup_after_propose_with_tx` helpers again, both moved onto the
  context's `with_` methods in one commit so they stay a pair: `5666a931299b8d2e` out, `76af3e733fa15833` in, 203
  kept. Commit 137 -- recovery-ibft's test helpers through the block's accessors: its two `make_block`s, one line
  each now, under dry4rust's five, pay their pair `3610fac662d3b38a`; its `make_recovery_block` leaves its group
  of three the same way; its `commit_certificate_for`, reading the height through the getter, leaves its group of
  three until node's and validation's copies read it so too, when the three are recorded again; 203 to 202.
  Commit 138 -- node's and validation's helper twins move together: `commit_certificate_for` recorded again with
  its three members (`6a3e0cf1de95c744` out, `24c86180ef695fb7` in), `build_block_with_commitments` and
  `make_pre_prepare` re-keyed (`40298922b91b8ae9` to `410cdf68518b2f17`, `c2117b77ffa96486` to
  `5f18c4d6efa5aab5`), `make_recovery_block` paid (`bfc9ace60a5bfbb4`, one line each now); the snapshot anchors'
  exact pair (`5472a4f676429fed`, node's copy and validation's) goes with node's copy, and validation's two copies,
  differing in the gas limit, are recorded as the near pair they are (`ea21bd393a440c18`) until validation's tests
  move; 202 to 201. Commit 139 -- the wire block's pair of field-for-field `From`s paid (`b52b192032123e3e`), the
  block's side a constructor chain now; 201 to 200. Commit 141 -- validation's two snapshot `anchor()`s, two lines
  each now, pay their near pair (`ea21bd393a440c18`); 200 to 199. Commit 166 -- node's tests build their context
  builder through one helper, which reshapes three groups of the recovery-host tests' `deliver` helpers alike: the
  exact `deliver` pair (`0c89ced451914a87` out, `9c4543b6454ba7bf` in), the exact `deliver_against` pair
  (`d1a94f3e0d71aeac` out, `f022dffb125ee388` in) and a near `deliver` pair (`32a5210819b973f6` out,
  `5f1cfde88a32f74e` in), each the same two members; 196 kept. Commits 179 and 180 -- validation's protocol helpers
  and then node's build their bootstrapper through node-infra's `BootstrapperBuilder`, one group of six pairs
  re-keyed across two commits as at 111 and 112: 179 leaves the six exact pairs of same-named helpers unmatched
  and records three near pairs inside validation's file (entry 33), 196 to 193; 180 moves node's alike, and the six
  match again -- `setup_dependencies` `0ab9c7fcd9432e87` to `4f8568ffaa402801`, `setup_hard_state_dependencies`
  `ba63aeb784810c83` to `39828b559a465759`, `setup_dependencies_with_execution_engine` `fea83b57040d54cc` to
  `3497b040b8f5d88d`, `setup_hard_state_dependencies_with_execution_engine` `1ff10db1564d649b` to
  `ee17610101adb955`, `setup_dependencies_with_observer` `e060de14bcb416e6` to `993aecb9b6912fc8`,
  `setup_dependencies_with_consensus_timing_policy` `cb09a6a75f1e5f01` to `b0b2f07b2272c23f` --, the three near
  pairs fall away as their members rejoin those groups, and node's near pair of recorder helpers is re-keyed,
  `30fa1cefabe768d5` to `f16f311374d1ad25`; 193 to 196, the members as at 178.

### 36. The hard state drops a pending block's transaction data

- **Raised:** commit 115, Part A, records and objects -- found moving the transaction's readers onto its getters.
- **What:** the consensus hard state persists the pending -- locked -- block, in two layouts: the whole state
  (`IbftHardStateEncoder`, types; the checkpoint record reuses it) and the `SetPendingBlock` delta
  (`WalRecordEncoder`, node). Both write each transaction's six scalar fields and not its `data`, and the cursor
  reads every transaction back with `data` empty. Three tests pin it -- one with the comment "the WAL never carries
  calldata, so data decodes empty", from 2026-08-21. Then the block hash was a byte-wise mix that left a
  transaction's data out, so a restored block hashed as before. Since `4f0c2cd` (2026-10-10) the Keccak block hash
  covers each transaction's data, so a node restarted holding a locked block with a contract call restores a block
  that no longer hashes as its prepared certificate: its votes and its own pending-block hash no longer match, and
  re-proposing it, which a locked proposer must, fails the NewView justification at every peer. A round is lost,
  not a block forked: what peers vote on is the certificate's hash.
- **Status:** answered.
- **Answer:** a bug -- the two halves of one design changed at different times -- fixed test-first in a `bug-fix`
  commit of its own (`SETUP.md`: "a bug found while fixing an offence is its own commit"): both encoders write the
  data, length first, and the cursor reads it. The house's default for a changed layout is in the history: when
  blocks gained a parent hash (`d809c18`), both layout bytes moved on -- `HARD_STATE_VERSION` 0x01 to 0x03,
  `WAL_V2_MAGIC` 0x02 to 0x04 -- "state in the 0x01 layout is refused, not misread". They move on again, 0x03 to
  0x05 and 0x04 to 0x06, past each other's old values as then. The tests that pinned the empty data check the data
  restored; a round-trip test of each layout is the commit's test, red on its parent. One of the three is renamed
  for what it now checks, `encode_decode_pending_block_transaction_drops_transaction_data` to `..._keeps_...`, its
  assertion the data written where it was the data lost; the commit's `Maps:` line names it. Not a
  security-relevant finding: it takes the node's own restart while it holds a lock on a contract call, which no
  peer or client can force, and what it costs is rounds, never a block that a quorum did not vote.
- **Commit:** 116.

### 37. The node diagnostics: a constructor of 28

- **Raised:** before commit 120, Part A, records and objects.
- **What:** `NodeDiagnostics` -- one node's diagnostic beacon sample -- has an associated `const fn`,
  `diagnostic_max_bytes`, and 28 public fields: a `records-and-objects` offence ("an object's fields are
  private"). Decision 1 keeps the function where it is, so the type is an object, and its correction is private
  fields, a getter for each reader -- the logging observer renders all 28 -- and a constructor for its one builder,
  `IbftProtocol::sample_diagnostics`, which now writes the 28 by name. The house's way past clippy's seven arguments
  is `#[allow(clippy::too_many_arguments)]` -- five constructors carry it, the largest of twelve -- but a constructor
  of 28, 24 of them `u32`, is another thing: two counters written in each other's place would compile, and nothing
  but the beacon's readers would ever show it. A record of the 28, handed to the constructor, keeps the names but is
  the same type twice over; grouping the fields changes the type's shape and every reader's.
- **Status:** open -- a design choice with no house default for this size.
- **Course meanwhile:** the type stays as it is, its offence in the stern baseline; the slice goes on with the
  other objects.
- **Recommended:** the type's own comments already divide the 28 into groups -- identity, chain position, chain
  progress, peers, the in-flight recovery slot, recovery counters, the protocol's census. Make each group a record
  of its named fields, in a file of its own beside the type, and give `NodeDiagnostics` a constructor taking the
  sequence and the six records, its getters handing each record back. The beacon's rendering and its byte budget
  do not change; its readers name the group they read.

### 38. The interpreter's operations: what returning what they change means

- **Raised:** commit 151, Part B, the skipped rules -- `returned-mutations` in evm, where it is stood down.
- **What:** `SETUP.md` says the interpreter's operations, which take the machine's stack, memory, gas and counter by
  `&mut`, "become functions that return what they change". Three shapes were weighed:
  - free functions taking each part by value and handing it back -- a `Vec<Hash>` threaded through every
    operation. The machine tries each group of opcodes in turn, and a group's dispatcher that declines an opcode
    would consume the stack it was handed: every dispatcher would need to give it back on both paths;
  - the operations as the bytecode machine's own `&mut self` methods. Its tests could then preset nothing -- the
    machine starts with an empty stack and memory -- and read nothing back;
  - the machine's changing parts -- stack, memory and its active words, gas, the working contract storage and the
    mutations -- as one object, the operations its `&mut self` methods, returning a value where they compute one.
  Moving an operation's body into an `impl` re-indents every line of it, so every line counts twice; the move goes
  a group of operations at a time.
- **Status:** answered.
- **Answer:** the third. CLAUDE.md allows `&mut self` in impl blocks, the rule counts parameters alone, and it is
  the house's own shape for an interpreter: `TinyEvmStorageSlotAnalyzer`, in the same folder, keeps its stack in
  itself and its arithmetic in `TinyEvmWordAlu`, which the machine's ten copies of the same arithmetic will use too.
  The object is `TinyEvmMachineState`. The program counter stays with the machine, which owns the program, and the
  push returns where it goes next (commit 151). The state comes in first, its methods calling today's functions;
  each group then moves in with its tests.
- **And:** where an operation answered whether it acted through a `bool` and its result through a `&mut`, it
  returns the result in an `Option`, and a test that asserted the two apart asserts the one value, which says
  both. The inventory counts one assertion fewer; each such test is mapped to itself in its commit's message,
  naming this entry -- `exec_push_n_reads_the_immediate_bytes_and_advances_the_counter` first, at commit 151.
- **Commit:** 151 onward.

### 39. The storage access plan reads evm's analyzer, which the execution port's move would make a cycle

- **Raised:** before commit 166, Part B, the execution seam -- planning the port's move after the merge.
- **What:** `SETUP.md` moves `ExecutionEngine` into `node/src/traits` and makes evm a plug-in "depending on `node`
  for the port". But `node` depends on `evm` in production: `TinyEvmStorageAccessPlan`, which decides which
  accounts and contract storage the node loads to execute a block and the context builder loads for a proposal,
  asks `TinyEvmStorageSlotAnalyzer` which slots a transaction's bytecode touches, falling back to a contract's whole
  storage when it cannot tell. With the port in `node`, evm would depend on `node` and `node` on evm. The plan
  assumed no such edge. (`node`'s tests also use evm -- the engine and opcode constants -- which a dev-dependency
  can carry, as `node`'s on `node-infra` already does.) Three ways were weighed:
  - the analysis as part of the execution port, each engine telling what its execution reads. The no-op and the
    value-transfer engines would then plan differently from today, or carry an EVM analysis they never run;
  - the analysis moved into `types` or `node` -- EVM bytecode knowledge in the data model or the protocol;
  - a port of its own for the analysis, injected where the plan is made.
- **Status:** answered -- the house has a clear default: a new protocol seam is a trait in `traits/`, its
  implementation in its own area, every dependency injected through the single constructor (CLAUDE.md).
- **Answer:** the third. `StorageSlotDiscovery`, in a new `node/src/traits`: the slots a bytecode touches, or none
  when it cannot tell. evm implements it with today's analyzer, and the node and the snapshot context builder are
  handed it through their constructors, so every engine keeps exactly today's plan -- the behaviour the reference
  traces and every test pin. `node` then keeps evm as a dev-dependency only, evm depends on `node`, and
  `ExecutionEngine` follows into the same folder. The constructor change reaches 58 constructions of the context
  builder in node's tests, so a helper that builds it goes first (commit 166), and the port's commit writes the
  new argument once.
- **Commit:** 166 onward. The adapter, `TinyEvmStorageSlotDiscovery`, was first written in node-infra (commit 167):
  evm could not depend on `node` while `node` still depended on evm. Once the execution port's move made evm depend
  on `node` (169), it moved into evm, as the answer has it (170) -- 21 files, past the threshold: a move across
  crates updates its 11 importers at once and cannot be split without a duplicate or a shim.

### 40. "`NoOpExecutionEngine` stays the default in `node-infra`", where the builders default to evm's engine

- **Raised:** commit 170, Part B, the execution seam -- making evm a plug-in.
- **What:** `SETUP.md` has evm become a plug-in while "`NoOpExecutionEngine` stays the default in `node-infra`",
  and describes the no-op as the default today. It is the port's one implementation in node-infra, but neither
  builder picks it unless asked: `EtheramNodeBuilder::new` and `ExecutionEngineBuilder::default` both build evm's
  `ValueTransferEngine`, and system-tests' gRPC node takes `ExecutionEngineBuilder::default()`. So node-infra
  depends on evm in production -- for the builders' `TinyEvm` and `ValueTransfer` variants, and now their slot
  discovery -- and will after the seam.
- **Status:** answered -- the house has a clear default: a refactoring moves no behaviour, and the plan asks for
  none here.
- **Answer:** read as the port's default implementation staying where it is, in node-infra, and not moving into
  evm with the engines. The builders' defaults stay evm's value transfer: making the no-op their default would
  change what every default-built node executes -- system-tests' among them -- which is a behaviour change, not
  this seam's. node-infra is the defaults and the assembly, not a plug-in, so Part B step 8's "no plug-in depends on
  another" still holds with node-infra choosing among evm's engines. Whether an assembly crate may name a plug-in
  at all is the owner's to settle at that gate.
- **Commit:** 170 (no change made).

### 41. The contract suite's kind, and what it may ask of an engine

- **Raised:** commit 171, Part B, the execution seam -- opening `node-contracts`.
- **What:** two things the plan leaves to the run. First, the kind: the suite is test code, but `tests` allows
  only `tests/` directories and no tooling (*What each kind allows*), while the crate has a `src` the engines' tests
  call, a manifest, and its place in stage 1's lists; and SETUP counts `validation/src` and `system-tests/src` as
  production for these checks. Second, the contract: "what every engine owes the protocol", run by both engines
  and the no-op -- but the no-op reports no transaction result at all, so a contract asking for one result per
  transaction would fail the default.
- **Status:** answered -- the house has a clear default for both: SETUP's own reading of a test crate's `src`, the
  merge's precedent for gate lists that follow a structure change (commit 164), and a contract that holds of every
  implementation it is run against.
- **Answer:** the kind is `refactoring` for the crate and for the commit that links the engines' tests to it:
  `node-contracts/src` counts as production as `validation/src` does, a new crate or dev-dependency edge is
  structure, and neither moves a count, a default or an assertion; the clause commits that only grow the crate's
  `src` and tests are `refactoring` by the same reading. The contract asks what the protocol relies on and all
  three engines do: results never outnumber the transactions; result i is credited to transaction i's sender; its
  gas stays within that transaction's limit; every mutation names a sender or recipient of the block, a storage
  write a recipient -- the access plan loads nothing else; and the same inputs give the same results. Fewer results
  than transactions stays allowed: the no-op's answer, which the suite's own tests pin.
- **Commit:** 171 onward.

### 42. The Faction seam: whose vocabulary the port speaks, where the plug-in lives, and how assemblies reach it

- **Raised:** before commit 179, Part B, the Faction seam -- after characterizing the port at 178.
- **What:** `node` names `faction`'s types in three places: the port, `BootstrappingCoordinator::process(Command)
  -> Vec<Outcome>`; the readiness event `ReadinessTransitionObserved { command, transition }`, which system-tests'
  logging observer renders field by field with `{:?}`; and three implementations -- Faction as the coordinator,
  the observer bridge, the factory. SETUP asks for the cluster port, a faction plug-in depending on `node`, and a
  fixed-membership default, ports "shaped by today's call sites" (decision 9). Three questions follow: what the
  port's data model is, where the translation goes, and how 15 files that build a bootstrapper with
  `NoOpBootstrappingCoordinator` and `FactionCoordinatorFactory` reach the plug-in without one move past the
  threshold.
- **Status:** answered -- decision 9 and CLAUDE.md's seam layout (trait, data model parallel to the protocol,
  implementation in its own area) settle the first two; the house's builders and variants in node-infra, which
  already choose the execution engine and the signature scheme, settle the third.
- **Answer:**
  - the data model is node's own mirror of what crosses the port today: `BootstrappingCommand` (Faction's eight
    commands), `BootstrappingOutcome` (its fourteen outcomes), `BootstrappingConclusion` (one),
    `BootstrappingPeerState` (five), and for the event `ReadinessView` (the peer state, concluded, the required
    count -- the three things any observer reads) and `ReadinessTransition` (previous view, outputs, new view).
    Variant and field names are Faction's, so `{:?}` renders as before and the readiness log line stays byte for
    byte; the names were learned from the compiler's exhaustiveness errors in a scratch crate
    (`overhaul-scratch/faction-probe`), not from Faction's sources;
  - the plug-in is a new crate, `node-faction` (`etheram_ibft_node_faction`), on `node` and `faction`: the
    translation both ways, the coordinator -- a wrapper struct, since a third crate may not implement `node`'s
    trait for `faction`'s type --, the observer bridge and the factory;
  - assemblies first build their bootstrapper through node-infra's `BootstrapperBuilder` (Faction by default,
    inactive for a restored node), so the factory's later move rewrites one import; the fixed-membership default
    is the builder's other choice;
  - the fixed-membership default answers as Faction did at 178 for a membership fixed at the validator set, joins
    refused; a contract suite in `node-contracts` pins what both owe the bootstrapper;
  - breaks for the consumer (decision 11): the port's signature, the readiness event's field types, and the
    factory's path; each named in its commit.
- **Commit:** 179 onward.

### 43. The Faction implementations' moves: an observer facet taken early, and one move past the threshold

- **Raised:** commit 186, Part B, the Faction seam -- planning the bridge's and the coordinator's moves into
  node-faction.
- **What:** two costs the plan did not foresee. The observer bridge's nine tests record what it reports through
  node's own test `RecordingObserver`, which node-faction's tests cannot reach; behind the 78-method
  `ProtocolObserver`, an observer of their own is about 360 lines, 77 methods of it doing nothing -- a commit's
  threshold by itself. And the bridge's test file is 205 lines and the Faction coordinator's 212 (eight of them
  pinned at 178): a move counts each line twice, so each move passes 360 lines on its own, and neither can be split
  -- a test file pairs with its source, and the alternatives a second copy of the code or of its tests.
- **Status:** answered -- the plan's own criteria settle the first (`dependency` ranks above the order of steps:
  the plug-in's bridge builds on the observer facet); the threshold's own wording the second ("a commit past it
  lands when its gates pass, and says why").
- **Answer:**
  - the observer split's first facet comes now (commit 186): `PeerDiscoveryObserver`, the facet step 7 names for peer
    discovery, holding the readiness event alone, with `ProtocolObserver` its subtrait -- the composite decision 10
    describes, every implementor implementing the facet in an `impl` of its own. The bridge is handed the facet, and
    node-faction's tests record readiness through a few lines. Step 7 adds the other discovery events to the facet
    and splits the rest; the consumer's observer follows in the same batch;
  - the bridge and the coordinator move in one commit, past the threshold on lines, rather than two: both are
    Faction's implementations leaving `node`, one concern, and the commit is the seam's one move that cannot be
    split. The coordinator becomes a wrapper struct, a third crate not being allowed to implement `node`'s trait
    for `faction`'s type; node's recording observer gives its readiness part to node-faction's tests with the
    bridge's. The port and the event change vocabulary after it, each within the threshold, and `faction` leaves
    node's manifest with the last of them.
- **Commit:** 186 onward.

### 44. The Beggar seam's port: which trait it is, where its vocabulary lives, and who builds the default

- **Raised:** after commit 205, Part B, the Beggar seam -- the characterization, the `#[must_use]`s, the peer
  knowledge, the snapshot apply's split and the three recorded bugs done, the port itself next.
- **What:** SETUP asks for "a port shaped by today's `recovery-control`, which becomes its default", whose trait
  "must not fix who drives a transfer", and gate 8 for `node` on `etheram-core` and `types` alone. Today the
  IBFT side, `recovery-ibft`'s `IbftRecovery`, already holds the control layer as a trait object,
  `Box<dyn RecoveryProcess>` -- `process(ControlEvent) -> ControlOutcome` --, but `node`'s assembly builds the
  default itself (`RecoveryControl::new(coordinator, timing, recorder)`), taking a `BoxedRecoveryCoordinator` and a
  `BoxedDecisionRecorder` as its dependencies, and `node` names about thirty of `recovery-control`'s and
  `recovery-ibft`'s types besides: the host's nine files, the timing config, and fourteen observation records in
  `ProtocolObserver`. Three questions: which trait is the port, where its vocabulary lives, and who builds the
  default.
- **Status:** answered -- decision 9 (Beggar's port shaped by today's call sites), SETUP step 4's wording, and the
  seam pattern CLAUDE.md and the Faction seam (entry 42) set: the contract on the consumer's side, the default
  chosen by node-infra's builders.
- **Answer:**
  - the port is today's call site, `RecoveryProcess::process(ControlEvent) -> ControlOutcome`: the one trait the
    IBFT side drives the control layer through, protocol-agnostic and on `etheram-core` alone, which a Beggar must
    speak too. It already admits a pushed transfer -- a range arriving with nothing asked is admitted by its chain,
    not by a request (`P1-ADR-OnlyTheChainAdmitsARecoveryResponse`), and every intent is optional -- so the step
    pins that with a characterization rather than changing the trait, and gives `process` the `#[must_use]` the
    coordinator's bools have;
  - its vocabulary stays in `recovery-control` for now, the vocabulary crate and the default at once. Splitting the
    two -- the event, the outcome, the intents, the snapshots and the observation records into a crate of their own
    -- moves some thirty-five files and their tests, every line counted twice, and the fourteen observation records
    in it are what step 7's observer split decides a home for; so it is gate 8's work (step 8), after the split,
    with `recovery-ibft`'s place beside `node`;
  - `node` stops building the default: a factory port, `RecoveryProcessFactory::build(self: Box<Self>, timing) ->
    Box<dyn RecoveryProcess>`, in `node/src/traits`, taken by `IbftProtocolDependencies` in place of the coordinator
    and the recorder; the protocol hands it the timing from its own config, so the timing keeps its one source.
    node-infra's `RecoveryControlFactory` builds `RecoveryControl` over the coordinator and recorder it is given --
    today's default, as `FactionCoordinatorFactory` builds Faction's. `node` then names no implementation of the
    control layer, and the recovery boundary's assembly exemption has nothing left to exempt;
  - breaks for the consumer (decision 11): `IbftProtocolDependencies::new`'s signature; named in its commit.
- **Commit:** 206 onward.

### 45. The factory port's home: the node's recovery boundary refuses `node/src/traits`

- **Raised:** commit 208, Part B, the Beggar seam -- the factory port and node-infra's default, the third step of
  entry 44's answer.
- **What:** entry 44 placed `RecoveryProcessFactory` in `node/src/traits`, and its `build` returns
  `Box<dyn RecoveryProcess>`. Stage 1's `cargo xtask recovery-boundary` refused the commit's first attempt with two
  findings, the import and the signature. Its node rule refuses `RecoveryProcess`, `ControlEvent`,
  `ControlOutcome` and `ControlOutput` in `node/src` outside `node/src/ibft/assembly/`. The node drives recovery
  through the IBFT side's entry point and makes none of the control layer's calls, and only the assembly may name the
  layer's trait (RECOVERY-AS-BUILT.md section 9, "The IBFT side's one entry point"). Entry 44 missed this rule.
  CLAUDE.md lists only the first one, which forbids `RecoveryControl`, `RecoveryState`, `RecoveryCoordinator` and
  `BoxedRecoveryCoordinator`.
- **Status:** answered. The gate is the house's, and a gate relaxed to fit a plan's detail buys a green stage with a
  weaker boundary. The trait's folder was the detail: the port, the factory's shape, who takes it and the default all
  stand.
- **Answer:**
  - the trait lives in `node/src/ibft/assembly/recovery_process_factory.rs`, beside `IbftProtocolDependencies`, which
    takes it in the next commit;
  - entry 44's last clause needs one correction. After that commit the assembly no longer names `RecoveryControl`,
    `RecoveryState`, `RecoveryCoordinator` or `BoxedRecoveryCoordinator`. Its exemption from the first rule then
    exempts nothing, and the documents step can say so. Its exemption from the node rule still covers the port's
    name, until gate 8 moves the vocabulary out of `recovery-control`.
- **Commit:** 208.

### 46. The dependencies take the factory: 30 sites, eight recorded duplicate groups, one default

- **Raised:** before commit 209, Part B, the Beggar seam -- the fourth step of entry 44's answer.
- **What:** `IbftProtocolDependencies::new` gives up the coordinator and the decision recorder for one
  `Box<dyn RecoveryProcessFactory>`. 30 sites in 11 files build it. 28 pass a fresh `RecoveryState` and a
  `NoOpRecorder`; one passes its own recorder, and one test helper both. node's test helpers and validation's are
  copies of each other: the dry4rust baseline records seven of them as exact pairs and two of node's as a near pair.
  The same edit to both copies re-keys the seven pairs, the same members under new fingerprints, and pays the near
  pair. Measured on the tree, with every site changed and the factory's default added: 184 lines in 13 files before
  the baseline, about 360 with it.
  The default -- `RecoveryControlFactory::default()`, the one-line form 28 sites take where `new` over the
  pair is four -- joins `cf60ee414b625be5`, the group of the seven `empty_state` helpers in node's tests, which
  build a different struct over two boxed fresh parts.
- **Status:** answered. The split is the house's way under the threshold. Entry 33 is the precedent for recording
  shape-alike members with nothing to share, and entry 35 the precedent for re-keyed pairs.
- **Answer:**
  - commit 209 adds the default and its test. The group is recorded with its eighth member: the same fingerprint, the
    tool's re-record with its closure names put back to `./`, and a diff of that one entry;
  - commit 210 is the wiring. The seven pairs are re-keyed, checked fingerprint by fingerprint, and the paid near pair
    drops, 196 groups to 195;
  - re-recording with `--path` absolute writes closure names absolute: 7 entries, 18 names, each carrying the
    checkout's path. The house form is `./`, which the tool writes from the root. The prefix is restored and
    the diff checked to hold only the group's change.
- **Commit:** 209, 210.

### 47. The certifier's seam: what is left when the port and its default already stand

- **Raised:** after commit 214, Part B, the certifier's seam (SETUP, *Part B*, step 5), the Beggar seam done.
- **What:** step 5 names the seam as "`node`'s `SignatureScheme` port, with `Ed25519SignatureScheme` -- in
  `node-infra` since step 2 -- behind it, deriving every key from the peer id as now", the seam only (decision 5).
  Both pieces stand already. The port has been in `node/src/ibft/crypto` since the base, shaped by its call sites:
  `sign`, `verify_for_peer` and `verify_commit_certificate_signature`, the last asked by name so a scheme can answer a
  certificate apart. The Ed25519 implementation has been in node-infra since `a352577`, which took `ed25519-dalek` out
  of `node`'s tree. The seam template (*Part B*) asks for characterization at the protocol's boundary, the port and its
  data model in `node`, today's logic as the default in node-infra, a contract suite in `node-contracts` every
  implementation runs, and the translation in a plug-in crate. Two questions: which of those are left, and whether
  the port, whose concern is whose key is valid at which height, takes the height.
- **Status:** answered, from decisions 5 and 9 and the seams already built (entries 41 to 44).
- **Answer:**
  - **the port is today's `SignatureScheme`, unchanged, with no height.** Decision 9 shapes the certifier's port by
    today's call sites, and none asks for a height: every key is derived from the peer id and valid at every height,
    which is what the protocol does. A height parameter would be a signature change at every verify site, with
    nothing reading it. It comes with the key variants, which are later work (decision 5);
  - **the boundary is characterized already**, at both tiers, so no characterization commit is owed. Certificates
    signed by a quorum with their own keys verify, and one carrying a signature its signer never made is refused
    (`commit_certificate_verifier_tests`, `validation_tests`). Invalid and forged signatures are refused in every
    message kind (validation's single-node signature and validation tests, and its cluster byzantine tests). A
    cluster recovers blocks committed under real signatures (`ibft_cluster_recovery_tests`);
  - **the data model is `types`' `SignatureBytes`** already, and **the default is node-infra's**: the builder's
    default is the no-op and its variant chooses Ed25519. `NoOpSignatureScheme` stays in `node`, the protocol's own
    no-op as `NoOpBootstrappingCoordinator` is: 43 files name it, and nothing in step 5 or gate 8 asks it moved. Gate
    8's "no crypto implementation in its tree" holds, since it computes nothing;
  - **no plug-in crate**: step 5 places Ed25519 in node-infra, the assembly. A key-variant plug-in comes with the
    variants;
  - **what is left is a contract suite**, `SignatureSchemeContract`, in a `certification` module of `node-contracts`.
    It asks what the protocol relies on and every scheme gives, as entry 41's execution suite does: over a set of
    members, each with its scheme, whatever one member signs every member's scheme accepts as that member's, both as
    a message's signature and as a commit certificate's. A quorum is counted from other members' signatures, so a
    scheme refusing them would stall every round. Refusal is not asked: the no-op accepts everything, and safety
    rests on the scheme a deployment chooses. Ed25519, the no-op and the switched measurement control run it from
    their own tests. `node` takes `node-contracts` as a dev-dependency for the two of them, as node-infra already
    does;
  - then the documents.
- **Commit:** 215 onward.

### 48. The consensus seam's recorded bug: how a held proposal gets a context of its own

- **Raised:** after commit 217, Part B, the consensus seam (SETUP, *Part B*, step 6), the certifier's seam done.
- **What:** SETUP's step 6 names the one recorded bug in the code it moves: "a proposal held in a buffer and replayed
  is judged today under its releaser's context, without the accounts its transactions touch; it is re-queued so the
  runtime builds it its own -- fixed test-first". There are two buffers.
  - The future-round buffer replays on a round advance, under whatever context advanced the round.
  - The pending buffer holds, among votes ahead of their proposal, a later round's proposal that arrived before its
    NewView. It replays on the NewView, under that context made header-only (`PendingPrePrepareReplayContext`).
  Measured at the cluster tier with a block spending a funded account: delivered after its NewView it commits (pinned
  at commit 218). Delivered ahead of its round, or ahead of its NewView, it is replayed under the releaser's context
  and the node stays at height 0: two tests, red on the code as it stands. Nothing in the protocol can re-queue: its
  one output is its actions, and none hands a message back.
- **Status:** answered. SETUP names the fix's shape, and the house's way to give the runtime work is an action.
- **Answer:**
  - **`Action::RequeueMessage { source, message }`**, the held consensus message as it arrived, from the peer who
    sent it. The action partitioner routes it to the node's own work, beside `ExecuteBlock`, so the partitioner's
    three-way shape stands. `EtheramNode` keeps re-queued messages in a queue it serves before its event source, so
    the next step judges the message under a context built for it, before anything else arrives;
  - **both buffers re-queue a held proposal**, and replay a held vote in place as now. A vote needs no account, and
    replaying it unchanged keeps every trace the corpus holds that has no held proposal;
  - **the replay report** for a re-queued proposal says so, with a new blocker, `Requeued`, rather than
    `ReplayProducedNoPrepare`, which would tell the reader a lie;
  - **`ActionKind` gains `RequeueMessage`**, the observer's mirror of the action;
  - **the consumer's breaks** (decision 11): the new action, the new action kind and the new blocker. A runtime that
    partitions or matches actions itself, as the boards do, routes the new one;
  - **what the fix changes**: a held proposal's judgement moves one step later, to the step that serves it with its
    own context. Tests that drove the protocol directly and saw the old replay's actions in the releasing call now
    see the re-queue there. Each is updated to the new behaviour, named in the commit: never relaxed, since what
    replaced the old outcome is asserted as specifically. A trace changes only where the corpus holds a held
    proposal, and a changed trace is accepted only where this commit explains it;
  - the port's design comes after the fix, in an entry of its own.
- **Commit:** 218, 219.

### 49. The consensus port: the quorum, the tallies and the decision, in the step shape

- **Raised:** after commit 219, Part B, the consensus seam, its recorded bug fixed.
- **What:** step 6 puts "the quorum, the tallies and the decision behind a port, in the shape agreed on 2026-10-18 -- a
  step from inputs to outputs, time and randomness outside it". Decision 9 gives consensus the step shape where the
  other bookkeepers kept their call sites' shape. Consensus today is `IbftProtocol`'s own methods, in `consensus.rs`.
  Its tallies are two `VoteTracker`s, prepare and commit, inside `IbftVolatileState`, each built with the quorum.
  Consensus reaches them through about ten volatile-state calls: record a vote; read a key's voters; ask whether a key
  has its quorum; clear them for a new round; replace them at a new quorum when the membership changes; export and
  restore them for the WAL's volatile snapshot. Only `IbftProtocol::new` builds the volatile state. The quorum itself,
  ⌊2n/3⌋+1, is `ValidatorSet::quorum_size()` in `types`, and invariant 1 rests on it. The view-change proofs are
  counted where they are kept, with their signatures, for the NewView's evidence.
- **Status:** answered, from decision 9, the seam template and the house's precedents (entries 42, 44 and 47).
- **Answer:**
  - **the port is `VoteTally::step(TallyInput) -> TallyOutput`**, in `node/src/traits`, `#[must_use]`, one tally per
    phase. Its inputs, in a data model under `node/src/ibft/tally/`, are:
    - a vote cast for a key -- height, round, block hash -- by a voter;
    - a key read;
    - the round cleared;
    - the tally resized to a new quorum;
    - its votes exported for the snapshot, or restored from one.
    Its outputs are a key's tally (its voters, and whether they reach the quorum), the export, or a bare
    acknowledgement. No input carries a time or a random value. The tally holds its quorum, and "reached" is the
    decision. The quorum's value stays `ValidatorSet`'s, handed in at construction and on each resize, so invariant 1
    has one source and nothing in this seam can change it;
  - **the volatile state is the adapter.** Its tally methods keep their signatures and step the port, so `consensus.rs`
    and every test of consensus are untouched, and a characterization commit would only repeat the prepare- and
    commit-quorum tests consensus already has, at both tiers;
  - **the default is node-infra's `QuorumTally`**, today's tracker behind the port, built by a `VoteTallyFactory` (a
    port beside it) that `IbftProtocolDependencies` takes, with a `Default`. That is entry 46's shape again: a one-line
    site, and the dependency helpers' recorded dry4rust pairs re-keyed. The tracker first moves behind the port in
    node-infra by delegation, and then wholly, so no commit holds a copy;
  - **view changes stay where they are**: their proofs are the round change's evidence, kept with their signatures,
    and the certifier's data rather than a tally;
  - **a contract suite**, `VoteTallyContract`, in node-contracts: the quorum-th distinct voter reaches the quorum and
    the one before does not; a voter counts once; keys do not mix; a cleared or resized tally starts empty; an export
    restored answers as it did. `QuorumTally` runs it;
  - **in order**: the port and its data model; the default by delegation, with its factory; the dependencies taking
    the factory and the volatile state stepping the port; the tracker moved into the default; the contract and its
    run; the documents, retiring the replayed-proposal open point with the rest;
  - **the consumer's breaks** (decision 11): `IbftProtocolDependencies::new` gains the factory, and
    `IbftVolatileState`'s constructors take the two tallies. Both are named in their commits.
  - **amended at commit 222: a read beside the step.** A key's tally and the WAL's export are asked from where
    consensus holds itself by reference: the commit certificate is built in a `&self` method, and the volatile
    snapshot is `&self`, called by some sixty tests on a protocol they only borrow. Routed through the `&mut` step,
    both would spread `&mut` across consensus and its tests. So `read(&self, TallyQuery) -> TallyOutput` answers a
    key or the export, and the step keeps what changes a tally. Neither takes a time or a random value. The
    volatile state's tests first build their state through two helpers, so the wiring changes each construction
    in one place.
- **Commit:** 220 onward.

### 50. The observer's split: which facet each method joins, and in what order

- **Raised:** after commit 231, Part B step 7, the consensus seam done.
- **What:** `ProtocolObserver` (`node/src/observer/protocol_observer.rs`) declares 77 methods and inherits an
  78th, `readiness_transition_observed`, from `PeerDiscoveryObserver`, the facet entry 43 split out for Faction's
  bridge. Six types implement it, each listing the methods in the trait's order:
  - node-infra's `NoOpObserver`;
  - system-tests' `LoggingObserver`;
  - validation's `PruneRecordingObserver`;
  - node's test `RecordingObserver`;
  - validation's two test observers.
  node-faction's test observer implements the facet alone. Step 7 names five concerns: consensus, recovery,
  retention, peer discovery and the output dimensions' refusals. Decision 10 composes them by one composite trait
  with no body, implemented explicitly in each implementor's file, the node handing each component its facet by
  trait upcasting.
  One method fits none of the five: `diagnostics_sampled`, the periodic beacon. Its comment calls it "unlike every
  other hook here": a timer drives it rather than an event. Its one sample carries consensus's round and
  signatures, recovery's requests and in-flight slot, and retention's retained heights.
  Others sit between concerns:
  - the admission of a peer message: duplicates, sequences, the height window;
  - the two buffers and their replays;
  - the timers' dispatch;
  - the pool's admission of a transaction;
  - the membership update a committed block applies.
  Three components report to one concern alone, by their calls:
  - the bootstrapper to peer discovery;
  - the recovery host's observation dispatch to recovery;
  - the reporting output to the refusals.
- **Status:** answered, from step 7, decision 10, and the open point the step answers.
- **Answer:**
  - **six facets**, each a trait of its own file in `node/src/observer/`:
    - `PeerDiscoveryObserver`, today's facet, gaining the bootstrapper's seven: `node_started`,
      `peer_discovery_started`, `peer_confirmed`, `peer_discovery_completed`, `peer_discovery_timeout`,
      `peer_rejoin_acknowledged`, `join_transition_observed`. node-faction's test observer gives the seven empty
      bodies, which is what a facet costs inside its own concern;
    - `ConsensusObserver`, 40 methods. It takes the proposal, the votes, the certificates, the view change and the
      commit. It also takes what the protocol does to admit and hold a consensus message: the admission checks, the
      two buffers and their replays, and the timers' dispatch. With them go the pool's admission and
      `validator_set_updated`, which a commit applies;
    - `RecoveryObserver`, 25 methods: recovery's 18, `peer_banned` and `recovery_check_decided` among them, and the
      seven snapshot methods, snapshot recovery being recovery's last resort;
    - `RetentionObserver`: `history_pruned`;
    - `OutputRefusalObserver`: the three refusals, with their comment;
    - `DiagnosticsObserver`: `diagnostics_sampled`, with its comment. It is a sixth facet rather than a place in one
      of the five, since its sample is every concern's: in any one facet it would report the others' state. The
      composition is still decision 10's, over six;
  - **`ProtocolObserver` becomes the composite**: the six as supertraits and no body, with
    `impl ProtocolObserver for X {}` in each implementor's file. There is no blanket implementation;
  - **one facet a commit, from the trait's tail**, each a `refactoring` that moves no count:
    - the refusals, then the beacon, then retention;
    - peer discovery, from the head;
    - recovery;
    - consensus last, after which the composite is empty.
    Each commit cuts the implementors' one impl block where the facet's methods already stand, so no method body
    moves;
  - **then the narrowing**, one commit each. Each of the three components named above takes its facet alone,
    upcast from the node's observer where it is built. A component that reports to several facets keeps the
    composite;
  - **then the documents**. The open point "The Observer trait is one surface for five concerns" is closed in
    this repository and retired. The consumer's own observer follows after the experiment, with its integration,
    as the entry asks;
  - **the consumer's break** (decision 11) is the facet traits to implement and import, and each narrowed
    signature. Each is named in its commit.
- **Commit:** 232 onward.

### 51. `paired-test-file` in validation and system-tests: the scenario sources, without renaming the tests

- **Raised:** after commit 241, Part B, the skipped rules. They come before step 8's gates, which ask every rule
  on. `risk` puts this slice first of the three.
- **What:** SETUP's slice reads "each `<scenario>_scenario_tests.rs` pairs with a `<scenario>_scenario.rs` in `src`
  that hands its tests the cluster they start from". With every rule on, the rule reports 100 test files. They are
  of two kinds:
  - **scenario tests**, about 80. These are validation's `tests/cluster/**` and `tests/single_node/**`, and
    system-tests' `tests/cluster/*`, each named for a scenario no source carries. Their sizes run from 21 to 2,306
    lines; validation's cluster files alone sum to about 15,000;
  - **tests of a source that exists at another path**, about 20. These are validation's seven `tests/harness/*` for
    sources at `src/`'s root, and system-tests' `tests/infra/**` for sources one folder deeper than their tests.
    Among them, `protocol_helpers_setup_tests.rs` is named for no source at all.
  SETUP's names rename every scenario test file. The threshold counts a commit's lines with `--no-renames`, so a
  rename counts the whole file removed and added again. The renames alone would be about 30,000 lines, each file a
  commit past the threshold and most of them several times over. That is the experiment's whole 5% allowance
  several times over.
- **Status:** answered, from SETUP's slice, the threshold rule (*The rules for every commit*), and the evm slice's
  precedent for kinds.
- **Answer:**
  - **a scenario source takes the test file's own stem**, at the path the rule asks:
    `tests/cluster/consensus/ibft_cluster_basics_tests.rs` pairs with
    `src/cluster/consensus/ibft_cluster_basics.rs`, and so on. No test file is renamed, so no test's name and no
    file's history moves. The content is SETUP's: the cluster, or the single node, the file's tests start from, as
    public functions their Arrange calls in place of the setup they repeat. A test whose setup is its own keeps it.
    The tests are the source's tests, so validation's `tested-public-api` holds each function to a caller. The `_scenario`
    of SETUP's names is the one part not kept, since keeping it costs the renames;
  - **a test of a source at another path moves to its source's path**, unchanged but for its module registration:
    validation's `tests/harness/*` to `tests/` beside its source's root, and system-tests' `tests/infra/**` one
    folder down. `protocol_helpers_setup_tests.rs`'s tests move into `protocol_helpers_tests.rs`. A move counts its
    lines twice, so a commit moves files of about 180 lines in all;
  - **kinds**: a commit that lowers `paired-test-file`'s everything-on count is a `rule-offence` with
    `Rule: paired-test-file`, as the evm slice's were with `returned-mutations` stood down. The stand-down comes
    out of both packages' sections in a `support` commit once the count is zero, and the stern config's comment
    against the rule goes with it;
  - **order**: the moves first, validation's then system-tests', since they are mechanical and small. Then the
    scenario sources, one test folder a commit, or two small ones, within 18 files and 360 lines;
  - **dry4rust**: a fixture that leaves a test for `src` becomes code dry4rust reads. A fixture repeated across
    scenario files becomes one function in `scenario_helpers.rs` that the sources call, rather than a recorded
    duplicate.
- **Commit:** 242 onward.

### 52. The scenario sources and `tested-public-api` in system-tests

- **Raised:** at commit 267, Part B, the skipped rules, the first commit of entry 51's scenario sources.
- **What:** a scenario source makes the harness call its tests made: `ClusterProcessHarnessBuilder::new(..)
  .with_scheduled_validator_update(..).spawn()` moved from `validator_set_update_tests.rs` into
  `src/cluster/validator_set_update.rs`. `tested-public-api` is stood down in system-tests, but with every rule on it
  counts a public function no test calls. It matches a call by name and arity, so it no longer sees one for
  `with_scheduled_validator_update`, and its count rises from 17 to 18. `with_retention` and
  `with_min_servable_history` moved too, but `ClusterConfigFile`'s same-named methods have tests the rule matches.
  Other calls only scenario setups make will follow as their sources land, among them
  `spawn_with_malicious_node`, `spawn_with_env_extras`, `spawn_with_env_extras_and_config` and
  `with_certificate_check_bypassed`. The next slice in `Over:` is `tested-public-api` in system-tests.
- **Status:** answered, from SETUP's slice, entry 51 and CLAUDE.md's *Do not buy a green gate with lost coverage*.
- **Answer:**
  - **the sources stay as entry 51 has them**, each handing its tests the cluster they start from. SETUP's slice
    asks exactly that, and the alternatives are worse: a source the tests do not call is empty, and one that hands
    back a half-built builder for the test to finish keeps the setup in the test;
  - **no coverage is lost and no visibility changes**: every such call is still made by the same test, one call
    deeper, through a public function of the package. CLAUDE.md's rule is about dropping a function to
    `pub(crate)` to quiet the gate, which none of this does;
  - **each commit names what the rule stops seeing**, so the count's rise is accounted for where it happens. A
    commit stays a `rule-offence` as long as the everything-on total falls;
  - **the `tested-public-api` slice, next, takes them** with the 17 it already had: a test of each that calls it
    directly, or the stand-down kept with its reason widened, decided there.
  - **added at commit 271:** SETUP's `rule-offence` lets another rule's count rise only within the total's fall,
    named here. So a source holds only what its tests start from: a harness call in a test's Act -- the operation
    under test, such as `timer_restart`'s `build_restart_from_db` -- stays in the test. A commit whose rise would
    match its fall is reshaped until the total falls. The calls the rule has stopped seeing, by commit:
    - 267: `ClusterProcessHarnessBuilder::with_scheduled_validator_update`;
    - 268: `ClusterProcessHarness::spawn_with_malicious_node`, `ClusterProcessHarness::spawn_with_env_extras_and_config`,
      `ClusterConfigFile::with_block_limits`;
    - 269: `ClusterProcessHarness::spawn_with_env_extras`;
    - 271: `RuntimeTimerHarness::build_single_node`, `ClientProcessHarness::spawn_single_node`,
      `ClientProcessHarness::spawn_single_node_with_default_config`.
- **Commit:** 267 onward.
