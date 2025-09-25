

# ClairyDataX - Data Marketplace Smart Contract

This Clarity smart contract implements a decentralized **Data Marketplace** on the **Stacks blockchain**, enabling users to **list**, **buy**, and **manage data assets** securely. The contract includes strong **input validation**, **access control**, and **automated revenue sharing**, providing a secure and extensible foundation for tokenized data commerce.

---

## 🚀 Features

* ✅ List data assets with metadata, price, and encrypted access keys
* ✅ Purchase assets using STX, with platform fees automatically handled
* ✅ Retrieve access keys for purchased assets
* ✅ Update asset prices and deactivate listings
* ✅ Tracks user sales and transaction history
* ✅ Modular and secure input validation
* ✅ Platform fee and ownership structure
* ✅ Designed for off-chain data delivery with on-chain validation

---

## 📜 Contract Details

| Name                  | Value / Description                              |
| --------------------- | ------------------------------------------------ |
| Contract Owner        | `marketplace-owner` (deployer of contract)       |
| Platform Fee          | `2%` of each sale (`marketplace-fee-percentage`) |
| Asset Description Max | `256` ASCII characters                           |
| Asset Category Max    | `64` ASCII characters                            |
| Access Key Max Length | `512` ASCII characters                           |

---

## 🧱 Data Structures

### `data-asset-listings`

Tracks all listed assets and their metadata.

```clojure
{
  asset-owner: principal,
  asset-price: uint,
  asset-description: (string-ascii 256),
  asset-category: (string-ascii 64),
  listing-active-status: bool,
  listing-creation-timestamp: uint
}
```

---

### `marketplace-user-profiles`

Stores profile data for each user (seller stats).

```clojure
{
  user-total-sales: uint,
  user-reputation-score: uint,
  user-last-activity-timestamp: uint
}
```

---

### `marketplace-transactions`

Tracks purchase transactions and timestamps.

```clojure
{
  transaction-timestamp: uint,
  transaction-amount: uint,
  asset-seller: principal
}
```

---

### `data-access-credentials`

Holds **encrypted** off-chain access keys for assets.

```clojure
{
  encrypted-access-key: (string-ascii 512)
}
```

---

## 🛠️ Functions

### `create-data-asset-listing`

Create a new listing with description, price, and encrypted access key.

```clojure
(asset-price uint) 
(asset-description string-ascii 256) 
(asset-category string-ascii 64)
(encrypted-access-key string-ascii 512)
```

📌 Validations:

* Non-empty fields
* Unique asset ID
* Price > 0

---

### `purchase-data-asset`

Purchase a listed asset. Automatically:

* Transfers STX to seller & platform
* Stores transaction
* Updates seller stats

```clojure
(data-asset-id uint)
```

📌 Only buyers (not the asset owner) can purchase.

---

### `retrieve-asset-access-key`

Returns the encrypted key for buyers of the asset.

```clojure
(data-asset-id uint)
```

📌 Only accessible to the verified buyer.

---

### `update-asset-price`

Allows seller to update price of a listed asset.

```clojure
(data-asset-id uint)
(updated-price uint)
```

📌 Price must be > 0, and only asset owner can update.

---

### `deactivate-asset-listing`

Deactivate a listing without deleting it.

```clojure
(data-asset-id uint)
```

📌 Only asset owner can perform this action.

---

## ❌ Error Codes

| Code   | Description                    |
| ------ | ------------------------------ |
| `u100` | Unauthorized marketplace owner |
| `u101` | Listing not found              |
| `u102` | Asset already listed           |
| `u103` | Insufficient STX               |
| `u104` | Unauthorized access            |
| `u105` | Invalid asset price            |
| `u106` | Invalid input                  |

---

## 📈 Marketplace Stats

* `asset-id-counter`: Tracks number of assets listed.
* `total-marketplace-transactions`: Tracks # of total purchases.

---

## 🔐 Security Highlights

* All input fields are validated (non-empty, within bounds).
* Asset access key only available to legitimate buyers.
* Platform fees automatically deducted and transferred.
* Each transaction and listing is immutable post-creation unless updated by the rightful owner.

---

## 💡 Design Considerations

* **Encrypted access keys** are stored on-chain; actual data delivery is off-chain, making it suitable for AI datasets, APIs, documents, and more.
* Easy to integrate with frontends (e.g., web3 dashboards) or Oracles for post-purchase data delivery.

---

## ✅ Deployment Checklist

* [ ] Test contract on **Stacks Testnet**.
* [ ] Set the `marketplace-owner` to correct principal.
* [ ] Ensure secure off-chain key delivery mechanism.
* [ ] Monitor `total-marketplace-transactions` for analytics.

---

## 📂 Example Usage (Clarity Console)

```clojure
;; Listing an asset
(contract-call? .data-marketplace create-data-asset-listing u100 
    "Access to climate model dataset" "Environment" "encrypted-key-abc123")

;; Purchasing asset with ID 0
(contract-call? .data-marketplace purchase-data-asset u0)

;; Retrieving asset access key (after purchase)
(contract-call? .data-marketplace retrieve-asset-access-key u0)

;; Updating price
(contract-call? .data-marketplace update-asset-price u0 u150)

;; Deactivating listing
(contract-call? .data-marketplace deactivate-asset-listing u0)
```

---

## 📜 License

MIT License — Free for commercial and personal use. Attribution appreciated.

---
