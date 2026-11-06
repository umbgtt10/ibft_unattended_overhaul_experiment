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
