# Fontys Blockchain
Blockchain Project 

## Block Chain Basics

- a blockchain consists of connected blocks
- every block contains specific information (usually transaction related) as well as a reference to the previous block (usually a hash)
- blocks can be validated and added to the chain through completing complex algorithms (proof of work)
- no central network for data; the entire blockchain is stored on every node (a group of computers usually), known as peer-to-peer network

### Hash Functions

Definition: *a mathematical algorithm that maps data of arbitrary size to a bit string of a fixed size and is a one-way function, that is, a function which is practically infeasible to invert*

- the **data** = the '**message**'
- **data** is mapped to a single bit string = the '**hash value**' / '**hash**' / '**message digest**'

Knowing the hash function, it is easy to verify that the input data maps to a given hash value, thus hash strings of blocks in a blockchain can be easily checked to see if they map to the correct data that that block should contain. The reverse however, is hard to do, hence why hash functions are perfect for usage in blockchains.

Encryption is simply a function that takes in a value and applies a secret key (a parameter) to return an encrypted value. Using that same secret key will decrypt the encrypted value back to the original value.

Hashing can not be reversed to reveal the original value, however, it always produces the same hash value if the same input is provided. The more complex a hashing algorithm, the lower the probability of more than one input providing the same hash (something you absolutely do not want in production). This is also known as **collision**. Most production-ready hashes such as SHA are essentially collision resistant. An example of a hash that is not collision resistant is demonstrated below:

```ts
function hashValue(password: number): number{ // input value
 
  const hash =  10 + (password % 2); // hash function
 
  console.log(`Original Password: ${password} --> Hashed Value: ${hash}`); // output (hashed) value
 
  return hash;
}
```

Here are examples of values that give the same hash with the following function ```hashValue()``` even though the input value was different:

```ts
Original Password: 4    -->   Hashed Value: 10
Original Password: 8    -->   Hashed Value: 10
Original Password: 32   -->   Hashed Value: 10
Original Password: 800  -->   Hashed Value: 10
```

This is a clear demonstration of collision taking place. If the above function was used in our blockchain, criminals could very easily defraud the chain by replacing blocks with fraudulent content that produces the same hash as the legitamate block (in our case, simply any even number). Some popular collision resistant hashing algorithms that we could use include:

- SHA-256
- SHA-512
- SHA-3
- BLAKE2s
- BLAKE2b
- MD2

The reason most of these are collision resistant is simply because the hash generated by the function created 2^256 possible combinations. For reference, that is more than the total number of grains of sands in the world!

A basic SHA-256 hash of the value "*hello world*" produces the following hash: **b94d27b9934d3e08a52e52d7da7dabfac484efe37a5380ee9088f7ace2efcde9**.

## Blockchain Blocks

Blocks usually contain:

- index (think of database primary keys)
- app-related data
- creation timestamp
- hash value (reference for future block)
- hash value of previous block (link to previous block)

The first block in a blockchain does not have a hash value for a previous block (field is usually empty string ```""```) and is known as the *genesis block*. Below are some examples of basic blocks:

```json
{
    "blocks": [
        {
            "id": 0,
            "timeStamp": "2022-8-4 11:0:23", 
            "data": "Genesis Block",
            "hash": "89eb0ac031a63d2421cd05a2fbe41f3ea35f5c3712ca839cbf6b85c4ee07b7a3",
            "previousHash": ""
        },
        {
            "id": 1,
            "timeStamp": "2022-8-4 11:15:38",
            "data": "John paid Oliver $20.",
            "hash": "1082573a933a0089ded219847c9f49a11b621471d57d083d1b63dfb58fc3d3b0",
            "previousHash": "89eb0ac031a63d2421cd05a2fbe41f3ea35f5c3712ca839cbf6b85c4ee07b7a3"

        },
        {
            "id": 2,
            "timeStamp": "2022-8-4 11:23:47",
            "data": "Jack paid Jacob $25.",
            "hash": "5fa522ae07f540e3d8dfbcbe2a904f51fcc5e6cfa2c6fcb6446eef6cfde8005c",
            "previousHash": "1082573a933a0089ded219847c9f49a11b621471d57d083d1b63dfb58fc3d3b0"
        }
    ]
}
```
*for simplicity we note timestamp here in human-readable string format, but usually an integer would be used for this*

If a criminal is able to recreate all three blocks in subsequent order with the same hashes but different data (such as ```"John paid Oliver $2000"``` for the second block) then that would be a succesful defraud. To prevent this from happening, a blockchain requires algorithms to be solved which takes time and resources. This work that is done to find the next block in the chain through solving the algorithm is what is known as block mining.

## Block Mining