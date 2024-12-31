# Microloan Smart Contract

## Overview
This smart contract facilitates a decentralized microloan system built on the Stacks blockchain. The contract enables borrowers to request loans, lenders to contribute to a lending pool, and repayments to be made with interest. It ensures proper state management and error handling while maintaining key functionalities like loan management and lending pool contributions.

---

## Features

### Core Functionalities
- **Lending Pool Contribution**: Lenders can contribute STX tokens to the lending pool.
- **Loan Request**: Borrowers can request loans with specific terms.
- **Repayment**: Borrowers can repay their loans incrementally or in full.

### Administrative Controls
- **Pause/Resume Contract**: The contract owner can pause or resume operations.
- **Update Loan Status**: The contract owner can update the status of active loans.

### Validation
- Ensures valid loan amounts, terms, and interest rates.
- Protects against overflows and unauthorized actions.

### Error Handling
Predefined error constants make it easy to identify and debug issues:
- `ERR-OVERFLOW`: Arithmetic overflow detected.
- `ERR-INVALID-AMOUNT`: Invalid loan amount.
- `ERR-INSUFFICIENT-BALANCE`: Insufficient lending pool balance.
- `ERR-LOAN-EXISTS`: Borrower already has an active loan.
- `ERR-LOAN-NOT-FOUND`: No active loan found for the borrower.
- `ERR-UNAUTHORIZED`: Unauthorized action.
- `ERR-INVALID-TERM`: Loan term is outside allowed range.
- `ERR-INVALID-RATE`: Interest rate is invalid.
- `ERR-LOAN-EXPIRED`: Loan term has expired.
- `ERR-LOAN-NOT-ACTIVE`: Loan is not in active status.
- `ERR-ZERO-ADDRESS`: Invalid borrower address.

---

## Data Structures

### Variables
- `contract-paused`: Indicates if the contract is active.
- `total-pool-amount`: Total STX in the lending pool.
- `total-active-loans`: Count of active loans.

### Maps
- `loans`: Tracks loan details for each borrower.
- `lending-pool-shares`: Tracks contributions of each lender.

---

## Contract Functions

### Public Functions

#### 1. **Contribute to Pool**
```lisp
(define-public (contribute-to-pool (amount uint)) -> (response bool uint))
```
Lenders contribute STX tokens to the lending pool.

- **Parameters**:
  - `amount`: The amount of STX to contribute.
- **Returns**: True on success or an error code.
- **Validations**:
  - Contract must be active.
  - `amount` must be within valid loan range.

#### 2. **Request Loan**
```lisp
(define-public (request-loan (amount uint) (term-blocks uint)) -> (response bool uint))
```
Borrowers request a loan from the pool.

- **Parameters**:
  - `amount`: Requested loan amount.
  - `term-blocks`: Loan term in blocks.
- **Returns**: True on success or an error code.
- **Validations**:
  - Contract must be active.
  - Borrower must not already have an active loan.
  - Loan amount and term must be valid.

#### 3. **Make Repayment**
```lisp
(define-public (make-repayment (amount uint)) -> (response bool uint))
```
Borrowers make repayments on their loans.

- **Parameters**:
  - `amount`: Repayment amount.
- **Returns**: True on success or an error code.
- **Validations**:
  - Contract must be active.
  - Loan must be active and not expired.
  - Repayment must not exceed the total owed.

### Read-only Functions

#### 1. **Calculate Interest**
```lisp
(define-read-only (calculate-interest (principal uint) (rate uint)) -> (response uint uint))
```
Calculates the interest on a given principal at a specific rate.

- **Parameters**:
  - `principal`: Principal loan amount.
  - `rate`: Annual interest rate.
- **Returns**: Interest amount or an error code.

#### 2. **Get Loan Details**
```lisp
(define-read-only (get-loan-details (borrower principal)) -> (optional {loan-details}))
```
Fetches loan details for a borrower.

#### 3. **Get Pool Share**
```lisp
(define-read-only (get-pool-share (lender principal)) -> (optional {share-details}))
```
Fetches the lending pool share of a lender.

#### 4. **Get Total Pool Amount**
```lisp
(define-read-only (get-total-pool-amount) -> uint)
```
Returns the total STX in the lending pool.

### Administrative Functions

#### 1. **Update Loan Status**
```lisp
(define-public (update-loan-status (borrower principal) (new-status (string-ascii 20))) -> (response bool uint))
```
Allows the contract owner to update the status of a loan.

#### 2. **Pause Contract**
```lisp
(define-public (pause-contract) -> (response bool uint))
```
Pauses the contract, disabling operations.

#### 3. **Resume Contract**
```lisp
(define-public (resume-contract) -> (response bool uint))
```
Resumes the contract, enabling operations.

---

## Constants
- `blocks-per-day`: Estimated number of blocks per day on the Stacks blockchain.
- `minimum-loan-amount`: Minimum loan amount (1 STX).
- `maximum-loan-amount`: Maximum loan amount (1000 STX).
- `minimum-term-blocks`: Minimum loan term (7 days).
- `maximum-term-blocks`: Maximum loan term (365 days).

---

## Deployment and Usage
1. Deploy the contract on the Stacks blockchain.
2. Use a Stacks wallet or developer tools to interact with the contract’s functions.
3. Ensure only the contract owner performs administrative actions.

---

## Future Enhancements
- Integration with decentralized identity (DID) systems for borrower verification.
- Implementation of variable interest rates based on reputation scores.
- Support for multi-currency lending and repayments.
- Automated liquidation for overdue loans.

---
