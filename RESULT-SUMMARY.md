# IBFT unattended overhaul experiment -- result summary

Written at the end of the run, from Part C's measurements, the history's re-derivation and the workbook; no figure
is copied by hand. Each line answers the bullet of the same number in `SETUP-SUMMARY.md`. The figures come from
these, run on the end, `2d969b8`, and on this repository's files:

```
cargo xtask result-figures WORKBOOK.csv WORKBOOK-ATTEMPTS.csv REDERIVATION.tsv
cargo xtask tree-change 8a775a2 2d969b8
cargo xtask size 2d969b8
cargo xtask test-inventory 2d969b8
cargo xtask transcript-usage <the session transcript>
```

and from the final checks report, `etheram-ibft`'s `docs/history/[IMPLEMENTED] OVERHAUL-2026-11-FINAL-CHECKS.md`:
section 1 for the black-box run, 2 for the traces, 3 for coverage, 4 for the offences, scores and environment at
both ends, 5 for the re-derivation.

**The bet**: won. The run completed Parts 0 to C with no intervention from the owner. No test was lost and no
assertion relaxed outside constraint 6's licence: the 30 tests removed and the 2 that say less are each named by a
`Maps:` line, and the re-derivation finds none unmapped. Every test was green at every commit: the
re-derivation ran all three stages at each of the 365 commits, not only the stages each one reached, and every one
was green. One red belongs beside this. In stage 5's thirty measuring runs at `2f7a771`, one failed. That was a
measurement, not a commit's validation, and the red had a cause: the base's own port allocator could hand out one
port twice. `b9d0bdf` fixed it, test-first.

| # | measure | at the base | predicted | actual | verdict |
|---|---|---|---|---|---|
| 1 | commits | -- | about 200 (165-260), nearer 220; 10-30 unplanned | 365 -- Part 0 25, A 125, B 187, C 28; 19 unplanned | missed, above the range; the unplanned share within |
| 2 | within 18 files and 360 lines | -- | at least 95% | 351 of 365, 96.2%; the 14 past it each declare why -- 5 of them the planned exceptions (12, 25, 26, 164, 169) | met |
| 3 | behaviour | -- | no change outside `bug-fix` commits, as far as the tests see: every test green at every commit on the stages it reaches, and at the end; the base's cluster tests and the reference traces hold on the final code | every commit's three stages green, re-derived; each of the 7 `bug-fix` tests red on its parent; the base's `system-tests` 398 of 398 against the final binary; 18 of 18 traces byte for byte | met |
| 4 | bugs | 4 recorded | the 4, and 10-20 found | the 4 fixed (`5013d0e`, `1e7ac29`, `643bfd5`, `8e1ca00`); 3 found and fixed (`eba863f`, `e1232ca`, `b9d0bdf`), and one in Part C's own coverage tool (`d5ff011`); no protocol decision | the 4 met; the found missed, 3 for 10-20 |
| 5 | tests | 3,440; coverage per crate | none lost and none relaxed outside the licence, every use named; 10-18% added; coverage not lower; every function covered at the base still covered | 3,974, +15.5% (564 added, 30 removed, 2 saying less, all 32 mapped); the shipped crates' lines 96.69% to 97.23%, no crate lower; no function covered at the base uncovered at the end, the 35 not held each with its successor | met |
| 6 | offences | 483 / 696 / 13, under 39 rules | zero, the harness's time reads excepted, held in the baseline | 33 / 33 / 0: the 20 time reads, and 13 held by entries 9, 25, 31 and 37 | cargo-shear met; stern missed by 13 |
| 7 | Grip/Braintax | 0.1425 | 0.150 (0.145-0.160) | 0.1627 (791.40 / 4,865.29), +14.2% | missed, above the range: more gain than predicted |
| 8 | autonomy | -- | no stop for the owner; questions answered on the fly or left open; no environment stop; no intervention | no stop for the owner; 59 questions, 54 answered on the fly, 5 left open; no environment stop recorded; no intervention | met |
| 9 | wall-clock | stages 1 min 25 s-1 min 30 s after a change, the run's laptop at the base | about a day (20-65 hours), about 26 hours; about 3.5 minutes a commit | 45 h 41 min, 2026-11-06 16:00Z to the last commit's validation at 2026-11-08 13:41Z; 7.5 minutes a commit -- 2.8 implementing, 1.6 validating by the workbook's stamps, the rest Part C's long checks, the record and the reading between; no usage-limit wait | within the range; the point missed |
| 10 | raised and answered or implemented | -- | 25-50 | 54 | missed, above the range |
| 11 | raised and left open | -- | 10-25 | 5 (entries 9, 25, 37, 57, 59) | missed, below the range |
| 12 | failed attempts and flakes | none in 30 stage 5 runs at the base | reported, not predicted; 0-4 flakes; 0-3 parked | 5 attempts refused of 370 -- stage 1 clippy 1, stage 2 dry4rust 3, stage 1 recovery boundary 1 --, 167 seconds in all; 0 flakes; 0 parked | met |
| 13 | the agent's resource use | -- | 1,500 calls; 2.2 million output; 0.75 billion cached input; 3-8 compactions | 9,164 calls; 7,906,908 output tokens; 4,428,431,829 cached input tokens read, 16,591,858 written, 18,708 uncached; 18 compactions -- one session, read at 2026-11-08 13:45Z; the cost not priced (entry 59) | missed, every figure above its range |
| 14 | files added | 1,071 files | about 360 (265-475); summed about 375 | 437 -- productive 79, test 236, tooling 116, other 6; summed 449 | within the range |
| 15 | files removed | -- | about 90 (50-200); summed about 105 | 60 -- productive 21, test 38, other 1; summed 72 | within the range |
| 16 | files changed | -- | about 650 (500-800); summed about 1,700 | 635 -- productive 240, test 320, tooling 51, other 24; summed 2,566 | base to end within; summed missed, above 1,250-2,350 |
| 17 | productive lines | 25,648 in 406 files | about 31,000 (28,500-34,500) | 27,739 in 464 files, +8.2% | missed, below the range |
| 18 | test lines | 109,966 in 491 files | about 122,000 (116,500-130,000) | 121,111 in 689 files, +10.1% | within the range |

