# clone-lin-bitcoin-core-eqtime

Experimental LIN clone of Bitcoin Core v27.1 **GetBlockProofEquivalentTime**.
This repository is the LIN copy.

Results and the machine-written proof harness live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open) (`examples/bitcoin_eqtime/`, `test/prove_bitcoin_eqtime_external.py`).

## Upstream

- Repo: [bitcoin/bitcoin](https://github.com/bitcoin/bitcoin)
- Tag: `v27.1` commit `1088a98f5aad080cc6cca2da174f206509fcda6c`
- License: **MIT** (Copyright (c) 2009-2022 The Bitcoin Core developers)
- `src/chain.cpp` sha256 `bbc1aa18b580d8a36ce7c416ee6cadee2c89686905912fc9a1edf6b3873992a5` blob `82007a8a1e7f38f7b02eb299f8fc30fc61fc190e`
- `src/test/pow_tests.cpp` sha256 `5911e49b195af6dac2e4e94f5509537555172ae9c6a4816dc06c147cd8c6d0ae` blob `3a44d1da499852cbdf6206bbf5026a50a9610f2f`

## Canonical vectors (Core GetBlockProofEquivalentTime_test)

Constant `nBits = 0x207fffff` → `GetBlockProof = 2`. Mainnet `nPowTargetSpacing = 600`.

| to_h | from_h | eqtime seconds |
|---|---|---|
| 1 | 0 | `600` |
| 0 | 1 | `-600` |
| 100 | 40 | `36000` |
| same | same | `0` |

Identity: `eqtime == (h1 - h2) * 600` when every block has the same nBits.

Class: **EXPERIMENTAL**. Not a Bitcoin node. Not uint256 nChainWork. Not CheckProofOfWork.

## Reproduce (C11 oracle, no Zig)

```
gcc -O2 -std=c11 -o bitcoin_eqtime_c11 test/oracles/bitcoin_eqtime_c11.c
./bitcoin_eqtime_c11 selftest
./bitcoin_eqtime_c11 eqtime 2 0 2 600
```
