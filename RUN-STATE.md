# IBFT unattended overhaul experiment -- run state

Where the run is, for the agent to resume from. Read after every compaction and at the start of every step;
updated before every commit; the guard writes the last-commit line. Overwritten as the run goes, committed and
pushed with the record at every commit. Before the run, this is its template.

## In progress

- **Part, step, slice:** --
- **Last commit:** -- (workbook row --)
- **Started (UTC):** --

## Next in line

The slices after this one, in order, each with the criterion that put it there (`SETUP.md`, *Choosing the next
slice*).

| # | slice | kind | `Chosen-By` |
|---|---|---|---|

## Parked

| # | slice | parked at | refused by | `QUESTIONS.md` entry |
|---|---|---|---|---|

## Counts

- **Questions:** -- answered, -- open.
- **Interventions:** --.
- **Waits:** --, -- hours in all.

## Easily forgotten

- every commit: `Kind`, `Rule` for a `rule-offence`, `Chosen-By`, `Over`, `Planned`, `Trigger` when unplanned --
  the guard writes the counts, the threshold and `Stages`;
- one kind a commit; tests that come with a change are that change's;
- stages by what the staged files reach; all three every tenth commit and at a part's end;
- a red without a cause: one rerun, logged `flake`; never a second;
- five refused attempts or an hour: park the slice;
- no test removed, no assertion relaxed, no `#[ignore]`; a question that needs one is left open;
- one agent, nothing in parallel.