## Every commit, re-derived

Part C's last check: every commit of the branch re-derived from the history alone -- its counts, its kind against its
diff, its inventory and assertions, its traces, its scores, its three stages -- beside what the record says, every
mismatch as found, never corrected. `REDERIVATION.tsv` holds the 365 rows beside `WORKBOOK.csv`'s; the report's
section 5 tells how they were taken.

- every stage green at every commit: 365 of 365, all three;
- traces byte for byte at the 341 commits from the driver's landing, commit 25; the 24 before it have no driver;
- tests 3,440 to 3,974; 564 added, 30 removed, 2 saying less, every one mapped; none unmapped;
- 351 within the threshold by their own diff; kinds 166 `rule-offence`, 111 `refactoring`, 61 `support`, 20
  `tests`, 7 `bug-fix`;
- summed over the commits, renames off: 449 files added, 72 removed, 2,566 changed; 56,312 lines added, 31,914
  removed;
- each `bug-fix` commit's test, run on an export of its parent with the commit's test files laid over it, fails
  there: `eba863f` 1 of 1, `e1232ca` 7 of 82 and 1, `5013d0e` 1, `1e7ac29` 3 of 60, `643bfd5` 1, `8e1ca00` 2 of 13,
  and `b9d0bdf`'s, run when it landed, 942 distinct ports of 1,000.

The rows that differ, from the commits' claims or from the workbook:

