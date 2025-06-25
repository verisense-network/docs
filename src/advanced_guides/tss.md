## Threshold Signature Scheme (TSS)

### What is Threshold Signature Scheme?

In traditional signature schemes, the private signing key is usually held by a single entity. Once this private key is compromised, the entire system's security is broken. In Threshold Signature Scheme (TSS), the private key is split into multiple shares and distributed among multiple participants. No single participant can reconstruct the full private key alone.

TSS allows us to set a threshold. For example:

* In a 2-of-3 TSS, three participants hold key shares. Any two participants can jointly generate a valid signature, but no single participant can sign alone.

This greatly improves security and eliminates single points of failure.

### TSS Use Cases in Verisense

On the Verisense platform, TSS is mainly used for:

* Cross-chain bridge custody: ensuring secure multi-party control over cross-chain assets.
* Vault locking: for example, in multi-party governance or multi-signature wallets, ensuring that sensitive operations require multiple parties to jointly authorize.
* Monadring Consensus Algorithm.

---

### Code Examples

The Verisense SDK provides out-of-the-box support for TSS. Below are examples of how to obtain TSS public keys and initiate signing:

#### Retrieve TSS Public Key

Obtaining a public key is usually for generating transfer addresses or on-chain address binding:

```rust
use hex;
use vrs_core_sdk::{get, tss::tss_get_public_key, tss::CryptoType};

#[get]
pub fn get_public_key(crypto_type: u8, tweak: String) -> Result<String, String> {
    let result =
        tss_get_public_key(CryptoType::try_from(crypto_type)?, tweak).map_err(|e| e.to_string())?;
    Ok(hex::encode(result))
}
```

* `crypto_type` specifies the signature algorithm type.
* `tweak` is used to derive a child public key based on the master key share. Different tweaks generate different deterministic child keys under the same master key share.

#### Generate TSS Signature

In actual transfer or authorization scenarios, the system can initiate a TSS signing request:

```rust
use hex;
use vrs_core_sdk::{get, tss::tss_sign, tss::CryptoType};

#[get]
pub fn sign(crypto_type: u8, tweak: String, message: String) -> Result<String, String> {
    let result =
        tss_sign(CryptoType::try_from(crypto_type)?, tweak, message).map_err(|e| e.to_string())?;
    Ok(hex::encode(result))
}
```

* `message` is the content to be signed (usually a hash value).
* The return value is the signature result encoded in hex.

#### Supported Signature Algorithms

Verisense currently supports the following signature algorithms:

```rust
#[repr(u8)]
#[derive(Encode, Decode)]
#[export]
pub enum CryptoType {
    P256 = 0,
    Ed25519 = 1,
    Secp256k1 = 2,
    Secp256k1Tr = 3,
    Ed448 = 4,
    Ristretto255 = 5,
    EcdsaSecp256k1 = 6,
}
```

*  `EcdsaSecp256k1` is used for ETH/BSC.
* The rest without the Ecdsa prefix (`Ed25519`, `Ristretto255`, `Secp256k1`, `Secp256k1Tr`, `P256`, `Ed448`) use Schnorr signatures.

Schnorr signature schemes provide better aggregation properties and security in multi-party signature (MPC/TSS) scenarios, making them ideal for high-performance and decentralized use cases.

---

### Example: 2-of-3 Signing Process

Suppose we have Alice, Bob, and Charlie forming a 2-of-3 TSS group:

1. During system initialization, each of the three participants receives a private key share.
2. When a signature is needed, Alice and Bob collaborate through secure channels to generate a valid signature.
3. Even if Charlie is offline, Alice and Bob can still successfully generate the signature.
4. No single participant (e.g., only Alice) can generate the signature alone.

This mechanism ensures both the security and flexibility of asset control, making it very suitable for decentralized custody, cross-chain bridges, DAO governance, and other multi-party control scenarios.
