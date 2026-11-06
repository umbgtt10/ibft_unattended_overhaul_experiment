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
