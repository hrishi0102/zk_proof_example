# Generating ZK Proofs

## 1. Install Rust and Verify

```bash
rustc --version
```

## 2. Install and Verify Circom

```bash
circom --version
```

## 3. Install snark.js

```bash
npm i -g snarkjs
```

## 4. Compile Circuit

```bash
circom circuits/sum_product.circom --r1cs --wasm --sym -o outputs
```

## 5. Start Power of Tau Ceremony

```bash
snarkjs powersoftau new bn128 14 pot14_0000.ptau -v
```

## 6. First Contribution

```bash
snarkjs powersoftau contribute pot14_0000.ptau pot14_0001.ptau --name="First contribution" -v
```

## 7. Verify So Far

```bash
snarkjs powersoftau verify pot14_0001.ptau
```

## 8. Apply Random Beacon

```bash
snarkjs powersoftau beacon pot14_0001.ptau pot14_beacon.ptau 0102030405060708090a0b0c0d0e0f101112131415161718191a1b1c1d1e1f 10 -n="Final Beacon"
```

## 9. Prepare Phase 2

```bash
snarkjs powersoftau prepare phase2 pot14_beacon.ptau pot14_final.ptau -v
```

## 10. Verify So Far

```bash
snarkjs powersoftau verify pot14_final.ptau
```

## 11. Export R1CS to JSON

```bash
snarkjs r1cs export json sum_product.r1cs sum_product.r1cs.json
```

## 12. Setup Groth16

```bash
snarkjs groth16 setup circuit.r1cs pot14_final.ptau circuit_0000.zkey
```

## 13. Contribute to the Phase 2 Ceremony

```bash
snarkjs zkey contribute circuit_0000.zkey circuit_0001.zkey --name="1st Contributor Name" -v
```

## 14. Verify ZKey

```bash
snarkjs zkey verify sum_product.r1cs pot14_final.ptau circuit_0001.zkey
```

## 15. Apply Random Beacon

```bash
snarkjs zkey beacon circuit_0001.zkey circuit_final.zkey 0102030405060708090a0b0c0d0e0f101112131415161718191a1b1c1d1e1f 10 -n="Final Beacon Phase2"
```

## 16. Verify Final ZKey

```bash
snarkjs zkey verify sum_product.r1cs pot14_final.ptau circuit_final.zkey
```

## 17. Export Verification Key

```bash
snarkjs zkey export verificationkey circuit_final.zkey verification_key.json
```

## 18. Create Input JSON File and Generate Witness

```bash
node outputs/sum_product_js/generate_witness.js outputs/sum_product_js/sum_product.wasm inputs/input.json outputs/witness.wtns
```

## 19. Create Proof

```bash
snarkjs groth16 prove outputs/circuit_final.zkey outputs/witness.wtns outputs/proof.json outputs/public.json
```

## 20. Verify Proof

```bash
snarkjs groth16 verify outputs/verification_key.json outputs/public.json outputs/proof.json
```
