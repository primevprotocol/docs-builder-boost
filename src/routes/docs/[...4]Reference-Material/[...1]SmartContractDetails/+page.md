---
title: BuilderStaking Smart Contract
description: The `BuilderStaking` is a Solidity smart contract, it implements a staking system where providers can set their staking requirements for others to subscribe to their services over Primev.
---

# {$frontmatter.title}

{$frontmatter.description}

:::admonition type="info"
The current contract is deployed on Seploia testnet at Address: [0x6219a236EFFa91567d5ba4a0A5134297a35b0b2A](https://sepolia.etherscan.io/address/0x6219a236EFFa91567d5ba4a0A5134297a35b0b2A)
:::

 It has three main types of storage:

- `BuilderInfo`: Holds the minimum stake and subscription period requirements for each builder.
- `StakeInfo`: Holds information about the stake, such as the end of the subscription and the amount staked.
- `TimeLock`: Holds information about the locking of funds, including the initial amount, the remaining amount, the start time, and the lock duration.

## Data Structures

- `BuilderInfo` struct: Contains the `minimalStake` and `minimalSubscriptionPeriod` for a builder. These are attributes set by the builder.
- `StakeInfo` struct: Contains the `subscriptionEnd` and `stake` details of a commitment. Representing the state of a Users access to a builder. For more information on Commitments (TODO)click here.

- `TimeLock` struct: Contains the `initialAmount`, `remainingAmount`, `startTime`, and `lockDuration` of a locked fund.
- `builders` mapping: Maps builder addresses to their respective `BuilderInfo`.
- `stakes` mapping: Maps commitment hashes to their respective `StakeInfo`.
- `timeLocks` mapping: Maps addresses to an array of `TimeLock` records for that address.

## Events

- `StakeUpdated`: Emitted when a deposit operation is successful.
- `BuilderUpdated`: Emitted when the builder updates their stake requirement or subscription period.
- `Withdrawal`: Emitted when a withdrawal operation is successful.

## Functions

- `deposit(address _builder, bytes32 _commitment)`: Allows the caller to deposit Ethereum for a specific builder, associating the deposit with a specific commitment hash. This updates the stake information and locks a portion of the funds for the builder.
- `updateBuilder(uint256 _minimalStake, uint256 _minimalSubscriptionPeriod)`: Allows the caller to set their minimal stake amount and subscription period.
- `withdraw()`: Allows the caller to withdraw their funds if the lock duration has passed.
- `withdrawableAmount()`: Returns the total amount of Ethereum that the caller can currently withdraw.
- `hasMinimalStake(address _builder, bytes32 _commitment)`: Returns whether a given commitment has at least the minimal stake for the given builder.
- `timeLocksCount(address _address)`: Returns the number of current time locks for a given address.
- `getSubscriptionPeriod(address _builder, uint256 _stakeAmount)`: Returns the number of blocks that the specified stake would subscribe to the given builder.

## Private Functions

- `_lockFunds(address _address, uint256 _amount, uint256 _lockDuration)`: Locks a specific amount of Ethereum for a duration of time for the specified address.
- `_removeTimeLock(address _address, uint256 _index)`: Removes a specific time lock for the given address.
- `_getSubscriptionPeriod(uint256 _minimalStake, uint256 _minimalSubscriptionPeriod, uint256 _stakeAmount)`: Calculates the number of blocks for a subscription based on the stake amount and the builder's minimal stake and subscription period requirements.
