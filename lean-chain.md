# Lean Chain

## Table of contents

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Introduction](#introduction)
- [Custom types](#custom-types)
- [Preset](#preset)
  - [Execution](#execution)
  - [Gwei values](#gwei-values)
  - [State list lengths](#state-list-lengths)
- [Constants](#constants)
  - [Execution layer triggered requests](#execution-layer-triggered-requests)
  - [Delegation Operations Request Types](#delegation-operations-request-types)
- [Configuration](#configuration)
  - [Time parameters](#time-parameters)
- [Containers](#containers)
  - [New containers](#new-containers)
    - [New `Delegator`](#new-delegator)
    - [New `DelegatedValidator`](#new-delegatedvalidator)
    - [New `DelegationOperationRequest`](#new-delegationoperationrequest)
    - [New `PendingActivateOperator`](#new-pendingactivateoperator)
    - [New `PendingDepositToDelegate`](#new-pendingdeposittodelegate)
    - [New `PendingDelegateRequest`](#new-pendingdelegaterequest)
    - [New `PendingUndelegateRequest`](#new-pendingundelegaterequest)
    - [New `PendingRedelegateRequest`](#new-pendingredelegaterequest)
    - [New `PendingWithdrawFromDelegatorRequest`](#new-pendingwithdrawfromdelegatorrequest)
    - [New `DelegationExitItem`](#new-delegationexititem)
    - [New `WithdrawalFromDelegators`](#new-withdrawalfromdelegators)
  - [Modified containers](#modified-containers)
    - [Modified `ExecutionRequests`](#modified-executionrequests)
    - [Modified `Validator`](#modified-validator)
    - [Modified `BeaconState`](#modified-beaconstate)
    - [Modified `ExecutionPayload`](#modified-executionpayload)
- [Beacon chain state transition function](#beacon-chain-state-transition-function)
  - [Block processing](#block-processing)
    - [New `get_expected_withdrawals_from_delegators`](#new-get_expected_withdrawals_from_delegators)
    - [New `process_withdrawals_from_delegators`](#new-process_withdrawals_from_delegators)
    - [New `process_delegation_operation_request`](#new-process_delegation_operation_request)
    - [Modified `process_block`](#modified-process_block)
      - [Modified `process_withdrawals`](#modified-process_withdrawals)
    - [Execution payload](#execution-payload)
      - [Modify `get_execution_requests_list`](#modify-get_execution_requests_list)
- [Helper functions](#helper-functions)
  - [Delegation helper functions](#delegation-helper-functions)
    - [New `register_new_delegator`](#new-register_new_delegator)
    - [New `get_delegated_validator`](#new-get_delegated_validator)
  - [Beacon state mutators](#beacon-state-mutators)
    - [New `initiate_delegated_balances_exit`](#new-initiate_delegated_balances_exit)
    - [Modified `slash_validator`](#modified-slash_validator)
    - [Modified `initiate_validator_exit`](#modified-initiate_validator_exit)
  - [Epoch processing](#epoch-processing)
    - [New `process_pending_deposits_to_delegate`](#new-process_pending_deposits_to_delegate)
    - [New `process_pending_activate_operators`](#new-process_pending_activate_operators)
    - [New `process_pending_delegations`](#new-process_pending_delegations)
    - [New `process_pending_undelegations`](#new-process_pending_undelegations)
    - [New `process_pending_redelegations`](#new-process_pending_redelegations)
    - [New `process_delegation_exit_queue`](#new-process_delegation_exit_queue)
    - [Modified process_rewards_and_penalties](#modified-process_rewards_and_penalties)
    - [Modified `process_slashings`](#modified-process_slashings)
    - [Modified `process_effective_balance_updates`](#modified-process_effective_balance_updates)
    - [New `is_validator_delegable`](#new-is_validator_delegable)
    - [Modified `process_epoch`](#modified-process_epoch)
    - [Modified `process_pending_consolidations`](#modified-process_pending_consolidations)
    - [Modified `process_registry_updates`](#modified-process_registry_updates)
    - [Operations](#operations)
      - [Modified `process_operations`](#modified-process_operations)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

---

## Introduction

*Note:* This document is the lean-chain specification and is under active development.

## Custom types

| Name          | SSZ equivalent | Description              |
|---------------|----------------|--------------------------|
| `StakerIndex` | `uint64`       | a stakers registry index | 
| `Quota`       | `uint64`       | a quota                  |

## Preset

### Execution

| Name                                                   | Value                     | Description                                                                      |
|--------------------------------------------------------|---------------------------|----------------------------------------------------------------------------------|
| `MAX_DELEGATION_OPERATIONS_REQUESTS_PER_PAYLOAD`       | `uint64(2**13)` (= 8,192) | Maximum number of execution layer delegation operations requests in each payload |
| `MAX_PENDING_WITHDRAWALS_FROM_DELEGATIONS_PER_PAYLOAD` | `uint64(2**4)` (= 16)     | Maximum number of withdraw from delegation operations requests in each payload   |

### Gwei values

| Name                 | Value                                  | Description                                         |
|----------------------|----------------------------------------|-----------------------------------------------------|
| `MIN_STAKER_BALANCE` | `Gwei(2**2 * 10**9)` (= 4,000,000,000) | Minimum balance for a validator to become delegable |

### State list lengths

| Name                                  | Value                                 | Unit    |
|---------------------------------------|---------------------------------------|---------|
| `STAKER_REGISTRY_LIMIT`               | `uint64(2**40)` (= 1,099,511,627,776) | stakers |

## Constants

### Execution layer triggered requests

| Name                                 | Value            |
|--------------------------------------|------------------|
| `DELEGATION_OPERATIONS_REQUEST_TYPE` | `Bytes1('0x03')` |

### Delegation Operations Request Types

| Name                                   | Value            |
|----------------------------------------|------------------|
| `ACTIVATE_OPERATOR_REQUEST_TYPE`       | `Bytes1('0x00')` |
| `DEPOSIT_TO_DELEGATE_REQUEST_TYPE`     | `Bytes1('0x01')` |
| `DELEGATE_REQUEST_TYPE`                | `Bytes1('0x02')` |
| `UNDELEGATE_REQUEST_TYPE`              | `Bytes1('0x03')` |
| `REDELEGATE_REQUEST_TYPE`              | `Bytes1('0x04')` |
| `WITHDRAW_FROM_DELEGATOR_REQUEST_TYPE` | `Bytes1('0x05')` |
| `EARLY_LIQUIDITY_REQUEST_TYPE`         | `Bytes1('0x06')` |

## Configuration

### Time parameters

| Name                                   | Value                    |  Unit  |  Duration  |
|----------------------------------------|--------------------------|:------:|:----------:|
| `MIN_DELEGATION_WITHDRAWABILITY_DELAY` | `uint64(2**11)` (= 2048) | epochs | ~218 hours |

## Containers

#### New `Staker`
```python

```

### New containers

#### Execution payload

## Helper functions

### Delegation helper functions

### Epoch processing

#### Operations
