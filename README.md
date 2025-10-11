# Overview

This repository contains transaction-history snapshots for **phishing contracts** that were observed performing **simulation-transaction attacks**.  
Each phishing contract has its own folder under `MAINNET_unflagged_GAS/`, named exactly by the contract address. Inside each contract folder there is a `tx_history` file which documents representative transaction traces (two example traces per file).

---

## Directory layout
```
MAINNET_unflagged_GAS/
├─ 0xAA...aa/               # folder named by phishing contract address
│  └─ tx_history            # trace file (plain text)
├─ 0xBB...bb/
│  └─ tx_history
└─ ...
```

---
## `tx_history` file format

Each `tx_history` file contains two example hashes (called `hash1` and `hash2` in this dataset).  
Each hash section lists the sequence of smart-contract-level state messages (SM) observed in the trace.

### Example

```
hash1

SM Address: 0x00000732774d2109e1203f7b583d9551cad30000, caller:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266,target:0x00000732774d2109e1203f7b583d9551cad30000 is_static:false, transfer:Transfer(600000000000000), input_size:0

SM Address: 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266, caller:0x00000732774d2109e1203f7b583d9551cad30000,target:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 is_static:false, transfer:Transfer(600000000000000), input_size:0

SM Address: 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266, caller:0x00000732774d2109e1203f7b583d9551cad30000,target:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 is_static:false, transfer:Transfer(10000), input_size:0

hash2

SM Address: 0x00000732774d2109e1203f7b583d9551cad30000, caller:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266,target:0x00000732774d2109e1203f7b583d9551cad30000 is_static:false, transfer:Transfer(600000000000000), input_size:0

SM Address: 0x0000e23abdc862a1911d77904e77fdb378d00000, caller:0x00000732774d2109e1203f7b583d9551cad30000,target:0x0000e23abdc862a1911d77904e77fdb378d00000 is_static:false, transfer:Transfer(0), input_size:4

SM Address: 0x000037bb05b2cef17c6469f4bcdb198826ce0000, caller:0x00000732774d2109e1203f7b583d9551cad30000,target:0x000037bb05b2cef17c6469f4bcdb198826ce0000 is_static:false, transfer:Transfer(600000000000000), input_size:0
```
---

## Field definitions

| Field | Description |
|-------|--------------|
| **SM Address** | Contract or account where the state-message executes. |
| **caller** | Address that invoked the call. |
| **target** | The callee address (same as SM Address). |
| **is_static** | Whether the call was made with `staticcall`. |
| **transfer:Transfer(x)** | Amount transferred in wei. |
| **input_size** | Size (bytes) of calldata for this call. |

---
## Meaning of `hash1` and `hash2`

- **`hash1`** — Represents the result of a transaction execution.
  In this trace, the contract returns the user’s transferred funds and even adds `10000 wei` extra back to the user.

- **`hash2`** — Represents the result of another transaction execution .  
  In this case, the user’s funds are sent to another address controlled by the attacker.

For the same phishing contract, changing certain fields ("GAS") results in **two different outcomes**:
- The **simulation** (wallet preview) shows a safe refund (`hash1`);
- The **actual execution** sends funds to the attacker (`hash2`).

This demonstrates a **simulation-transaction attack**.

---
## Usage

To inspect:
1. Open any contract folder named by address under `MAINNET_unflagged_GAS/`.
2. View the `tx_history` file inside.
3. Compare `hash1` and `hash2` traces to understand behavioral differences.
