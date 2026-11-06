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
| 5 | commit 27, Part A | the five harness tests restored lower a count -- `tested-public-api`, stood down in `system-tests` as configured -- so no `tests` commit can carry them | answered | `rule-offence`, `Rule: tested-public-api`: calling an untested public function from a test is the rule's own fix; the count falls with everything on and cannot move as configured, where the rule is stood down |

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
