# Solana Vault Program

## Overview

The Solana Vault program is a decentralized application (dApp) on the Solana blockchain that allows users to securely deposit and withdraw their SPL tokens into a managed vault. The program is designed to provide a safe and efficient way for users to store their digital assets, while ensuring that the vault manager cannot withdraw the deposited funds.

## Features

- **Deposit SPL Tokens**: Users can deposit their SPL tokens of any kind into the vault, which will be securely stored and managed by the program.
- **Withdraw SPL Tokens**: Users can withdraw their deposited SPL tokens from the vault at any time, subject to a cooldown period.
- **Vault Manager Access Control**: The vault manager (the user who initializes the vault) is not able to withdraw the funds deposited in the vault, ensuring the safety and integrity of users' assets.
- **Cooldown Period**: The program implements a cooldown period (e.g., 24 hours) between withdrawals to prevent abuse and ensure vault stability.
- **Error Handling**: The program includes robust error handling, with custom error types for scenarios such as insufficient funds, invalid instructions, and arithmetic overflow.

## How It Works

### Key Components

- **Vault Account**: The program maintains a vault account that stores the following information:
  - `is_initialized`: A boolean flag indicating whether the vault has been initialized.
  - `owner`: The Pubkey of the vault owner (the user who created the vault).
  - `total_deposits`: The total amount of SPL tokens deposited in the vault.
  - `last_withdrawal_time`: The timestamp of the last withdrawal made from the vault.

### Deposit

When a user wants to deposit SPL tokens into the vault, they call the `deposit` instruction. The program performs the following steps:

1. Verifies that the user is the owner of the vault.
2. Transfers the specified amount of SPL tokens from the user's account to the vault account.
3. Updates the `total_deposits` field in the vault state.

### Withdraw

Users can withdraw their deposited SPL tokens from the vault by calling the `withdraw` instruction. The program executes the following logic:

1. Checks if the cooldown period (e.g., 24 hours) has elapsed since the last withdrawal.
2. Verifies that the user is the owner of the vault.
3. Transfers the specified amount (or 10% of the total deposits if no amount is specified) from the vault account to the user's account.
4. Updates the `total_deposits` field and the `last_withdrawal_time` in the vault state.

### Access Control

The program ensures that the vault manager (the user who created the vault) cannot withdraw the funds deposited in the vault. This is achieved by checking the `owner` field in the vault state and verifying that the transaction signer is the owner.

### Error Handling

The program includes custom error types to handle various error scenarios:

- `UninitializedAccount`: The vault account has not been initialized.
- `InsufficientFunds`: The vault does not have enough funds to fulfill the withdrawal request.
- `InvalidInstruction`: The instruction data provided is invalid.
- `AlreadyInitialized`: The vault account has already been initialized.
- `InvalidOwner`: The signer is not the owner of the vault account.
- `Overflow`: An arithmetic overflow has occurred.
- `WithdrawalCooldown`: The cooldown period has not yet elapsed since the last withdrawal.

## Project Structure

The Solana Vault program is organized into the following files:

- **`lib.rs`**: The main entry point of the program, which exports the `process_instruction` function.
- **`error.rs`**: Defines the custom error types used by the program.
- **`instruction.rs`**: Defines the instruction types and functions for creating transaction instructions.
- **`processor.rs`**: Implements the logic for processing each instruction.
- **`state.rs`**: Defines the `VaultState` struct and its associated methods.
- **`validation.rs`**: Defines input validation functions used throughout the program.