| # | commit | kind | record | re-derived | mismatch |
|---|---|---|---|---|---|
| 13 | `1663af1` | `rule-offence` | `Rule: cargo-shear unused_dependency, unused_workspace_dependency`; cargo-shear's findings 13 to 0 | the kind check: the named rules 0 to 0 and the total 696 to 696 with everything on | three breaches, the check's own: it reads the named rules from stern's counts, and these are cargo-shear's, which fell 13 to 0 in the same row |
| 122 | `1d27ff1` | `refactoring` | threshold `exceeded` | `exceeded -- the 36 accessors and the types crate's own tests ...` | the workbook keeps the verdict without the trailer's reason |
| 164 | `8afa421` | `refactoring` | threshold `exceeded` | `exceeded -- the planned move out of execution-types ...` | the same |
| 169 | `4cfcce2` | `refactoring` | threshold `exceeded` | `exceeded -- the planned move of ExecutionEngine into node/src/traits ...` | the same |
| 170 | `2883602` | `refactoring` | threshold `exceeded` | `exceeded -- the adapter's move across crates ...` | the same |
| 177 | `a352577` | `refactoring` | threshold `exceeded` | `exceeded -- the scheme's move across crates ...` | the same |
| 187 | `d7d2c45` | `refactoring` | threshold `exceeded` | `exceeded -- moving the coordinator and the bridge out of node ...` | the same |
| 243 | `857cb80` | `rule-offence` | `Lines-Removed: 145`, in the trailer and the workbook | 143 | the sum written into the trailer by hand was two out; the numstat the session read then, in its transcript, gives 32, 109 and 2 |

The verdict agrees in all fourteen `exceeded` rows; the eight not listed copy the reason too. No row is red, and no
mismatch touches the bet.

## Stops, waits and interventions

- **Stops for the owner:** none. Every question went to `QUESTIONS.md` and the run went on.
- **Stops forced by the environment:** none recorded -- no key, machine or network stop, no usage-limit wait. No
  stop condition was reached: not the seven days, not twelve hours without a commit, not the disk.
- **Interventions:** none. `QUESTIONS.md`'s interventions table is empty: no message from the owner during the run.
- **Compactions:** 18 of the session's context, the run resuming from `RUN-STATE.md` after each; not stops.

## The first question

Can a migration of about 200 commits run unattended, with no behaviour change and no lost test?

**Here, once: yes, at 365 commits.** The run took 45 h 41 min and ran without a stop. Nothing it did waited for
the owner, and the owner sent nothing: 59 questions arose, 54 answered on the fly and 5 left open, none stopping
the run. As far as the tests see, behaviour did not change outside the seven `bug-fix` commits. Every commit
was green on all three stages when re-run from the history. The base's `system-tests` pass against the final
binary, all eighteen reference traces replay byte for byte, and every `bug-fix` test fails on its parent. No test
was lost: each of the 32 removed or weakened is mapped to the entry that licensed it. The evidence is the
history's, re-derived at the end, and not the record's: where the two differ, the record had erred, in one
trailer's sum and six abridged rows, none bearing on behaviour or tests.

What the answer does not cover: one codebase, one run, one agent who planned, ran and audited; mutation testing
out of scope, so whether the tests would notice a change is not measured; and behaviour no test, trace or cluster
run exercises. The run was also far larger than planned -- 365 commits for about 200, 9,164 model calls for about
1,500 -- so "about 200 commits" is answered by a run nearly twice that size.

## The second question

Does the structure measurably improve?

**Yes, by every measure taken at both ends.** Over the shipped crates the Grip/Braintax ratio rose from 0.1425 to
0.1627, +14.2%: Grip 642.59 to 791.40, Braintax 4,510.09 to 4,865.29. It rose in every crate but `node-infra`,
0.255 to 0.237. The rules block gave most of
the gain, 0.1425 to 0.1592; the restructuring added 0.1592 to 0.1627. The prediction expected the opposite. The
measures the house does not own agree:

- stern4rust's offences 483 to 33 as configured and 696 to 33 with everything on;
- cargo-shear 13 to 0;
- dry4rust's groups 217 to 194;
- iceberg's total risk 299.27 to 285.09;
- the shipped crates' line coverage 96.69% to 97.23%;
- tests 3,440 to 3,974 -- Part 0 +81, A +149, B +220, C +84.

All of it is measured by instruments the same hands built, or by counts beside them. None is an independent
judgement of the design.

## Raised and answered or implemented

From `QUESTIONS.md`, every entry the run answered on the fly: the question, the answer, and the commit that carried
it where it caused a change -- by ordinal, the workbook giving each its hash.

