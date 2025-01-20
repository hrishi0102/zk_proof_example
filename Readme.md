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

11. Export r1cs to json
    snarkjs r1cs export json sum_product.r1cs sum_product.r1cs.json

12. Setup groth 16
    snarkjs groth16 setup circuit.r1cs pot14_final.ptau circuit_0000.zkey

13. Contribute to the phase 2 ceremony
    snarkjs zkey contribute circuit_0000.zkey circuit_0001.zkey --name="1st Contributor Name" -v

14. Verify zkey
    snarkjs zkey verify sum_product.r1cs pot14_final.ptau circuit_0001.zkey

15. Apply random beacon
    snarkjs zkey beacon circuit_0001.zkey circuit_final.zkey 0102030405060708090a0b0c0d0e0f101112131415161718191a1b1c1d1e1f 10 -n="Final Beacon phase2"

16. Verify final zkey
    snarkjs zkey verify sum_product.r1cs pot14_final.ptau circuit_final.zkey

17. Export veriication key
    snarkjs zkey export verificationkey circuit_final.zkey verification_key.json

18. Create input json file and generate witness
    node outputs/sum_product_js/generate_witness.js outputs/sum_product_js/sum_product.wasm inputs/input.json outputs/witness.wtns

19. Create Proof
    snarkjs groth16 prove outputs/circuit_final.zkey outputs/witness.wtns outputs/proof.json outputs/public.json
