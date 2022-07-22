<!-- .slide: data-background-color="#8D3AED" -->

## Cryptography

---

### Asymetric crytograpgy

Elliptic Curve Cryptography protocol is used in Cosmos SDK.

They provide 3 digital key schemes for creating digital signatures:

- secp256k1: for accounts
- secp256r1: for accounts on apple/android devices.
  (for supporting signing algorithm on secure enclave in macOS/iOS/watchOS and Android Hardware-backed Keystore)
- Ed25519: Only used for Consensus validation on Tendermint side

---

### Signing

- List all kind a transactions which can be signed on Cosmos

---

### Cryptographic hash function

The cryptographic hash function used in Cosmos is _SHA-256_.

It is used everywhere a hash function is needed:

- compute addresses from public keys.
- Merkle tree hashing function.

RIPEMD160 cryptographic hash function has been fully removed from all componenents of Cosmos/Tendermint.

---

### Symetric encryption

_XSalsa20_ Symmetric Cipher is used to store on disk encrypted private keys.
https://github.com/cosmos/cosmos-sdk/blob/main/docs/basics/accounts.md#keyring

---

### Exotic primitives

There are initiatives to support ([Zero Knowledge Validator](https://medium.com/zero-knowledge-validator/cosmos-privacy-zkp-showcase-recap-8ad8def58573)) and use ([Penumbra Blockchain](https://penumbra.zone)) ZKP in cosmos ecosystem for privacy.

Mostly they reach a high barrier as tools for ZK and privacy are missing, mainly due to the fact that the CosmosSDK is written in Go, and many of the zk related libraries are written in Rust and the zk community is primarily focused on building with that language.

https://medium.com/zero-knowledge-validator/cosmos-privacy-zkp-showcase-recap-8ad8def58573

---

### Cryptography librairies used

Cosmos SDK and Tendermint are written in Go.
Go librairies are used for cryptography

- secp256k1, as implemented in the [Cosmos SDK's crypto/keys/secp256k1 package](https://github.com/cosmos/cosmos-sdk/blob/v0.46.0-rc1/crypto/keys/secp256k1/secp256k1.go)
- secp256r1, as implemented in the [Cosmos SDK's crypto/keys/secp256r1 package](https://github.com/cosmos/cosmos-sdk/blob/v0.46.0-rc1/crypto/keys/secp256r1/pubkey.go)
- tm-ed25519, as implemented in the [Cosmos SDK crypto/keys/ed25519 package](https://github.com/cosmos/cosmos-sdk/blob/v0.46.0-rc1/crypto/keys/ed25519/ed25519.go) (opens new window). This scheme is supported only for the consensus validation.
- SHA256 modules for core crypto modules in Go lang library

---

### Accounts

At the core of every Cosmos account, there is a seed, which takes the form of a 12 or 24-words mnemonic. From this mnemonic, it is possible to create any number of Cosmos accounts, i.e. pairs of private key/public key

---

### Accounts

#### Hierarchical Deterministic Key Derivation

Cosmos use `hard derivation` for deriving multiple cryptographic keypairs from a single secret following [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki), [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki), [BIP43](https://github.com/bitcoin/bips/blob/master/bip-0043.mediawiki) and [BIP44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) specifications and principally the [confio specifications](https://github.com/confio/cosmos-hd-key-derivation-spec) for HD key derivation standarization on the Cosmos ecosystem.

---

### Accounts

#### Hierarchical Deterministic Key Derivation

```text
     Account 0                         Account 1                         Account 2
+------------------+              +------------------+               +------------------+
|    Address 0     |              |    Address 1     |               |    Address 2     |
|        ^         |              |        ^         |               |        ^         |
|        |         |              |        |         |               |        |         |
|  Public key 0    |              |  Public key 1    |               |  Public key 2    |
|        ^         |              |        ^         |               |        ^         |
|        |         |              |        |         |               |        |         |
|  Private key 0   |              |  Private key 1   |               |  Private key 2   |
+------------------+              +------------------+               +------------------+
         |                                 |                                  |
         +--------------------------------------------------------------------+
                                           |
                                 +---------+---------+
                                 |  Mnemonic (Seed)  |
                                 +-------------------+

```

---

### Accounts

#### Addresses in Cosmos

---

### Data structures

- merkle tree
- IAVL+ Tree

---

### Data structures

#### Merkle tree

Tendermint uses RFC 6962 specification of a merkle tree, with sha256 as the hash function.
Merkle trees are used throughout Tendermint to compute a cryptographic digest of a data structure.

2 Particularities for RFC 6962 compliant merkle trees:

- leaf nodes and inner nodes have different hashes. This is for "second pre-image resistance", to prevent the proof to an inner node being valid as the proof of a leaf. The leaf nodes are SHA256(0x00 || leaf_data), and inner nodes are SHA256(0x01 || left_hash || right_hash).

- When the number of items isn't a power of two, the left half of the tree is as big as it could be. (The largest power of two less than the number of items) This allows new leaves to be added with less recomputation.

---

### Data structures

#### Merkle tree with 6 items

```text
            root
             / \
           /     \
         /         \
       /             \
    h0123            h45
     / \             / \
    /   \           /   \
   /     \         /     \
  h01    h23      h4     h5
 / \     / \      |      |
h0  h1  h2 h3     |      |
|   |   |  |      |      |
d0  d1  d2 d3     d4     d5

h0 = SHA256(0x00 || d0); h3 = SHA256(0x00 || d3);
h1 = SHA256(0x00 || d1); h4 = SHA256(0x00 || d4);
h2 = SHA256(0x00 || d2); h5 = SHA256(0x00 || d5);
h01 = SHA256(0x01 || h0 || h1);
h23 = SHA256(0x01 || h2 || h3);
h45 = SHA256(0x01 || h4 || h5);
h0123 = SHA256(0x01 || h01 || h23);
root = SHA256(0x01 || h0123 || h45);
```

---

### Data structures

#### Merkle tree with 7 items

Adding a 7th element to the merkle tree:

```text
              *
             / \
           /     \
         /         \
       /             \
      *               *
     / \             / \
    /   \           /   \
   /     \         /     \
  *       *       *       h6
 / \     / \     / \
h0  h1  h2  h3  h4  h5
```

---

### Data structures

#### IAVL+ Tree

The purpose of this data structure is to provide persistent storage for key-value pairs, for example store account balances.

In Ethereum, the analog is Patricia tries.

Pro:

- Keys do not need to be hashed prior to insertion in IAVL+ trees, so this provides faster iteration in the key space which may benefit some applications.
- The logic is simpler to implement, requiring only two types of nodes -- inner nodes and leaf nodes

Cons:

- while IAVL+ trees provide a deterministic merkle root hash, it depends on the order of transactions.

---

### Improvement we would like to see

- change spec256K1 cure to a better one (nsa backdoor)
- better tree data structure like sparse merkle tree