| # | question | answer | commit |
|---|---|---|---|
| 1 | slotgate's per-test timeout, 120 to 160 seconds | the plan's own change, named; no wait's budget moves | 15 |
| 2 | which kind carries the cluster harness's richer wait-failure message | `refactoring`: only the text a failing wait panics with changes | 16 |
| 3 | which kind carries the single-node ports moving into the slot's range | `refactoring`: arbitrary ports stay arbitrary | 17 |
| 4 | five conflict tests pass with conflict detection disabled | each given the setup or assertion only detection explains | 18-23 |
| 5 | restored harness tests lower `tested-public-api`, so no `tests` commit can carry them | `rule-offence`, the rule's own fix | 27 |
| 6 | the cluster waits fail on 60 s without progress | the plan's own change; the budget kept as the backstop | 31 |
| 7 | a recorded dry4rust group reshaped by a clock argument | the baseline re-recorded, the same group under its new fingerprint | 34 |
| 8 | the gRPC buses' wake tests timed a wait | they assert the notification itself | 41 |
| 10 | three test waits change shape onto `PollingWait` | every ceiling kept or replaced by its event | 42 |
| 11 | five fixed delays with no event to wait for | through the sleeper port, unchanged, each named | 43 |
| 12 | the cluster tests' last four waits onto `PollingWait` | each window grows by at most one poll | 44 |
| 13 | factories that cannot fail stop returning `Result` | tests check what was built; one `is_ok()` mapped | 45 |
| 14 | two assertions move with the transport's failure type | the same contracts, checked on what exists | 47 |
| 15 | five assertions move with partitioning's failure type | the same words; the `is_err()` names its failure | 48 |
| 16 | the client-interface codec's refusals checked `is_err()` alone | each names its failure | 50 |
| 17 | the node failure's words test builds a storage failure | the same contract on the store's failure | 51 |
| 18 | the storage codec's refusals compared as failures | compared as the failure itself | 52 |
| 19 | the cluster config's refusals read through their text | the same text through `to_string()` | 54 |
| 20 | the gate sequence's failure and its two tests | the same claim, through the new type | 55 |
| 21 | the gate tests read a failure through its text | the same words through `to_string()` | 56 |
| 22 | the fake gate fails with a gate's failure | the same claims of a real failure | 59 |
| 23 | the runner's launch failure read through its text | the same check through `to_string()` | 60 |
| 24 | the CRAP parser's failures read through their text | the same words; the type's tests pin the text | 62 |
| 26 | three block builders' recorded duplicate reshaped | the baseline re-recorded, fingerprint by fingerprint | 69 |
| 27 | the receipt summary's two tests compare it by name | the same four counts, named | 71 |
| 28 | the SHA3 window's test compares it by name | the same three values, named | 73 |
| 29 | the harness's node heights print as the pairs they were | the record prints as the pair did, pinned | 75 |
| 30 | ten opcodes a re-export shim counted as shared | private where they are read, as stern's corrections say | 79 |
| 31 | the diagnostic beacon's field table | stays beside its struct, its offence in the baseline | 81 |
| 32 | the bootstrapper is handed its coordinator | `init` takes it; the dry4rust baseline re-keyed | 92 |
| 33 | two configs' defaults alike in shape | the pair recorded in the dry4rust baseline, named | 96 |
| 34 | a getter's test the record-alone rule refuses | removed under the licence, mapped to the test covering its claim | 97 |
| 35 | the in-flight slot's getters reshape a recorded pair | the pair re-keyed | 105 |
| 36 | the hard state drops a pending block's transaction data | a bug: fixed test-first, the layout moved on | 116 |
| 38 | what "return what they change" means for the interpreter | the machine's state one object, its operations its methods | 151-163 |
| 39 | the storage access plan would close a cycle | a `StorageSlotDiscovery` port in `node/src/traits` | 166-168, 170 |
| 40 | "`NoOpExecutionEngine` the default" where the builders default to evm | read as the port's default implementation; the builders unchanged | -- (raised at 170) |
| 41 | the contract suite's kind, and what it may ask | `refactoring`; only what all three engines do | 171 |
| 42 | the Faction seam's vocabulary and home | `node` mirrors it; a `node-faction` plug-in | 178-195 |
| 43 | the Faction moves: a facet early, one move past the threshold | the facet first, then one unsplittable move | 186, 187 |
| 44 | Beggar's port, its vocabulary and its default | today's `RecoveryProcess`; a factory with node-infra's default | 206-214 |
| 45 | the factory port's home against the recovery boundary | the gate stands; the trait in the assembly | 208 |
| 46 | the dependencies take the factory | two commits: the default, then the wiring | 209, 210 |
| 47 | the certifier's seam already standing | a contract suite every scheme runs; no height | 215-217 |
| 48 | a held proposal judged without its accounts | re-queued as an action, judged under its own context | 219 |
| 49 | the consensus port in the step shape | `VoteTally`, one per phase, `QuorumTally` its default | 222, 223 |
| 50 | the observer's split into facets | six facets under a composite, one a commit | 232-241 |
| 51 | `paired-test-file` without renaming the scenario tests | a source per test file's stem | 242-305 |
| 52 | scenario sources and `tested-public-api` | the sources stay; the next slice takes the calls | -- (raised at 267) |
| 53 | a public helper counted tested through a namesake | an unplanned `tests` commit gives it a test | 300 |
| 54 | system-tests' 25 untested public functions | each its own test, none made private | 306-314 |
| 55 | a scenario named for an event it cannot see | the builder's test asserts the event; the scenario listed | 313 |
| 56 | the gates: what step 8 asks and what the run can pay | `returned-mutations` paid; a dependency gate; the owed named | 325-335 |
| 58 | how each of Part C's checks is run | a report a section a check; tools in xtask; the re-derivation in chunks | 338-365 |

