## Week 13 - Day 3
### Blockchains
The blockchain is basically an append-only, public linked list  
It uses cryptographic hashes in a clever way to (mostly) guarantee that it is tamper proof  
Cryptocurrencies use a blockchain to store transactions (A gave a bitcoin to B)

#### The Double Spending Problem
This scheme ensure that user N indeed intends to give their coin to user N+1, but there is nothing stopping user N from ALSO giving that same coin to user M  
This problem is usually solved by using a bank as an intermediary. The bank will confirm that yes, user N indeed has adequate funds to cover the desired transaction  
Blockchain folks are strongly opposed to centralized authorities  
Blocks need to be chronological in order to help prevent this

Publishing Blocks in chronological order needs to be agreed on  
The blockchain solution: allow any node to publish a transaction (append to the tail of the linked list), but make it difficult to do so  
Reward users for appending to the linked list  
In case of forks, users should choose the longest chain as the authoritative version

#### Proof of Work
A linked list element is called a “block”  
A block contains:

* The hash of the previous block
* The transactions that are to be recorded in this block 
* A “nonce” an integer

In order to publish a block, a user must find a nonce value such that the hash of the block begins with a given number of 0 bits  
Since blockchains use cryptographic hash functions, the only way to find such a nonce is to guess and check until you get lucky.  
By adjusting the number of required 0 bits, blockchains can adjust how difficult it is to find a “good” nonce

#### Bitcoin Network
When you want to make a transaction, you broadcast it to all nodes  
All nodes collect the transactions they hear about until they have enough to make a block  
Nodes then search for a suitable nonce for the block When they find it, they announce to the network  
Nodes check to make sure all transactions are valid (the users involved have enough balance to cover the transactions)  
If they accept it, they start working on the next block

#### Bitcoin Mining
The first transaction in a block says “create a new cryptocoin, and give it to the user who publishes this block”  
This rewards nodes that work to find nonces, and help the chain to be published  
Transactions can also include a “fee” to be paid to the user who publishes the block  
Users who try to find these nonces are “miners” who guess nonces

#### Merkle Trees
A Merkle tree is a way to verify an array of data in a compressed way  
The elements of the array are the leaves of a binary tree  
Each internal node is the hash of its children concatenated together  
If you only care about a subset of transactions, you can discard MOST of the tree and still preserve its hash  
Starting from the transaction you care about, move up the tree. Keep all siblings along this path, but discard their descendants. This is enough to verify the root hash  
Merkle Trees are used in other applications. For example, in git, they are used to determine what changes exist between different commits.

#### Smart Contracts
Transactions can contain “executable code”  
Basically, if the network decides that the conditions of a contract are met, it validates the transaction, executing the code  
Otherwise, it does not

#### Downsides
Anything in the blockchain is public, forever  
People can “graffiti” the blockchain to store illicit or obnoxious things in it permanently  
Transactions are pseudo-anonymous (identifiable only by public key), but since they’re public, it’s possible to examine the transaction history and possibly identify users. Any bug found in the future affects ALL previous transactions!
Since Bitcoin is often used to buy illegal goods/services, this is a concern for users!

#### Power Usage
Bitcoin operates using the “proof of work” concept  
It adjusts the hash difficulty so that one block is approved every ~10 minutes
Because of the current value, a huge network of miners works to find appropriate nonces.  
At one point, the network used more electricity than Ireland!  
Since nonce guessing is easily parallelizable, miners use GPUs, and created a scarcity of graphics cards!