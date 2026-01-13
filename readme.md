## THIS APP IS A VISUAL LAB TO UNDERSTAND THE WORKING OF HEIRARCHICAL DETERMINISTIC WALLET WORKS.

---

## What Is a Mnemonic Phrase?

A mnemonic phrase (usually 12 or 24 words) is a human-readable representation of cryptographic randomness.
It follows the BIP-39 standard.

Example:

gravity machine north sort system school ...

This phrase itself is NOT a private key.
It is simply a readable way to store entropy that can later regenerate all keys.

---

## Mnemonic → Seed

The mnemonic phrase is converted into a 512-bit seed using PBKDF2 hashing.

seed = PBKDF2(mnemonic, "mnemonic" + optionalPassphrase)

This seed becomes the root of the entire wallet system.

---

## Seed → HD Wallet (BIP-32)

Using the seed, BIP-32 creates:
- A master private key
- A master chain code

From this root, the wallet can deterministically derive infinite child keys.

## Example:
HD Wallet Derivation Tree (Ethereum)

| Path | Wallet Index |
|----|-------------|
| `m` | — |
| `m/44'/60'/0'/0/0` | Wallet #1 |
| `m/44'/60'/0'/0/1` | Wallet #2 |
| `m/44'/60'/0'/0/2` | Wallet #3 |

Each increment of `i` deterministically generates a **new wallet**  
from the **same seed phrase**, without storing any extra data.

## What Does the Path m/44'/60'/0'/0/i Mean?

This is the standard Ethereum derivation path used in this app.

Path breakdown:

m    → Master root derived from seed  
44'  → BIP-44 standard (multi-account hierarchy)  
60'  → Coin type (Ethereum)  
0'   → Account index  
0    → External chain (public addresses)  
i    → Address index (0, 1, 2, ...)

Each time you click "Add Wallet", the value of i increments automatically,
deriving a new wallet from the same seed.

---

## Different Blockchains Use Different Derivation Paths

| Blockchain | Standard / Curve | Derivation Path | Notes |
|----------|------------------|-----------------|------|
| **Ethereum** (used in this app) | BIP44 / secp256k1 | `m/44'/60'/0'/0/i` | Used by MetaMask, Ledger, Polygon, BSC, Avalanche C-Chain |
| **Bitcoin (Legacy)** | BIP44 | `m/44'/0'/0'/0/i` | Old P2PKH addresses |
| **Solana** | SLIP-0010 / ed25519 | `m/44'/501'/0'/0'` | Not compatible with secp256k1 |
| **Cardano** | CIP-1852 | `m/1852'/1815'/0'/0/0` | Uses extended account model |

---

## Private Key vs Public Key

Private Key:
- 32 bytes
- Must remain secret
- Used to sign transactions

Public Key:
- 64 bytes
- Derived from the private key
- Used to verify signatures

---

## Why Public Keys Look Longer

Private Key: 32 bytes  
Public Key: 64 bytes  
Address: 20 bytes  

Public keys store elliptic curve coordinate data,
while addresses are hashed and shortened for usability.

---
