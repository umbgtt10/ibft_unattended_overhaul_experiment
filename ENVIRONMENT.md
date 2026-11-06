# IBFT unattended overhaul experiment -- the environment

The machine, the toolchain and the agent (`SETUP.md`, *The environment, recorded at the start*), read at the start of
the run and again at its end; a difference between the two is reported in `RESULT-SUMMARY.md`.

## At the start -- 2026-11-06T16:02Z

**The machine.**

| | |
|---|---|
| processor | AMD Ryzen 7 7735HS with Radeon Graphics -- 8 cores, 16 threads (`lscpu`) |
| memory | 91,405,904 kB, 87 GiB usable (`/proc/meminfo`, `free -g`); 7 GiB swap |
| disk | `/dev/nvme0n1p2`, 937 G, 580 G free (`df -h` on the scratch directory) |
| system | Ubuntu 26.04.1 LTS (Resolute Raccoon) (`/etc/os-release`) |
| kernel | Linux 7.0.0-38-generic #38-Ubuntu SMP PREEMPT_DYNAMIC Fri Sep 4 09:10:14 UTC 2026 x86_64 (`uname -a`) |
| clock source | `hpet` (`/sys/devices/system/clocksource/clocksource0/current_clocksource`) -- the TSC still marked unstable, as at the base's measurements |

**The toolchain.**

| tool | version | read from |
|---|---|---|
| rustc | 1.99.1 (58cf8f9c3 2026-10-13) | `rustc -V`; pinned by `rust-toolchain.toml` with `rustfmt`, `clippy`, `llvm-tools` |
| just | 1.58.0 | `just --version` |
| cargo-llvm-cov | 0.9.1 | `~/.cargo/.crates2.json` |
| cargo-shear | 1.14.0 | `~/.cargo/.crates2.json` |
| cargo-stern4rust | 0.20.1 | `~/.cargo/.crates2.json` |
| cargo-dry4rust | 0.5.0 | `~/.cargo/.crates2.json` |
| cargo-crap4rust | 0.9.0 | `~/.cargo/.crates2.json` |
| cargo-twin4rust | 0.7.0 | `~/.cargo/.crates2.json` |
| cargo-iceberg4rust | 0.4.0 | `~/.cargo/.crates2.json` |
| slotgate | 0.7.0 | `~/.cargo/.crates2.json` |
| cargo-grip4rust | 0.10.0 | `~/.cargo/.crates2.json`; its JSON reports `"version": "0.10.0"` |
| cargo-braintax4rust | 0.14.0 | `~/.cargo/.crates2.json`; its JSON reports `"version": "0.14.0"` |

Every one the release `SETUP.md` names; no development build.

**The agent.**

| | |
|---|---|
| model | `claude-opus-5-5`, Opus 5.5 |
| harness | Claude Code 2.1.314 (the session transcript's `version` field, on every line so far) |
| permission mode | `dontAsk`, with the settings in `SETTINGS.json` |
| starting prompt | `PROMPT.md`, word for word |
| instructions in effect | `etheram-ibft`'s `CLAUDE.md`, SHA-256 `4288deff79e59f27fbb1372567dbc309176373c76fe0f85ee60d804b08ff100b` -- the figure `SETUP.md` records |
| memory directory | empty |

**The base, scored.** Grip and Braintax over the seven shipped crates of a `git archive` export of `8a775a2`, with
the releases above -- the figures every later row is compared with:

| crate | Grip | Braintax |
|---|---:|---:|
| `node` | 297.59 | 2,295.50 |
| `node-infra` | 150.75 | 591.42 |
| `recovery-control` | 62.73 | 367.73 |
| `recovery-ibft` | 49.72 | 247.32 |
| `types` | 35.49 | 440.00 |
| `evm` | 35.24 | 503.97 |
| `execution-types` | 11.07 | 64.14 |
| **total** | **642.59** | **4,510.09** -- ratio **0.1425** |

Offences on the same export, under stern4rust 0.20.1: 483 as configured, 696 with everything on; `cargo-shear`
13. Stage 1 at the base: 3,042 tests passed, none ignored; stage 5, 398 (the 3,440 `SETUP.md` gives).

## At the end

-- to be read at the end of the run.
