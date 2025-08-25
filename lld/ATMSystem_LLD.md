# LLD: ATM System (Java)

## 🎯 Requirements (Minimal)
- User authentication via card and PIN
- Check account balance
- Withdraw cash
- Deposit cash
- Print mini statement

## 📦 Main Classes

### 1. ATM
**Fields:**
- CashDispenser cashDispenser
- Keypad keypad
- Screen screen
- CardReader cardReader
- Bank bank

**Methods:**
- authenticateUser(cardNumber, pin)
- performTransaction(transactionType)
- ejectCard()

### 2. CardReader
**Fields:**
- currentCard

**Methods:**
- insertCard(card)
- ejectCard()

### 3. Keypad
**Methods:**
- getInput()

### 4. Screen
**Methods:**
- displayMessage(msg)
- displayAmount(amount)

### 5. CashDispenser
**Fields:**
- int cashAvailable

**Methods:**
- dispenseCash(amount)
- refillCash(amount)

### 6. Bank
**Fields:**
- List<Account> accounts

**Methods:**
- validateUser(cardNumber, pin)
- getAccount(cardNumber)

### 7. Account
**Fields:**
- int accountNumber
- double balance
- List<Transaction> transactions

**Methods:**
- debit(amount)
- credit(amount)
- getBalance()
- addTransaction(transaction)

### 8. Transaction
**Fields:**
- TransactionType type
- double amount
- Date date

## 🏷️ Enums
```java
enum TransactionType { WITHDRAW, DEPOSIT, BALANCE_INQUIRY; }
```

## 🔑 Flow
- User inserts card → CardReader.insertCard(card)
- User enters PIN → ATM.authenticateUser(cardNumber, pin)
- If authenticated, user selects transaction
- ATM.performTransaction(type) interacts with Bank and CashDispenser as needed
- Screen displays result, card is ejected

## ⚡ Example (Simplified Usage)
```java
ATM atm = new ATM();
atm.cardReader.insertCard(card);
atm.authenticateUser(cardNumber, pin);
atm.performTransaction(TransactionType.WITHDRAW);
```

## 🚀 Extensions (Not in minimal LLD)
- Multi-currency support
- Receipt printing
- Fund transfers
- Mobile OTP authentication
