commands to use:

1. Install rust and verify
   rustc --version

2. Install and verify circom
   circom --version

3. install snark.js
   npm i -g snarkjs

4. compile circuit
   circom circuits/sum_product.circom --r1cs --wasm --sym -o outputs

5. Start power of Tau ceremony
   snarkjs powersoftau new bn128 14 pot14_0000.ptau -v

6. First contribution
   snarkjs powersoftau contribute pot14_0000.ptau pot14_0001.ptau --name="First contribution" -v

7. Verify so far
   snarkjs powersoftau verify pot14_0001.ptau

8. Apply random beacon
   snarkjs powersoftau beacon pot14_0001.ptau pot14_beacon.ptau 0102030405060708090a0b0c0d0e0f101112131415161718191a1b1c1d1e1f 10 -n="Final Beacon"

9. Prepare phase 2
   snarkjs powersoftau prepare phase2 pot14_beacon.ptau pot14_final.ptau -v

10. Verify so far
    snarkjs powersoftau verify pot14_final.ptau

11.
