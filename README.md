# Overview

This repository contains verification snapshots for **phishing contracts** that were observed performing **transaction simulation phishing attacks**.  
Each phishing contract has its own folder under `Ethereum_unlabelled_phishing_contracts/`, named by the contract address. Inside each contract folder, there is a `tx_history` file which documents our testing result of phishing contracts.

---

## Directory layout
```
Ethereum_unlabelled_phishing_contracts/
├─ 0xAA...aa/               # folder named by phishing contract address
│  └─ tx_history            # trace file (plain text)
├─ 0xBB...bb/
│  └─ tx_history
└─ ...
```

---
## `tx_history` file format

Each `tx_history` file contains two example hashes (referred as `Testing transaction 1` and `Testing transaction 2` in this dataset).  
Under each hash, there is a sequence of smart-contract-level state messages (SM) observed in our verification result.

### Example

```
Testing transaction 1: the transaction sender will receive back more than the deposited amount.

SM Address: 0x00000732774d2109e1203f7b583d9551cad30000, caller:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266,target:0x00000732774d2109e1203f7b583d9551cad30000 is_static:false, transfer:Transfer(600000000000000), input_size:0

SM Address: 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266, caller:0x00000732774d2109e1203f7b583d9551cad30000,target:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 is_static:false, transfer:Transfer(600000000000000), input_size:0

SM Address: 0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266, caller:0x00000732774d2109e1203f7b583d9551cad30000,target:0xf39fd6e51aad88f6f4ce6ab8827279cfffb92266 is_static:false, transfer:Transfer(10000), input_size:0


Testing transaction 2: the transaction sender's deposits are moved to another address undet the attackers' control.

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
## Meanings of `Testing transaction 1` and `Testing transaction 2`

- **`Testing transaction 1`** — Represents the result of a testing transaction execution.
  In this testing transaction, the contract returns the user’s transferred funds and adds `10000 wei` extra back to the user.

- **`Testing transaction 2`** — Represents the result of another testing transaction execution .  
  In this case, the user’s funds are sent to another address controlled by the attacker.

For the same phishing contract, changing certain fields (statements:"GAS","CALLER") results in **two different outcomes**:
- The **simulation** (wallet preview) shows a safe refund (`Testing transaction 1`);
- The **actual execution** sends funds to the attacker (`Testing transaction 2`).

This demonstrates a **simulation-transaction attack**.

---
## Usage

To inspect:
1. Open any contract folder named by address under `Ethereum_unlabelled_phishing_contracts/`.
2. View the `tx_history` file inside.
3. Compare `Testing transaction 1` and `Testing transaction 2` traces to understand behavioral differences.

## Additional Verification Notice:
We have manually verified all phishing contract addresses listed in this repository. 
Our verification involved analyzing:
1. The sources and destinations of funds associated with each contract;
2. The historical activities of the deployer addresses;
3. Behavioral patterns such as mass airdrops, malicious transfers, or repeated phishing-linked transactions.
These verifications are to ensure all identified contracts are indeed phishing-related.

