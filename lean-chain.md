# Lean Chain

## Table of contents

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Introduction](#introduction)
- [Custom types](#custom-types)
- [Preset](#preset)
  - [State list lengths](#state-list-lengths)
- [Constants](#constants)
  - [Staker Roles identifiers](#staker-roles-identifiers)
- [Containers](#containers)
  - [New containers](#new-containers)
    - [New `AttesterRole`](#new-attesterrole)
    - [New `IncluderRole`](#new-includerrole)
    - [New `ProposerRole`](#new-proposerrole)
    - [New `StakerRoleConfig`](#new-stakerroleconfig)
    - [New `Staker`](#new-staker)
    - [New `BeaconState`](#new-beaconstate)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

---

## Introduction

*Note:* This document is the lean-chain specification and is under active development.

## Custom types

We define the following Python custom types for type hinting and readability:

| Name          | SSZ equivalent | Description             |
|---------------|----------------|-------------------------|
| `Slot`        | `uint64`       | a slot number           |
| `Epoch`       | `uint64`       | an epoch number         |
| `StakerIndex` | `uint64`       | a staker registry index |
| `Gwei`        | `uint64`       | an amount in Gwei       |
| `BLSPubkey`   | `Bytes48`      | a BLS12-381 public key  |
| `Quota`       | `uint64`       | a quota                 |

## Preset

### State list lengths

| Name                     | Value                                 | Unit    |
|--------------------------|---------------------------------------|---------|
| `STAKERS_REGISTRY_LIMIT` | `uint64(2**40)` (= 1,099,511,627,776) | stakers |

## Constants

### Staker Roles identifiers

| Name            | Value            |
|-----------------|------------------|
| `ROLE_ATTESTER` | `Bytes1('0x00')` |
| `ROLE_INCLUDER` | `Bytes1('0x01')` |
| `ROLE_PROPOSER` | `Bytes1('0x02')` |

## Containers

### New containers

#### New `AttesterRole`

```python
class AttesterRole(Container):
    # Activation / Exit churn
    activation_eligibility_epoch: Epoch
    activation_epoch: Epoch
    exit_epoch: Epoch
    withdrawable_epoch: Epoch
    is_active: boolean
    # Accounting
    balance: Gwei
    # Slashing
    slashed: boolean
    # Delegation
    staker_quota: uint64
    delegations_quotas: List[Quota, DELEGATIONS_REGISTRY_LIMIT]
    delegated_balances: List[Gwei, DELEGATIONS_REGISTRY_LIMIT]
    total_delegated_balance: Gwei
```

#### New `IncluderRole`

```python
class IncluderRole(Container):
    # Activation
    is_active: boolean
    # Accounting
    balance: Gwei
    # Delegation
    staker_quota: uint64
    delegations_quotas: List[Quota, DELEGATIONS_REGISTRY_LIMIT]
    delegated_balances: List[Gwei, DELEGATIONS_REGISTRY_LIMIT]
    total_delegated_balance: Gwei
```

#### New `ProposerRole`

```python
class ProposerRole(Container):
    # Activation / Exit churn
    activation_eligibility_epoch: Epoch
    activation_epoch: Epoch
    exit_epoch: Epoch
    withdrawable_epoch: Epoch
    is_active: boolean
    # Accounting
    balance: Gwei
    # Slashing
    slashed: boolean
    # Delegation
    staker_quota: uint64
    delegations_quotas: List[Quota, DELEGATIONS_REGISTRY_LIMIT]
    delegated_balances: List[Gwei, DELEGATIONS_REGISTRY_LIMIT]
    total_delegated_balance: Gwei
```

#### New `StakerRoleConfig`

```python
class StakingRoleConfig(Container):
    role_identifier: Bytes1  # Staking roles Byte1 identifier
    active: boolean
    delegated: boolean
    target_staker: ExecutionAddress
    delegatable: boolean
    fee_quotient: uint64
```

#### New `Staker`

```python
class Staker(Container):
    # Staker identification
    pubkey: BLSPubkey
    withdrawal_credentials: Bytes32
    # Staker Configuration
    role_config: list[StakerRoleConfig, 3]
    attester_role: AttesterRole
    includer_role: IncluderRole
    proposer_role: ProposerRole
```

#### New `BeaconState`

```python
class BeaconState(Container):
    # Registry
    stakers: List[Staker, STAKERS_REGISTRY_LIMIT]
```