
# STX-Orbita

## Overview

**STX-orbita** is an advanced **fungible token contract** built on the Stacks blockchain using the Clarity smart contract language.
It extends the basic `define-fungible-token` functionality with **enhanced security, governance mechanics, and administrative controls**, making it suitable for modern DeFi, DAO, and utility-token use cases.

---

## ✨ Features

### ✅ Core Token

* **Fungible Token (FT):** Implements a token named **Stellar Token (STLR)** by default.
* **Precision:** Supports up to **6 decimal places**.
* **Maximum Supply:** Capped at `1,000,000,000,000,000` (1 trillion units).

### 🛡 Security Enhancements

* **Pause / Unpause:** Owner can temporarily halt transfers in emergencies.
* **Blacklist:** Restrict malicious addresses from participating.
* **Locked Balances:** Tokens can be locked until a block height.
* **Overflow Protection:** Safe math for addition and supply growth.
* **Allowance Expiry:** `approve` includes block-height based expiration.

### 👑 Governance

* **Voting Power:** Token holders automatically gain governance weight proportional to their balance.
* **Dynamic Updates:** Voting power updates on **mint**, **transfer**, and **burn**.

### 👨‍💼 Administrative Controls

* **Initialization:** Owner can set name, symbol, and decimals (only once).
* **Owner Transfer:** Ownership can be reassigned securely.
* **Minting:** Owner can mint new tokens within supply limits.
* **Pausing:** Owner can pause/unpause contract activity.

### 🔐 Allowances

* **Approve with Expiry:** Delegated spending rights expire after a block height, preventing perpetual approvals.
* **Allowance Query:** Supports real-time allowance checks.

---

## 📑 Contract Components

### Constants

* Error codes (`ERR-NOT-AUTHORIZED`, `ERR-INSUFFICIENT-BALANCE`, etc.)
* Token configuration:

  * `MAX-SUPPLY` → 1 trillion tokens
  * `PRECISION` → 6 decimal places

### Data Variables

* `contract-owner` → Current administrator
* `token-name`, `token-symbol`, `token-decimals`
* `total-supply`
* `paused`, `initialized`
* `last-event-id` → For tracking events

### Data Maps

* **allowances**: Owner → Spender approvals with expiry
* **locked-tokens**: Tokens restricted until block height
* **governance-tokens**: Voting power tracking
* **restricted-addresses**: Blacklisted users
* **minter-roles**: Addresses with minting rights

---

## ⚙️ Functions

### 🔍 Read-Only

* `get-name` → Returns token name
* `get-symbol` → Returns symbol
* `get-decimals` → Returns decimals
* `get-total-supply` → Returns circulating supply
* `get-balance` → Returns balance of an account
* `get-allowance` → Returns spender allowance (0 if expired)
* `is-locked` → Checks if account is under lock

### 🔧 Public

* **Admin**

  * `initialize(name, symbol, decimals)` → One-time setup
  * `set-contract-owner(new-owner)` → Transfer ownership
  * `pause` / `unpause` → Stop / resume contract activity
* **Token Operations**

  * `mint(amount, recipient)` → Mint tokens to recipient
  * `transfer(amount, recipient)` → Transfer tokens
  * `approve(amount, spender, expiry)` → Approve allowances with expiry

---

## 🚀 Usage Examples

### 1. Initialize Token

```clarity
(contract-call? .stx-orbita initialize "Stellar Token" "STLR" u6)
```

### 2. Mint Tokens

```clarity
(contract-call? .stx-orbita mint u100000 (tx-sender))
```

### 3. Transfer Tokens

```clarity
(contract-call? .stx-orbita transfer u500 (some-recipient))
```

### 4. Approve Allowance

```clarity
(contract-call? .stx-orbita approve u1000 spender-principal (+ block-height u144))
```

### 5. Pause Contract

```clarity
(contract-call? .stx-orbita pause)
```

---

## 📌 Security Considerations

* **Owner Privileges:** Owner can mint, pause, and reassign ownership.
* **Blacklist Feature:** Prevents malicious actors from token operations.
* **Allowance Expiry:** Mitigates "infinite allowance" risks.
* **Overflow Checks:** All arithmetic operations are safe.

---
