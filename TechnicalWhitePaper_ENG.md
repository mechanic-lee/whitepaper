# MediBloc Technical Whitepaper (ENG) v0.2.1


## Abstract
MediBloc healthcare data platform fosters a patient-centric ecosystem while enabling collection, management, and utilization of healthcare data. Such an ecosystem requires verification of integrity and ownership of the data; thus, the platform incorporates blockchain technology as a core component to verify them. Blockchain is a tamper-evident, shared digital ledger which verifies and records a list of transactions including cryptographic signatures. Any modification to a ledger will also be verified by a community of peers, preventing possible attempts at forgery and fraud.

With this tamper-proof nature of the blockchain technology, MediBloc platform offers the verification of integrity and ownership as a key feature by recording the hash value of data on the blockchain. It also implements its own cryptocurrency, MED, to maintain the blockchain network by incentivizing activities valuable to the network. Furthermore, benefiting from the fundamental features of blockchain, the platform offers services such as validation of medical professionals, data relay, backup, and incentive scheme for data exchange that provide a virtuous cycle for all parties including, but not limited to doctors, patients, and medical researches. Features of MediBloc healthcare data platform mentioned above will be published and deployed through Software Development Kit (SDK) to enable third-party developers to easily integrate their existing applications with the core features of the MediBloc Platform.

## Blockchain
### Consensus Mechanism
MediBloc blockchain uses Delegated Proof of Stake (DPOS) consensus mechanism. One of the advantages of the DPOS consensus mechanism is its outstanding number of transactions per second. In DPOS consensus mechanism, a small number of block producers elected by the network users through voting can generate a new block at high speed while efficiently performing block synchronization. For the details of DPOS consensus mechanism, the team referred to the implementation of Steem and EOS.IO, which are the pioneers of this consensus mechanism; the details could be changed later in tailoring to the special use case of MediBloc platform.

In MediBloc blockchain, the block producers consist of the top 21 users who are determined by voting. In each round, elected 21 individuals, create one block every 3 seconds based on their respective account addresses in a specified order. Note that there is no limit in the block size; however, there is a limit for transaction size.

If the block producer is unable to produce a block because it tried to process too many transactions within a given block generation time, the block generation authority will be passed to the next block producer, and the previous block producer will suffer from economic loss. Therefore, block producers would prefer to include the transactions, which could be executed and verified in a given time, and, because each user pays for its transaction, malicious intentions to create excessively large blocks will be prevented.

The annual inflation rate in MediBloc blockchain is 5% and will be entirely given to the block producers as an incentive to produce blocks.

### Usage Rental Model
Unlike traditional database or data storage services, blockchain is a costly data storage space. Once recorded on the blockchain, the content is propagated to the global blockchain nodes, and each node goes through a process to verify the validity of this content to be included in the blockchain. Filling the blockchain only with records that add substantial value to the network is highly important as no content can be erased/modified and will remain in the blockchain in its original form at the time of recording.

The most widely used method to achieve this is the pay-per-transaction model. Blockchains such as Bitcoin and Ethereum are capable of using the pay-per-transaction model because it requires its own cryptocurrency as a fee for any new transactions and prevents any malicious users from filling the blockchain with meaningless data.

This model effectively prevents misuse of limited resources in the blockchain network. However, requiring users to constantly pay commission fee whenever the state of the blockchain needs modification, prevents users from using the services.

Hence, a service with frequent user usage needs a different approach. Among various alternatives that have been proposed in the blockchain community, MediBloc decided to use the bandwidth model; the fundamental concept of bandwidth model is to require users to stake a certain amount of cryptocurrency to get their shares of the total bandwidth. The users can utilize a portion of the total transaction of the entire network proportional to their staked cryptocurrency amount. Therefore, users can stake their MED according to how many transactions they would like to make in the blockchain network in a given time window. These staked assets could be withdrawn anytime when a user is not using the network.

