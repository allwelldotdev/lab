The definition of a blockchain, obtained from [Wikipedia](https://en.wikipedia.org/wiki/Blockchain)

> The **blockchain** is a [distributed ledger](https://en.wikipedia.org/wiki/Distributed_ledger "Distributed ledger") with growing lists of [records](https://en.wikipedia.org/wiki/Record_\(computer_science\) "Record (computer science)") (_blocks_) that are securely linked together via [cryptographic hashes](https://en.wikipedia.org/wiki/Cryptographic_hash_function "Cryptographic hash function").[1](https://en.wikipedia.org/wiki/Blockchain#cite_note-fortune20160515-1)[2](https://en.wikipedia.org/wiki/Blockchain#cite_note-nyt20160521-2)[3](https://en.wikipedia.org/wiki/Blockchain#cite_note-te20151031-3)[4](https://en.wikipedia.org/wiki/Blockchain#cite_note-cryptocurrencytech-4) Each block contains a cryptographic hash of the previous block, a [timestamp](https://en.wikipedia.org/wiki/Trusted_timestamping "Trusted timestamping"), and transaction data (generally represented as a [Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree "Merkle tree"), where [data nodes](https://en.wikipedia.org/wiki/Node_\(computer_science\) "Node (computer science)") are represented by leaves). Since each block contains information about the previous block, they effectively form a _chain_ (compare [linked list](https://en.wikipedia.org/wiki/Linked_list "Linked list") data structure), with each additional block linking to the ones before it. Consequently, blockchain transactions are resistant to alteration because, once recorded, the data in any given block cannot be changed retroactively without altering all subsequent blocks and obtaining network consensus to accept these changes.

Blockchain database managed autonomously using a peer-to-peer network and a distributed timestamping server.

Blockchain network consists of different layers:
- infra (hardware)
- networking (node discovery, info propagation and verification)
- consensus (proof of work, proof of stake)
- data (blocks, transactions)
- application (smart contracts/decentralized applications, if applicable)

Blocks hold batches of valid transactions (otherwise known as records) that are hashed and encoded into a Merkle tree.

Block time is the average time it takes for the network to generate one extra block in the blockchain. A shorted block time means faster transactions. The block time for [Ethereum](https://en.wikipedia.org/wiki/Ethereum) is set to between 14 and 15 seconds, while for bitcoin it is on average 10 minutes.

### Types
Obtained from [Wikipedia](https://en.wikipedia.org/wiki/Blockchain).
#### Public blockchains
A public blockchain has absolutely no access restrictions. Anyone with an [Internet](https://en.wikipedia.org/wiki/Internet "Internet") connection can send [transactions](https://en.wikipedia.org/wiki/Financial_transaction "Financial transaction") to it as well as become a validator (i.e., participate in the execution of a [consensus protocol](https://en.wikipedia.org/wiki/Consensus_\(computer_science\) "Consensus (computer science)")).[71](https://en.wikipedia.org/wiki/Blockchain#cite_note-71)_[self-published source?](https://en.wikipedia.org/wiki/Wikipedia:Verifiability#Self-published_sources "Wikipedia:Verifiability")_ Usually, such networks offer [economic incentives](https://en.wikipedia.org/wiki/Incentive "Incentive") for those who secure them and utilize some type of a [proof-of-stake](https://en.wikipedia.org/wiki/Proof-of-stake "Proof-of-stake") or [proof-of-work](https://en.wikipedia.org/wiki/Proof-of-work "Proof-of-work") algorithm.

Some of the largest, most known public blockchains are the bitcoin blockchain and the Ethereum blockchain.

#### Private blockchains
A private blockchain is permissioned.[53](https://en.wikipedia.org/wiki/Blockchain#cite_note-btit-53) One cannot join it unless invited by the network administrators. Participant and validator access is [restricted](https://en.wikipedia.org/wiki/Closed_platform "Closed platform"). To distinguish between open blockchains and other peer-to-peer decentralized database applications that are not open ad-hoc compute clusters, the terminology [Distributed Ledger](https://en.wikipedia.org/wiki/Distributed_Ledger "Distributed Ledger") (DLT) is normally used for private blockchains.

#### Hybrid blockchains
A hybrid blockchain has a combination of centralized and decentralized features.[72](https://en.wikipedia.org/wiki/Blockchain#cite_note-72) The exact workings of the chain can vary based on which portions of centralization and decentralization are used.

#### Sidechains
A sidechain is a designation for a blockchain ledger that runs in parallel to a primary blockchain.[73](https://en.wikipedia.org/wiki/Blockchain#cite_note-73)[74](https://en.wikipedia.org/wiki/Blockchain#cite_note-74) Entries from the primary blockchain (where said entries typically represent [digital assets](https://en.wikipedia.org/wiki/Digital_asset "Digital asset")) can be linked to and from the sidechain; this allows the sidechain to otherwise operate independently of the primary blockchain (e.g., by using an alternate means of record keeping, alternate [consensus algorithm](https://en.wikipedia.org/wiki/Consensus_\(computer_science\) "Consensus (computer science)"), etc.).[75](https://en.wikipedia.org/wiki/Blockchain#cite_note-75)_[better source needed](https://en.wikipedia.org/wiki/Wikipedia:NOTRS "Wikipedia:NOTRS")_

#### Consortium blockchain
A consortium blockchain is a type of blockchain that combines elements of both public and private blockchains. In a consortium blockchain, a group of organizations come together to create and operate the blockchain, rather than a single entity. The consortium members jointly manage the blockchain network and are responsible for validating transactions. Consortium blockchains are permissioned, meaning that only certain individuals or organizations are allowed to participate in the network. This allows for greater control over who can access the blockchain and helps to ensure that sensitive information is kept confidential.

Consortium blockchains are commonly used in industries where multiple organizations need to collaborate on a common goal, such as supply chain management or financial services. One advantage of consortium blockchains is that they can be more efficient and scalable than public blockchains, as the number of nodes required to validate transactions is typically smaller. Additionally, consortium blockchains can provide greater security and reliability than private blockchains, as the consortium members work together to maintain the network. Some examples of consortium blockchains include [Quorum](https://en.wikipedia.org/wiki/Quorum "Quorum") and [Hyperledger](https://en.wikipedia.org/wiki/Hyperledger "Hyperledger").[76](https://en.wikipedia.org/wiki/Blockchain#cite_note-76)


