# IBFT unattended overhaul experiment -- setup summary

Draft; frozen with `SETUP.md` at the run's start. Each
bullet is a measure, its value at the base of `etheram-ibft`'s branch, and the value predicted at
the end. `RESULT-SUMMARY.md` answers them in the same order.

**The bet**: the run completes Parts 0 to C with no intervention from the owner, no test lost and no assertion relaxed outside the plan's
licence (each use named in `QUESTIONS.md`), and every test green
at every commit. It starts at the time its `Freeze:` commit names, and this repository's `main` carries its record as
it goes.

1. **Commits:** about 200 on the branch (165-260), read as nearer 220 -- Part 0 about 20, the rules block about 75,
   the restructuring about 100, the final checks about 4; of them 10-30 unplanned. The experiment ends when all its
   work in `etheram-ibft` is done; the consumer comes after it, outside it. Mutation testing is out of
   scope.
2. **Size:** at least 95% of the commits within 18 files and 360 lines; the planned exceptions are the dry4rust
   baseline, which the tool records whole, the trace driver's landing, the two moves out of `execution-types` and any
   mechanical `--fix`.
3. **Behaviour:** no change outside `bug-fix` commits, as far as the tests see -- every test, the base's, the new
   ones and the changed ones, green at every commit on the stages its change reaches, and all of them at the end;
   every `bug-fix` with a test red on its parent; and the final checks hold -- the base's cluster tests pass against
   the final binary and every reference trace replays byte for byte, each exception a named `bug-fix`. All of it
   re-derived from the branch's history at the end, every commit's three stages run again, and every mismatch with
   the record reported.
4. **Bugs:** 4 recorded bugs fixed where their code moves, and 10-20 more found on the way -- a suspected bug that
   is a protocol decision among them, fixed test-first and reported at the end.
5. **Tests:** 3,440 at the base (the inventory sets the figure); none lost and no assertion relaxed outside the licence, every use named; 10-18% added; coverage not lower,
   overall or per crate, and every function covered at the base still covered where its code still exists.
6. **Offences:** 483 as configured, all in the baseline, and 696 with everything on, under stern4rust 0.20.1's 39
   rules;
   13 `cargo-shear` findings -- all three to zero, the cluster harness's time reads excepted, held in the baseline.
7. **Grip/Braintax:** 0.1425 (643 / 4,510) to 0.150 (0.145-0.160), +5% (+2% to +12%).
8. **Autonomy:** the run never stops for the owner; every question it raises goes to `QUESTIONS.md`, answered on
   the fly or left open and delivered with the implementation; stops forced by the environment counted with their
   cause -- predicted none; no intervention, no message from the owner during the run.
9. **Time:** about a day of continuous wall-clock (20-65 hours), about 26 hours, one agent working serially with
    no parallelism of any kind: about 3.5 minutes a commit -- 1 implementing, 1.5 validating (the three stages
    measured at 1 minute 25 seconds to 1 minute 30 seconds after a change, on the run's laptop at the base), 1 for
    failed attempts, flakes, the checks, scoring and the record's push -- over about 200 commits, plus about 11 hours of
    fixed work, the final checks 8 of them. Active time: a usage limit's waits are reported apart.
10. **Raised and answered:** questions and open points the run raises and answers on the fly, or implements -- 25-50,
    each recorded in `QUESTIONS.md` with its answer and, where it caused a change, its commit.
11. **Raised and left open:** questions and open points the run raises and does not answer -- 10-25, delivered with
    the implementation as one of the experiment's results, each with the course its slice took meanwhile and the
    run's recommendation.
12. **Failed attempts and flakes:** failed attempts reported, by cause, not predicted; 0-4 flaky reds; 0-3 slices parked; none in 30 stage 5 runs at the
    base.
13. **The agent's resource use:** about 1,500 model calls (1,050-2,600), 2.2 million output tokens (1.6-3.6
    million), 0.75 billion cached input tokens read (0.4-1.5 billion), 3-8 context compactions, over about 200
    commits, measured from the session transcripts.
14. **Files added**, base to end, a move counting as one removed and one added: about 360 (265-475) -- productive
    125, test 195, tooling 30, other 10; summed over the commits about 375. The trace driver's 6 files are in the
    test figure; the corpus stays on the branch it was recorded on and counts nowhere.
15. **Files removed:** about 90 (50-200) -- productive 45, test 40, the rest tooling and other; summed about 105.
16. **Files changed:** about 650 (500-800) of the base's 1,071; summed over the commits about 1,700 (1,250-2,350).
17. **Productive lines:** 25,648 in 406 files to about 31,000 (28,500-34,500), +21% (+11% to +35%), Part B's ports,
    defaults and plug-ins the largest share.
18. **Test lines:** 109,966 in 491 files to about 122,000 (116,500-130,000), +11% (+6% to +18%), the trace driver's
    about 940 among them.
