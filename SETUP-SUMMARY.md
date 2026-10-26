# IBFT unattended overhaul experiment -- setup summary

Draft; frozen with `SETUP.md` at the run's start, after a dry run of about 25 commits recalibrates predictions 10,
13, 14 and 15. Each bullet is a measure, its value at the base of `etheram-ibft`'s branch, and the value predicted at
the end. `RESULT-SUMMARY.md` answers them in the same order.

**The bet**: the run completes Parts A and B with no intervention from the owner, no test lost, and every test green
at every commit. It starts at the time its `Freeze:` commit names, and this repository's `main` carries its record as
it goes.

1. **Commits:** about 180 on the branch (150-235), read as nearer 200 -- Part 0 about 8, the rules block about 75,
   the restructuring about 100; of them 10-30 unplanned. The further checks and the consumer are optional follow-ups
   after the experiment, outside it.
2. **Size:** at least 95% of the commits within 15 files and 300 lines; the planned exceptions are the two moves out
   of `execution-types` and any mechanical `--fix`.
3. **Behaviour:** no change outside `bug-fix` commits, as far as the tests see -- every test, the base's, the new
   ones and the changed ones, green at every commit on the stages its change reaches, and all of them at the end;
   every `bug-fix` with a test red on its parent.
4. **Bugs:** 4 recorded bugs fixed where their code moves, and 10-20 more found on the way.
5. **Tests:** about 3,500 at the base (the inventory sets the figure); none lost; 10-18% added; coverage not lower,
   overall or per crate.
6. **Offences:** 475 as configured (8 more baselined) and 696 with everything on, under stern4rust 0.18.0's 38 rules;
   13 `cargo-shear` findings -- all three to zero, the cluster harness's time reads excepted, held in the baseline.
7. **Grip/Braintax:** 0.1425 (642 / 4,503) to 0.150 (0.145-0.160), +5% (+2% to +12%).
8. **Seams:** `node` from 8 dependencies to `etheram-core` and `types` alone; `execution-types` gone; the observer
   trait's 78 methods split into five concerns.
9. **Autonomy:** the run never stops for the owner; every question it raises goes to `QUESTIONS.md`, answered on
   the fly or left open and delivered with the implementation; stops forced by the environment counted with their
   cause -- predicted none; no intervention, no message from the owner during the run.
10. **Time:** about 2 days of continuous wall-clock (1-5), about 45 hours, one agent working serially with no
    parallelism of any kind: about 12 minutes a commit -- 7 implementing, 3 validating (the three stages measured at
    2 minutes 40 seconds to 2 minutes 49 seconds after a change on the reference laptop, timed again on the run's
    laptop at the base), 2 for refused attempts, flakes, scoring and the record's push -- over about 180 commits,
    plus about 3 hours of fixed work. Active time: a usage limit's waits are reported apart.
11. **Raised and answered:** questions and open points the run raises and answers on the fly, or implements -- 25-50,
    each recorded in `QUESTIONS.md` with its answer and, where it caused a change, its commit.
12. **Raised and left open:** questions and open points the run raises and does not answer -- 10-25, delivered with
    the implementation as one of the experiment's results, each with the course its slice took meanwhile and the
    run's recommendation.
13. **Refused attempts and flakes:** at most one attempt in six refused (Raft: one in seven); 0-4 flaky reds after
    Part 0 fixes the base's restart-test race (one failure in 36 stage 5 runs on record); 0-3 slices parked.
14. **The agent's resource use:** about 3,300 model calls (2,400-5,200), 3.8 million output tokens (2.9-5.6
    million), 1.6 billion cached input tokens read (1.1-2.4 billion), 10-23 context compactions -- per commit 1.5-2.5
    times Raft's rules block, over about 180 commits, measured from the session transcripts.
15. **Peak disk:** at most 30 GB above the start's use, never below 10 GB free; the free disk at the start recorded
    by the measurement script (42 GB on the reference laptop).
16. **Files added**, base to end, a move counting as one removed and one added: about 355 (260-470) -- productive
    125, test 190, tooling 30, other 10; summed over the commits about 370. Raft's Phase 11 added 283.
17. **Files removed:** about 90 (50-200) -- productive 45, test 40, the rest tooling and other; summed about 105.
    Raft's: 79.
18. **Files changed:** about 650 (500-800) of the base's 1,088; summed over the commits about 1,700 (1,250-2,350).
    Raft's: 209, 97% of the files that stayed; 777 summed.
19. **Productive lines:** 25,613 in 406 files to about 31,000 (28,500-34,500), +21% (+11% to +35%), Part B's ports,
    defaults and plug-ins the largest share.
20. **Test lines:** 110,934 in 510 files to about 122,000 (117,000-130,000), +10% (+5% to +17%).