## Protocol decisions taken along the way

None. No suspected bug was raised or answered as a protocol decision under decision 12. The seven `bug-fix`
commits are the four recorded bugs and three found, each against a contract its code or its tests already stated.

| # | suspected bug | decision taken | test | commit |
|---|---|---|---|---|

## Raised and left open -- delivered as a result

From `QUESTIONS.md`, every entry the run left open, for the owner to implement, discuss, discard, postpone or move
into `etheram-ibft`'s `OPEN_POINTS.md`.

| # | question | course taken meanwhile | recommendation |
|---|---|---|---|
| 9 | the wake signal's six tests time a timed wait, and no timeless rewrite says as much | they stay in the stern baseline, `timeless-tests` | accept them beside decision 2's exception: the one place the subject is a timed wait |
| 25 | one file per implemented type: `IbftProtocol` (13 files) and `RecoveryControl` (5) -- merge, or split? | both offences in the baseline; the run went on | split, a type per concern, the protocol's split taken into the seams |
| 37 | `NodeDiagnostics`: a constructor of 28 fields | the type as it is, its offence in the baseline | group the fields into the records its comments already name, and a constructor of those |
| 57 | where `recovery-ibft` goes, and the control layer's vocabulary | `node` keeps both recovery crates; the dependency gate names them owed | `recovery-ibft` into `node`, the control vocabulary mirrored and translated by a plug-in; the hash stays in `types` |
| 59 | prediction 13's cost at the list in force at the end, which the run cannot read | the counts given by kind, the cost unpriced | price them at the list of 2026-11-08 in a dated addendum here |

## What the experiment could not see

- **Behaviour no test, trace or cluster run exercises**, as `SETUP.md` names. Mutation testing was out of scope, so
  whether the tests would notice a change is not measured.
- **The corpus holds no held proposal.** `8e1ca00`, the one fix to consensus's behaviour, moves no trace, and the
  protocol-tier check could not have seen the bug it fixes either.
- **The black-box check reaches the binary through process tests only.** The base's in-process tests run the base's
  own code, as they always did.
- **The kind check reads stern's counts.** A `rule-offence` that pays another tool's findings is beyond it: commit
  13's breaches are the check's, not the commit's.
- **A scenario's name can claim more than its test checks.** Entry 55 left one such scenario as it was and listed it.
- **One agent planned, ran and audited.** The re-derivation is the history's, not the record's, and it found the
  record's two slips. But it was written and run by the same agent, and nothing rules out the same error made twice.
- **The cost is unpriced** (entry 59).
