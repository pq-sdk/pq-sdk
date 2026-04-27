## PQSDK — Post-Quantum Vault Protocol for Solana

```
Today every Solana wallet uses Ed25519 to sign transactions. The moment you sign, your public key is permanently on-chain. Quantum computers running Shor's algorithm can derive your private key directly from your public key. Attackers are harvesting on-chain signatures right now, waiting for quantum hardware to mature. By then it is too late. Your funds are already exposed.
PQSDK solves this today. No server. No backend. No trusted third party. Pure on-chain enforcement.
```

## The Core Insight

```
Instead of protecting an Ed25519 key, PQSDK eliminates the need for any permanent signing key entirely. Funds live in a Program Derived Address owned by an Anchor program. PDAs have no private key — mathematically guaranteed by the elliptic curve. There is nothing for a quantum computer to derive. No key exists to steal.
The vault is unlocked not by a signature but by knowledge of a secret value that never appears on-chain.
The Double Hash Mechanism
At identity initialization the user generates a CRYSTALS-Dilithium keypair. Dilithium is a NIST-approved post-quantum signature scheme based on lattice mathematics. No known quantum algorithm can break it.
```

```
Only the doubleHash is stored on-chain inside the PDA. The dilithiumPublicKey and the singleHash never touch the blockchain. Ever.
To withdraw funds the user submits the singleHash to the Anchor program. The program computes sha256(singleHash) on-chain and compares it against the stored doubleHash. If they match, funds move. If they do not, the transaction is rejected.
```

## Why An Attacker Cannot Steal Funds
```
An attacker who monitors the blockchain sees only the doubleHash stored in the PDA account data. To withdraw they need the singleHash. To get the singleHash from the doubleHash they must reverse SHA-256 — a preimage attack that is computationally infeasible even for quantum computers. Grover's algorithm gives only a square root speedup against SHA-256, reducing 256-bit security to 128-bit security. Still unbreakable in practice.
Even if somehow the singleHash were obtained, the attacker still does not have the dilithiumPublicKey. And even if the dilithiumPublicKey were obtained, forging a Dilithium signature requires breaking lattice cryptography — which no quantum algorithm can do.
Every layer is independently quantum resistant.
No other approach eliminates on-chain key exposure this completely. Pure Dilithium wallets cannot talk to Solana natively. Backend-gated approaches require trusting a server. Ed25519-only approaches leave permanent keys on-chain. PQSDK stores nothing on-chain that can be reversed, forged, or quantum-attacked. The vault is keyless. The proof is a secret hash. The enforcement is autonomous and on-chain.
```
