---
title: Technical Specifications
description: Summary of technical specifications of Stacks 2.0
---

## Consensus

- Proof of Transfer (PoX) as described in [SIP-007](https://github.com/blockstack/stacks-blockchain/blob/master/sip/sip-007-stacking-consensus.md)
- Network will transition to Proof of Burn (PoB) as described in [SIP-001](https://github.com/blockstack/stacks-blockchain/blob/master/sip/sip-001-burn-election.md) after 10 years. More details [here](https://github.com/blockstack/stacks-blockchain/blob/master/sip/sip-001-burn-election.md).
- For more details, see [Proof of Transfer](/understand-stacks/proof-of-transfer).

## Proof of Transfer Mining

- Coinbase reward schedule:
  - 1000 STX/block for first 4 years
  - 500 STX/block for following 4 years
  - 250 STX/block for subsequent 4 years
  - 125 STX/block in perpetuity after that
- Reward maturity window: 100 blocks, meaning leaders will earn the coinbase reward 100 blocks after the block they successfully mine.
- Block interval: Stacks blockchain produces blocks at the same rate as the underlying burnchain. For Bitcoin, this is approximately every 10 minutes.

## Stacking

- Stacking works in 2 phases
  1. Prepare phase: In this phase an "anchor block" is chosen. The qualifying set of addresses ("reward set") is determined based on the snapshot of the chain at the anchor block. Length of prepare phase is 250 blocks (overlaps with reward cycle of the previous phase).
  2. Reward phase: In this phase miner BTC commitments are distributed amongst the reward set. Reward cycle length is 2000 BTC blocks (~2 weeks).
- Two reward addresses / block, for a total of 4000 addresses every reward cycle. The addresses are chosen using a VRF (verifiable random function), so each node can deterministically arrive at the same reward addresses for a given block.
- Stacking threshold: 0.025% of the participating amount of STX when participation is between 25% and 100% and when participation is below 25%, the threshold level is always 0.00625 of the liquid supply of STX.
- Delegation: An STX address can designate another address to participate in Stacking on its behalf. [Relevant section in SIP-007](https://github.com/blockstack/stacks-blockchain/blob/master/sip/sip-007-stacking-consensus.md#stacker-delegation).
- Pooling: STX holders that individually do not meet the Stacking threshold can pool together their holdings to participate in Stacking. To do this, STX holders must set the (optional) reward address to the "delegate address". For more details, see [this reference](/references/stacking-contract#delegate-stx).
- Further reading: [Stacking](/understand-stacks/stacking)

## Accounts and Addresses

- Transactions in the Stacks blockchain originate from, are paid for by, and execute under the authority of accounts
- An account is fully specified by its address + nonce + assets
- Address contains 2 or 3 fields: 1 byte version, 20 byte public key hash (RIPEMD160(SHA256(input))), optional name (variable length, max 128 bytes)
- Two types of accounts: standard accounts are owned by one or more private keys; contract accounts are materialized when a smart-contract is instantiated (specified by the optional name field above)
- Nonce counts number of times an account has authorized a transaction. Starts at 0, valid authorization must include the _next_ nonce value.
- Assets are a map of all asset types -- STX, any on-chain assets specified by a Clarity contract (e.g. NFTs) -- to quantities owned by that account.
- Accounts need not be explicit "created" or registered; all accounts implicitly exist and are instantiated on first-use.
- Further reading: [Accounts](/understand-stacks/accounts)

## Transactions

- [Transaction types](/understand-stacks/transactions#types): coinbase, token-transfer, contract-deploy, contract-call, poison-microblock.
- Only standard accounts (not contracts) can pay transaction fees.
- Transaction execution is governed by 3 accounts (may or may not be distinct)
  1. _originating account_ is the account that creates, _authorizes_ and sends the transaction
  2. _paying account_ is the account that is billed by the leader for the cost of validating and executing the transaction
  3. _sending account_ is the account that identifies who is currently executing the transaction: this can change as a transaction executes via the `as-contract` Clarity function
- Two types of authorizations: standard authorization is where originating account is the same as paying account. _Sponsored_ authorization is where originating account and paying account are distinct. For instance, developers or service providers could pay for users to call their smart-contracts.
- For sponsored authorization, first a user signs with the originating account and then a sponsor signs with the paying account.
- Transaction encoding is described [here](https://github.com/blockstack/stacks-blockchain/blob/master/sip/sip-005-blocks-and-transactions.md#transaction-encoding) and [here](/understand-stacks/transactions#encoding)
- Transaction signing and verification are described [here](https://github.com/blockstack/stacks-blockchain/blob/master/sip/sip-005-blocks-and-transactions.md#transaction-signing-and-verifying) and [here](/understand-stacks/transactions.md#signature-and-verification)
- Further reading: [Transactions](/understand-stacks/transactions)
