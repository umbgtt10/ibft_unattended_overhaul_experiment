# IBFT unattended overhaul experiment -- the coverage baseline per module

Part 0's coverage baseline: line and function coverage, module by module, of the base, `8a775a2`, taken on a
`git archive` export of it with the tests stage 1 runs on the protocol's packages:

    cargo llvm-cov --manifest-path <export>/Cargo.toml -p execution-types -p evm -p types -p recovery-control \
        -p recovery-ibft -p node -p node-infra -p validation --json --summary-only --output-path <summary.json>
    cargo xtask coverage-by-module <summary.json> <export>

cargo-llvm-cov 0.9.1, its 16 test binaries passing. A module is the directory a file sits in. `validation` is a
crate that exists to test; it is listed but is not a shipped crate. Part C takes the same measure at the end, and
function by function at both ends.

| module | lines | % | functions |
|---|---:|---:|---:|
| `evm/src` | 72/72 | 100.00 | 4/4 |
| `evm/src/tiny_evm` | 875/1012 | 86.46 | 97/106 |
| `execution-types/src/common_types` | 155/155 | 100.00 | 19/19 |
| `execution-types/src/execution` | 26/26 | 100.00 | 3/3 |
| `node-infra/src/builders` | 517/543 | 95.21 | 81/83 |
| `node-infra/src/builders/etheram_node_builder_components` | 192/192 | 100.00 | 22/22 |
| `node-infra/src/implementations` | 604/655 | 92.21 | 140/159 |
| `node/src` | 99/99 | 100.00 | 9/9 |
| `node/src/ibft` | 120/120 | 100.00 | 19/19 |
| `node/src/ibft/assembly` | 161/161 | 100.00 | 15/15 |
| `node/src/ibft/assembly/consensus` | 49/52 | 94.23 | 11/12 |
| `node/src/ibft/buffers/bounded_buffer` | 60/61 | 98.36 | 12/12 |
| `node/src/ibft/buffers/wal_buffer` | 53/54 | 98.15 | 8/8 |
| `node/src/ibft/crypto` | 85/88 | 96.59 | 16/17 |
| `node/src/ibft/ibft_protocol` | 3615/3676 | 98.34 | 218/218 |
| `node/src/ibft/ibft_protocol/bootstrapping` | 457/458 | 99.78 | 58/58 |
| `node/src/ibft/ibft_protocol/recovery_host` | 587/636 | 92.30 | 31/31 |
| `node/src/io/incoming` | 56/56 | 100.00 | 5/5 |
| `node/src/io/outgoing` | 132/139 | 94.96 | 15/15 |
| `node/src/io/partitioner` | 29/32 | 90.62 | 3/4 |
| `node/src/observer` | 27/49 | 55.10 | 3/3 |
| `node/src/observer/consensus` | 36/36 | 100.00 | 10/10 |
| `node/src/runtime/context` | 568/568 | 100.00 | 40/40 |
| `node/src/runtime/execution` | 12/12 | 100.00 | 1/1 |
| `node/src/runtime/state` | 722/739 | 97.70 | 119/120 |
| `node/src/runtime/state/cache` | 6/7 | 85.71 | 2/3 |
| `node/src/runtime/state/storage` | 101/107 | 94.39 | 15/17 |
| `recovery/control/src` | 619/634 | 97.63 | 102/107 |
| `recovery/control/src/input` | 4/4 | 100.00 | 1/1 |
| `recovery/control/src/output` | 121/121 | 100.00 | 27/27 |
| `recovery/control/src/process` | 465/465 | 100.00 | 28/28 |
| `recovery/control/src/recording` | 14/14 | 100.00 | 4/4 |
| `recovery/ibft/src` | 371/371 | 100.00 | 39/39 |
| `recovery/ibft/src/input` | 27/27 | 100.00 | 5/5 |
| `recovery/ibft/src/output` | 153/156 | 98.08 | 29/30 |
| `recovery/ibft/src/process` | 174/174 | 100.00 | 10/10 |
| `recovery/ibft/src/snapshot` | 115/119 | 96.64 | 10/10 |
| `types/src` | 62/62 | 100.00 | 16/16 |
| `types/src/message` | 3/3 | 100.00 | 1/1 |
| `types/src/state` | 747/757 | 98.68 | 100/100 |
| `validation/src` | 1503/1641 | 91.59 | 201/268 |

| crate | lines | % | functions |
|---|---:|---:|---:|
| `evm` | 947/1084 | 87.36 | 101/110 |
| `execution-types` | 181/181 | 100.00 | 22/22 |
| `node` | 6975/7150 | 97.55 | 610/617 |
| `node-infra` | 1313/1390 | 94.46 | 243/264 |
| `recovery-control` | 1223/1238 | 98.79 | 162/167 |
| `recovery-ibft` | 840/847 | 99.17 | 93/94 |
| `types` | 812/822 | 98.78 | 117/117 |
| **the seven shipped crates** | **12291/12712** | **96.69** | **1348/1391** |
| `validation` | 1503/1641 | 91.59 | 201/268 |
| all | 13794/14353 | 96.11 | 1549/1659 |