| ![Image of Time window](https://github.com/medibloc/whitepaper/blob/master/images/TimeWindow.png) |
|:--:|
| *Time window* |

The bandwidth model is expected not only to lower the barriers for users by eliminating the constant expenditure but also to remove the inconvenience of constantly purchasing cryptocurrency to use the blockchain network. In this simple model described above, however, each user must hold more than the exact necessary amount of cryptocurrency to use the blockchain network. Also, to use the service, all users are required to go through various steps, such as purchasing the cryptocurrency on exchanges, that could act as obstacles in commercializing our services. To solve this problem, MediBloc introduces the concept of "Usage Rental Model."

In this model, any person with a certain amount of usage rights (e.g., bandwidth) on the blockchain network can handle transactions of other users in the network. For this feature, a new optional field named “secondary signature” or “payer signature” is introduced in transactions. If a transaction is submitted with the “payer signature” (secondary signature), the block producer processes the transaction based on the usage rights of the secondary signer, not the primary signer. As a result, transaction usage result reflects the transaction usage list of the second signer and the bandwidth is consumed accordingly.

In the usage rental model, a service provider on top of MediBloc platform can stake a large amount of cryptocurrency, provide a secondary signature for the service users’ transactions, and charge a certain amount of either cryptocurrency or fiat currency. The terms and conditions on providing secondary signatures are determined by the service providers, and MediBloc does not intend to enforce any limitation regarding this matter.

| ![Image of Rental model](https://github.com/medibloc/whitepaper/blob/master/images/RentalModel.png) |
|:--:|
| *Usage rental model* |

The fundamental principle of the usage rental model can be applied not only to the bandwidth model but also to the fee-based model. In this case, the term "pay-on-behalf" rather than "rental" would be the better term for the usage rights.

The pay-on-behalf payment model can have both advantages and disadvantages compared to the usage rental model. MediBloc platform adopted the usage rental model based on the bandwidth model because in the fee-based model, the amount of a certain cryptocurrency a secondary signer gets in return, decreases as the user uses the service, whereas the rental model offers the stability of the service and convenience for users.

In the usage rental model, usage rights are calculated in the same way as the bandwidth of the bandwidth model. Users can directly allocate their cryptocurrency to use a certain amount of bandwidth without renting the usage amount.

Bandwidth is calculated as the number of transactions available for seven days based on the amount of user’s staked cryptocurrency and the maximum transaction throughput of the network for seven days. The bandwidth recovers seven days after the transaction is generated.

### Merkle Tree of Healthcare Data
On MediBloc platform, healthcare data is processed and managed in the form of a Merkle tree. The process of making certain types of healthcare data(e.g., HL7 FHIR) into a Merkle tree must be supported by  services that use the specific types of data, and MediBloc provides software tools and guidelines to help facilitate this process.

| ![Image of Merkle tree](https://github.com/medibloc/whitepaper/blob/master/images/MerkleTree.png) |
|:--:|
| *HL7 FHIR to Merkle tree* |

One of the many advantages of managing data in the form of a Merkle tree is its capability to share only a portion of the data content. Data owner records the Root hash of the Merkle tree on the blockchain. Using the nature of the Merkle tree, it is possible to verify the partial data and whether it is included in the original data set, as long as the data is provided with the Merkel proof and the Root hash. Users can exclude personal identification information (PII) to de-identify themselves before transferring their data, as well as prove the data's credibility prior to transfer by disclosing what is requested by the data receiver such as the data creator's signature.

The original data converted to a Merkle tree is primarily stored in smartphone device of the use. Since only the root hash extracted from the data source is recorded in the blockchain, there is no restriction on the encryption method used in data storing and sharing process. Hence, even if the encryption method loses its security due to the increase of computing power, new encryption method can be applied at the service level without affecting the contents recorded in the blockchain.

### Certification of Healthcare Professionals
In real world, the certification of medical professional is currently taking different forms in different countries/regions. Moreover, because there is no automated verification system built in many cases, it is very difficult to build a common certification system that can encompass all different types of healthcare professional certifications.

Thus, MediBloc will not build one common certification system for all but rather allow one or more certification services for each country/region on the MediBloc platform in a decentralized manner. The certification of the medical professional is made by storing both the "Certifier" and "Certified" together in the blockchain. These records can easily be identified by users and the reliability of the certifications could be varied according to the “Certifier”.

| ![Image of Certification](https://github.com/medibloc/whitepaper/blob/master/images/Certification.png) |
|:--:|
| *Certifications on the blockchain* |

Certifying an account as a healthcare professional is accomplished by writing a transaction containing the information on the blockchain and anyone can do the action of certifying. The end users of the MediBloc platform determine whether a person claiming to be a healthcare professional by referring to the “Certifier” recorded on the blockchain. For this to be possible, an authentication service that authenticates the healthcare professionals using known public keys must be available on the platform. More details about healthcare professional certification service will be elaborated in sections below.

While it is possible for various certifiers to be registered on the blockchain, it is not necessary for users to discern the validity of the certifier each time. Based on the known public key information of a reliable certification authority in a specific area/region, it is possible to discern the validity of the certifier to the user in the application or service level.

The healthcare professional certification transaction also includes the hash value of the certification provided by the certifier and the expiration date. This certificate contains more detailed information about the healthcare data provider (eg., departmental, hospital) and is included in the data when the healthcare data provider sends the medical data (i.e. diagnosis) to the patient.

### Token Staking and Withdrawal
DPOS voting influence and the bandwidth allocated by the blockchain network increase in proportion to the amount of staked cryptocurrency. Conversely, voting influence and network bandwidth will be reduced as the cryptocurrency is withdrawn from staking.

While token staking happens instantaneously with the staking transaction, token withdrawal, an event where withdrawal amount is added to the balance of the account occurs exactly seven days after the withdrawal transaction. Unlike the token withdrawal, voting influence and network bandwidth quota decrease immediately upon the withdrawal transaction because it is comprehended as a waiver of the rights.

The token withdrawal time interval exists to prevent Denial-of-service (DoS) attacks such as transferring cryptocurrency to multiple accounts by repeating deposit and withdrawal to overflow the network and also to prevent users from only targeting for short-term gains.

### Smart Contract
MediBloc blockchain supports multiple transaction types that follow a predefined format to support the various features described above. The "Smart Contract" feature, which supports users to arbitrarily store and change data with appropriate transaction format, will not be supported in the initial version of MediBloc. However, the introduction of Smart Contract Engine (eg., EVM) will be continuously reviewed to enable various healthcare data-based applications to run on the MediBloc platform.

## Service
### Medical Professional Certification Service
The entity that records the certification details on MediBloc blockchain should be trusted by the platform participants. Fundamentally, as mentioned above, anyone can act as an authenticator, but during the initial state of the platform, MediBloc team will develop and directly operate the medical certification service.

Healthcare professionals who want to participate in the MediBloc platform are required to sign up to this service and submit the required documents to prove their qualifications. Upon verifying the legitimacy of a medical professional, the certification service issues a certificate for the healthcare professional and records corresponding data on the blockchain in a form of transaction. The transaction includes hash of the certificate and expiration date.

![Image of Certification process](https://github.com/medibloc/whitepaper/blob/master/images/CertificationProcess.png) |
|:--:|
| *Certification process* |

When a healthcare professional with a certificate loses the qualification for any reason, a transaction with disposal record of the certificate will be stored in the blockchain allowing platform participants to confirm. As the number of users of the platform and the use case increases, more trusted healthcare provider certification services will coexist with that of MediBloc team.

### Data Relay and Backup Services
There exists a risk of losing the healthcare data when stored only on the individual device. Also, in terms of providing the data, if a user needs to transmit his or her data directly in a peer-to-peer (P2P) manner, the user experience could vary vastly depending on the network environment of the device.

To solve these issues, a service that relays data transfer and backs up to secondary repository is required. The relay service takes responsibility in smoothly and securely delivering data from professional medical institution to a user and the user to any party that the user wishes to share his or her data.

| ![Image of Data relay](https://github.com/medibloc/whitepaper/blob/master/images/DataRelay.png) |
|:--:|
| *Data transfer* |

In this process, the data is encrypted by symmetric-key algorithm so that only the sender and receiver can access the content. To encrypt the data encryption key, MediBloc blockchain uses the Elliptic-Curve Diffie Hellman (ECDH) technique, which utilizes an elliptic curve key pair. In addition, the user can encrypt the data for exclusive access and back up the encrypted data.

In this case, user is not required to constantly transmit the entire data to the relay server. Instead, the receiver can download the encrypted data from the backup service and the user (the sender) could share asymmetrically encrypted decryption key for the data through the data relay service.

Furthermore, when a user only wants to disclose partial healthcare data (eg., a signature healthcare professional) for transaction purposes, keeping data in the backup service and opening the partial data for public access would be more efficient than sending the data every time it is requested. Likewise, in the case of the Merkle proof, if the sender stores the Merkle tree in the data backup service, the data receiver can access the Merkle tree from the backup service to prove the authenticity of the copy.

### Data Market Service
Data Market is provided to platform users as in-platform-service and the users can participate as data providers by disclosing only the partial contents of their own data to the market. Data buyers can set specific field value of the desired data and enable corresponding data provider to offer a deal on the data with the Merkle proof. Upon agreement of both parties, part or all the data source and the corresponding Merkle proof are transmitted to the purchaser through encrypted channel. Upon receipt, the authenticity of the data is checked through the given Merkle proof and the root hash is stored on the blockchain. Once the validity is proved, token transfer of the corresponding transaction is made, and the deal is concluded.

## Privacy Protection
High-level of privacy protection is essential in the management of healthcare data. Whenever new healthcare data is created and recorded to the platform, to prevent users from being able to deduce any private data from the blockchain, records such as data transfer history or information on data creator will not be recorded on the blockchain. Information about the data owner is included only in the data source when constructing the Merkle tree and only the root hash of the Merkle tree is stored in the blockchain making it impossible to deduce the data owner from the public data.

Instead, the receiver could verify that the information has not been manipulated by creating a Merkle tree containing the information of the data owner, which is directly received. Since data transfer is performed off-chain through the data relay service, the transfer cannot be traced with the public information.

Even when data is intercepted, whether using the P2P or the data relay service, there is no risk of data breach as the data is encrypted in a way that only the sender and the receiver can decrypt. In terms of data trade, details of trade are not stored in the blockchain. By executing data transfer off-chain and conducting a payment on a separate system from the blockchain, MediBloc prevents anyone without the data from acquiring any private data and opens possibilities of payment methods other than its own cryptocurrency.

Finally, the anonymity of users will be maintained as personal information of a user is not stored on the blockchain and healthcare data can be shared after making it unidentifiable using the Merkle tree.

## Conclusion
MediBloc platform consists of various components based on blockchain technology to enable management and utilization of healthcare data under complete control of the user. The ultimate goal of MediBloc platform is to create a completely patient-centric healthcare ecosystem using blockchain technology and the team is still in the process of enhancement of the platform with continuous re-validation and revision.

## Roadmap

| ![Image of roadmap](https://github.com/medibloc/whitepaper/blob/master/images/roadmap.png) |
|:--:|
| *MediBloc platform roadmap* |
