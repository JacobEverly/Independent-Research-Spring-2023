# Independent-Research-Spring-2023 (Work in progress)


## Project Abstract

![Screen Shot 2023-03-08 at 5 44 09 PM](https://user-images.githubusercontent.com/112036223/223868377-f5d89b26-6b18-43dd-9823-618377dfe689.png)

## Link to project board

https://www.notion.so/Blockchain-Independent-Research-38c4abe91db94eb8b41f2c6361302530

## TODO
1. Posedidon
   - [x] Code
   - [ ] Test
2. Baby JubJub
   - [x] Code
   - [x] Test
3. Signature Scheme
   - [ ] Code
   - [ ] Test
3. Merkle Tree
   - [x] Code
   - [ ] Test
3. Weight Generation
   - [ ] Code
4. Validtor Selection
   - [ ] Code
5. Proof System
   - [ ] Code
   - [ ] Test

## Create enviroment
```cmd
conda create -n {env} python=3.9
conda activate {env}
```
## Create and switch branch
```cmd
# Create branch
git branch {branch name}

# Switch branch
git checkout {branch name}

# Push to branch
git push -u origin {branch name} # for the first time
git push # for afterwards

# synchronize(rebase) with main branch
git pull origin main

# Delete branch
git branch -d {branch name}
git checkout main
```

Then do pull request for merging to main branch


## Git push
```cmd
git add {changed files}
git commit -m "{description}"
git push
```

## Install Python dependencies
```cmd
git pull
pip install -r requirement.txt
pip install -Ue .
```

## Import modules
```python
# python main.py -n ${number of nodes}
from py_cc.hashes import Poseidon, prime_254
from py_cc.elliptic_curves.baby_jubjub import GeneratePoint

import argparse
from random import randint, randbytes
import random
import hashlib

class PoseidonHash(Poseidon):
    def __init__(self, prime=prime_254, security_level=128, input_rate=3, t=3, alpha=5):
        """
        # TODO - add parameters description
        """
        super().__init__(prime, security_level, alpha, input_rate, t)
    
    def __str__(self) -> str:
        return "PoseidonHash"

class KeyPairGen:
    def __init__(self):
        self.hash = PoseidonHash()
        self.ecc = GeneratePoint

        self.t = 3 # TODO: try to remove this
    
    def getKeyPair(self):
        input_vec = [randint(0, prime_254 - 1) for _ in range(0, self.t)]
        poseidon_output = int(self.hash.run_hash(input_vec))
        print("Hash(int): ", poseidon_output)

        print("Generate key pair")
        secret_key = poseidon_output
        public_key = secret_key * self.ecc
        return secret_key, public_key
    
    def getKeyPairs(self, num):
        pairs = {}
        for _ in range(num):
            while True:
                sk, pk = self.getKeyPair()
                if sk not in pairs:
                    pairs[sk] = pk
                    break
        return pairs
                
    def __repr__(self):
        return f"Hash: {self.hash}, ECC: {self.ecc}"

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description='Generate Key Pairs')
    parser.add_argument('-n', '--num', type=int, default=1, help='Number of key pairs to generate')
    args = parser.parse_args()
    keyGen = KeyPairGen()
    pairs = keyGen.getKeyPairs(args.num)
    print(pairs)
```

