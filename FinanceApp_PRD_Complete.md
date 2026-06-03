# Personal Finance Intelligence App — PRD

> **Version:** 1.0  
> **Platform:** Android (Flutter + Kotlin SMS Bridge)  
> **Architecture:** 100% Local · Offline-First · Privacy-First  
> **Language:** Dart (Flutter) + Kotlin (SMS only)  

---

## Document Index

| # | Section | File |
|---|---|---|
| 1 | Core Philosophy | [01_core_philosophy.md](01_core_philosophy.md) |
| 2 | Tech Stack | [02_tech_stack.md](02_tech_stack.md) |
| 3 | SMS Processing Engine | [03_sms_engine.md](03_sms_engine.md) |
| 4 | Smart Transaction Detection | [04_smart_detection.md](04_smart_detection.md) |
| 5 | Identity Resolution | [05_identity_resolution.md](05_identity_resolution.md) |
| 6 | Account Management | [06_account_management.md](06_account_management.md) |
| 7 | Transaction Management | [07_transaction_management.md](07_transaction_management.md) |
| 8 | Transaction List & Filters | [08_filters.md](08_filters.md) |
| 9 | Debt & Split Tracking | [09_debt_split.md](09_debt_split.md) |
| 10 | Transaction Linking | [10_linking.md](10_linking.md) |
| 11 | Recurring Intelligence | [11_recurring.md](11_recurring.md) |
| 12 | Budget System | [12_budget.md](12_budget.md) |
| 13 | Dashboard | [13_dashboard.md](13_dashboard.md) |
| 14 | Insights & Reports | [14_insights.md](14_insights.md) |
| 15 | Notifications & Alerts | [15_notifications.md](15_notifications.md) |
| 16 | Location Features | [16_location.md](16_location.md) |
| 17 | Security & Privacy | [17_security.md](17_security.md) |
| 18 | Backup & Restore | [18_backup.md](18_backup.md) |
| 19 | Goals & Savings | [19_goals.md](19_goals.md) |
| 20 | Tags System | [20_tags.md](20_tags.md) |
| 21 | Gmail Integration (Phase 2) | [21_gmail.md](21_gmail.md) |
| 22 | Onboarding | [22_onboarding.md](22_onboarding.md) |
| 23 | UI & UX | [23_ui_ux.md](23_ui_ux.md) |
| 24 | Settings | [24_settings.md](24_settings.md) |
| 25 | iOS Companion (Phase 3) | [25_ios.md](25_ios.md) |
| 26 | Future Roadmap | [26_roadmap.md](26_roadmap.md) |

---

## One-Line Summary

> A privacy-first, fully local, AI-assisted personal finance app that automatically reads bank SMS, intelligently extracts and categorises transactions, learns from user behaviour, and presents a polished fintech-grade UI — built in Flutter with a native Kotlin SMS bridge.
# 01 · Core Philosophy

## Principles

- **100% local, offline-first** — no internet required for any core feature
- **No raw data ever leaves the device** — SMS, transactions, profiles all stay local
- **Privacy-first** — user owns all their data, always
- **AI only as optional fallback** — never mandatory, never forced
- **Works on low-end Android phones** — target ₹8,000–₹15,000 devices
- **Battery-safe** — event-driven only, zero background polling
- **No backend dependency** for any core feature
- **Smart by default** — the app learns and improves silently over time

---

## What This App Is

A **personal finance intelligence layer** that:

- Listens to bank transactions via SMS
- Understands intent (not just debit/credit)
- Learns patterns over time
- Feels premium and fluid
- Respects privacy completely

---

## What This App Is NOT

- Not a banking app
- Not a cloud-synced service
- Not dependent on any API for core functionality
- Not another basic expense tracker

---

## The Core Promise to Users

> "Zero effort. Every transaction tracked. Nothing leaves your phone."
# 02 · Tech Stack

## Decision Summary

| Factor | Choice | Reason |
|---|---|---|
| Framework | Flutter | AI-assisted dev, cross-platform later, hot reload |
| Language | Dart | Familiar to TypeScript devs, consistent AI output |
| SMS Bridge | Kotlin | Only native code — written once, never touched again |
| IDE | VS Code + Flutter ext | Or Android Studio |

---

## Full Stack

### Core
| Layer | Technology | Purpose |
|---|---|---|
| Framework | Flutter | UI + business logic |
| Language | Dart | Everything except SMS |
| SMS Engine | Kotlin (bridge) | BroadcastReceiver — one file |
| Architecture | Feature-first + Repository | Clean separation |
| State Management | Riverpod | Scales well for this complexity |

### Data
| Layer | Technology | Purpose |
|---|---|---|
| Local Database | Drift (SQLite) | All transactions, profiles, accounts |
| Secure Storage | flutter_secure_storage | PIN, biometric keys, encryption |
| Preferences | shared_preferences | App settings |

### Background & System
| Layer | Technology | Purpose |
|---|---|---|
| Background Tasks | workmanager | Scheduled processing |
| Notifications | flutter_local_notifications | All alerts |
| Location | geolocator | Passive coarse location |

### UI
| Layer | Technology | Purpose |
|---|---|---|
| Navigation | Go Router | Declarative routing |
| Animations | flutter_animate | Bouncy, spring, fluid |
| Charts | fl_chart | Dashboard graphs |
| Icons | Material + custom | Consistent iconography |

### Phase 2 (Gmail)
| Layer | Technology | Purpose |
|---|---|---|
| Auth | google_sign_in | OAuth login |
| Gmail API | googleapis | Read bank emails |
| HTTP | dio | API calls |

---

## The SMS Bridge — The Only Native Code

```
Flutter App (Dart)
      │
      │  MethodChannel / EventChannel
      │
Kotlin Bridge
      │
      ├── SmsReceiver.kt      (BroadcastReceiver)
      ├── SmsChannel.kt       (Flutter bridge)
      └── SmsService.kt       (Processing logic)
```

### What Kotlin handles (one time setup, never touched again)
- Registering BroadcastReceiver in AndroidManifest
- Catching SMS on arrival even when app is closed
- Sending raw SMS body + sender to Flutter via EventChannel
- Requesting SMS permissions natively

### What Flutter/Dart handles (everything else)
- Parsing SMS content
- Confidence scoring
- Profile matching
- All business logic
- All UI
- Database
- Everything

---

## Why This Stack

```
Java/Spring Boot background
        ↓
Dart feels like TypeScript (you know this)
Drift feels like JPA (you know this)
Riverpod feels like Spring DI (you know this)
Repository pattern (you know this)
        ↓
Productive from week 1
```

---

## Folder Structure

```
lib/
├── core/
│   ├── database/           # Drift tables, DAOs, database instance
│   ├── models/             # All data models
│   ├── parser/             # SMS parsing engine (pure Dart)
│   │   ├── sms_classifier.dart
│   │   ├── regex_engine.dart
│   │   ├── confidence_scorer.dart
│   │   └── field_extractor.dart
│   └── utils/              # Constants, helpers, extensions
│
├── features/
│   ├── dashboard/
│   ├── transactions/
│   ├── accounts/
│   ├── budgets/
│   ├── debts/
│   ├── reports/
│   ├── goals/
│   └── settings/
│
├── shared/
│   ├── widgets/            # Reusable UI components
│   ├── theme/              # Colors, typography, spacing
│   └── animations/         # Reusable animation wrappers
│
android/
└── app/src/main/kotlin/
    ├── SmsReceiver.kt      # BroadcastReceiver
    ├── SmsChannel.kt       # Flutter MethodChannel bridge
    └── MainActivity.kt     # Channel registration
```
# 03 · SMS Processing Engine

> This is the heart of the entire app. Everything else depends on this working perfectly.

---

## How It Works End to End

```
Bank sends SMS
      │
      ▼
Android BroadcastReceiver (Kotlin)
catches it instantly — even app closed
      │
      ▼
EventChannel sends to Flutter
raw body + sender ID only
      │
      ▼
Message Classifier (Dart)
Is this a transaction? OTP? Promo?
      │
      ├── OTP → ignore completely
      ├── Promotional → ignore
      ├── Balance alert → update balance, no transaction
      ├── Statement → parse separately
      └── Transaction → continue below
      │
      ▼
Universal Regex Engine (Dart)
Extract all fields
      │
      ▼
Confidence Scorer (Dart)
Score each field independently
      │
      ▼
Profile Matcher (Dart)
Match against known merchants/persons
      │
      ├── Score > 80 → auto save silently
      ├── Score 50–80 → show suggestion, user confirms
      └── Score < 50 → show review screen or AI fallback
      │
      ▼
Saved to Drift (SQLite)
Transaction stored locally
      │
      ▼
UI updates reactively
Dashboard, list, balance all update
```

---

## Layer 1 — Kotlin BroadcastReceiver

### What it does
- Registered in AndroidManifest.xml
- Fires the instant any SMS arrives
- Filters nothing — sends everything to Flutter
- Flutter decides what to process

### AndroidManifest entries
```xml
<uses-permission android:name="android.permission.READ_SMS"/>
<uses-permission android:name="android.permission.RECEIVE_SMS"/>

<receiver android:name=".SmsReceiver"
    android:exported="true">
    <intent-filter android:priority="999">
        <action android:name="android.provider.Telephony.SMS_RECEIVED"/>
    </intent-filter>
</receiver>
```

### SmsReceiver.kt
```kotlin
class SmsReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        val messages = Telephony.Sms.Intents
            .getMessagesFromIntent(intent)
        for (msg in messages) {
            SmsChannel.sendToFlutter(
                body = msg.messageBody,
                sender = msg.originatingAddress ?: ""
            )
        }
    }
}
```

---

## Layer 2 — Sender ID Whitelist

### How Indian bank sender IDs work
TRAI mandates all banks use registered 6-character sender IDs:

| Bank | Sender IDs |
|---|---|
| HDFC | HDFCBK, HDFCBN, HDFCRD |
| SBI | SBMSBI, SBIINB, SBICRD |
| ICICI | ICICIB, ICICIT, ICICRD |
| Axis | AXISBK, AXISBN |
| Kotak | KOTAKB, KOTKRD |
| YES Bank | YESBNK |
| IndusInd | INDBNK |
| IDFC | IDFCBK |
| Federal | FEDBAK |
| Paytm | PYTMBN |
| PhonePe | PHNEPE |
| Amazon Pay | AMZNPAY |

### Whitelist logic
```dart
class SenderWhitelist {
  // Core bank sender IDs
  static const Set<String> _knownBankSenders = {
    'HDFCBK', 'HDFCBN', 'HDFCRD',
    'SBMSBI', 'SBIINB', 'SBICRD',
    'ICICIB', 'ICICIT', 'ICICRD',
    'AXISBK', 'AXISBN',
    'KOTAKB', 'KOTKRD',
    'YESBNK', 'INDBNK',
    'IDFCBK', 'FEDBAK',
    'PYTMBN', 'PHNEPE',
    // ... all banks
  };

  // User can add custom sender IDs
  static Set<String> _userAddedSenders = {};

  static bool isKnownBank(String sender) {
    final upper = sender.toUpperCase();
    return _knownBankSenders.contains(upper) ||
           _userAddedSenders.contains(upper);
  }
}
```

---

## Layer 3 — Message Classifier

### Classification types
```dart
enum SmsClassification {
  transaction,      // Process fully
  balanceAlert,     // Update balance only
  otp,              // Ignore completely
  promotional,      // Ignore
  statement,        // Parse separately
  loanAlert,        // Special handling
  unknown,          // Flag for user if from bank
}
```

### Classification logic
```dart
class SmsClassifier {

  SmsClassification classify(String body, String sender) {

    // OTP check — highest priority, ignore immediately
    if (_isOtp(body)) return SmsClassification.otp;

    // Only process known bank senders further
    if (!SenderWhitelist.isKnownBank(sender)) {
      return SmsClassification.unknown;
    }

    // Promotional
    if (_isPromotional(body)) return SmsClassification.promotional;

    // Balance alert (no transaction)
    if (_isBalanceAlert(body)) return SmsClassification.balanceAlert;

    // Statement summary
    if (_isStatement(body)) return SmsClassification.statement;

    // Loan/EMI alert
    if (_isLoanAlert(body)) return SmsClassification.loanAlert;

    // Default — treat as transaction
    return SmsClassification.transaction;
  }

  bool _isOtp(String body) {
    return RegExp(
      r'\b(OTP|one.time.password|verification.code)\b',
      caseSensitive: false
    ).hasMatch(body) || RegExp(r'\b\d{4,8}\b is your').hasMatch(body);
  }

  bool _isPromotional(String body) {
    return RegExp(
      r'\b(offer|discount|cashback offer|win|congratulations|apply now|click|limited time)\b',
      caseSensitive: false
    ).hasMatch(body);
  }

  bool _isBalanceAlert(String body) {
    return RegExp(
      r'\b(balance|avl bal|available balance|bal is|bal:)\b',
      caseSensitive: false
    ).hasMatch(body) &&
    !RegExp(r'\b(debited|credited|spent|received)\b',
      caseSensitive: false).hasMatch(body);
  }

  bool _isStatement(String body) {
    return RegExp(
      r'\b(statement|mini statement|account statement)\b',
      caseSensitive: false
    ).hasMatch(body);
  }

  bool _isLoanAlert(String body) {
    return RegExp(
      r'\b(EMI|loan installment|due date|overdue)\b',
      caseSensitive: false
    ).hasMatch(body);
  }
}
```

---

## Layer 4 — Universal Regex Engine

### Amount extraction
```dart
// Handles all Indian bank amount formats:
// Rs.500 / Rs 500 / INR 500 / ₹500 / Rs.500.00 / Rs 1,500.00
static final RegExp amountPattern = RegExp(
  r'(?:Rs\.?|INR|₹)\s*([\d,]+(?:\.\d{1,2})?)',
  caseSensitive: false,
);
```

### Transaction type (debit/credit)
```dart
static final RegExp debitPattern = RegExp(
  r'\b(debited|deducted|spent|withdrawn|paid|used for|purchase)\b',
  caseSensitive: false,
);

static final RegExp creditPattern = RegExp(
  r'\b(credited|received|deposited|refund|reversed|cashback|added)\b',
  caseSensitive: false,
);
```

### Account number extraction
```dart
// Matches: XX1234 / XXXX1234 / x-1234 / ending 1234 / last 4 digits 1234
static final RegExp accountPattern = RegExp(
  r'(?:a[/]?c|account|acct|card)[\s\w]*?(?:XX+|x+[-]?)(\d{4})',
  caseSensitive: false,
);
```

### Merchant extraction
```dart
// Matches: at AMAZON / to VPA rahul@ybl / towards SWIGGY / at POS DMART
static final RegExp merchantPattern = RegExp(
  r'(?:at|to|towards|from|via)\s+([A-Z][A-Z0-9\s\-&]{2,30}?)(?:\s+on|\s+via|\s+ref|\.|,|$)',
  caseSensitive: false,
);
```

### Transaction channel
```dart
static final RegExp channelPattern = RegExp(
  r'\b(UPI|IMPS|NEFT|RTGS|ATM|POS|online|net.?banking|auto.?debit|NACH|cheque|BNPL|FASTag)\b',
  caseSensitive: false,
);
```

### Reference number
```dart
static final RegExp refPattern = RegExp(
  r'(?:ref|txn|transaction|UPI ref)[.\s#:]*([A-Z0-9]{8,20})',
  caseSensitive: false,
);
```

### Full extractor
```dart
class FieldExtractor {

  ParsedSms extract(String body) {
    return ParsedSms(
      amount: _extractAmount(body),
      isDebit: _extractType(body),
      accountSuffix: _extractAccount(body),
      merchant: _extractMerchant(body),
      channel: _extractChannel(body),
      referenceNumber: _extractReference(body),
      rawBody: body,
    );
  }

  double? _extractAmount(String body) {
    final match = amountPattern.firstMatch(body);
    if (match == null) return null;
    final raw = match.group(1)!.replaceAll(',', '');
    return double.tryParse(raw);
  }

  bool? _extractType(String body) {
    if (debitPattern.hasMatch(body)) return true;   // isDebit
    if (creditPattern.hasMatch(body)) return false;  // isCredit
    return null; // unknown
  }

  String? _extractAccount(String body) {
    return accountPattern.firstMatch(body)?.group(1);
  }

  String? _extractMerchant(String body) {
    return merchantPattern.firstMatch(body)?.group(1)?.trim();
  }

  TransactionChannel? _extractChannel(String body) {
    final match = channelPattern.firstMatch(body);
    if (match == null) return null;
    return _mapToChannel(match.group(1)!);
  }
}
```

---

## Layer 5 — Confidence Scoring

### Per-field scoring
```dart
class ConfidenceScorer {

  ConfidenceResult score(ParsedSms parsed) {
    final scores = <String, int>{};

    // Amount — most reliable field
    scores['amount'] = parsed.amount != null ? 40 : 0;

    // Type (debit/credit) — very reliable
    scores['type'] = parsed.isDebit != null ? 20 : 0;

    // Account suffix — reliable
    scores['account'] = parsed.accountSuffix != null ? 15 : 0;

    // Merchant — variable quality
    scores['merchant'] = _scoreMerchant(parsed.merchant);

    // Channel — reliable when present
    scores['channel'] = parsed.channel != null ? 10 : 0;

    final total = scores.values.fold(0, (a, b) => a + b);

    return ConfidenceResult(
      overall: total,
      fieldScores: scores,
      action: _determineAction(total),
    );
  }

  int _scoreMerchant(String? merchant) {
    if (merchant == null) return 0;
    if (merchant.length < 3) return 2;
    if (merchant.length > 20) return 5; // might be noise
    return 15;
  }

  ConfidenceAction _determineAction(int score) {
    if (score >= 80) return ConfidenceAction.autoSave;
    if (score >= 50) return ConfidenceAction.suggestToUser;
    return ConfidenceAction.reviewRequired;
  }
}

enum ConfidenceAction {
  autoSave,       // Save silently, notify gently
  suggestToUser,  // Show card, ask to confirm
  reviewRequired, // Open review screen
}
```

---

## Layer 6 — Unknown Format Handler

```dart
class UnknownFormatHandler {

  // When a known bank sender sends an unrecognised format
  Future<void> handle(String body, String sender) async {

    // Log for user review
    await _db.flaggedMessages.insertOne(FlaggedMessage(
      body: body,
      sender: sender,
      timestamp: DateTime.now(),
      status: FlagStatus.pending,
    ));

    // Notify user
    _notifier.show(
      title: 'New transaction needs review',
      body: 'Tap to fill in the details',
      route: '/review/$id',
    );
  }

  // When user manually fills in details
  // Pattern saved permanently — never asked again
  Future<void> savePattern(String sender, ParsedSms userCorrected) async {
    await _patternDB.save(SmsPattern(
      sender: sender,
      extractedFields: userCorrected,
      learnedAt: DateTime.now(),
    ));
  }
}
```

---

## Supported Banks — India

### PSU Banks
SBI · PNB · Bank of Baroda · Canara · Union Bank · Indian Bank · UCO · Bank of India · Central Bank · Bank of Maharashtra

### Private Banks
HDFC · ICICI · Axis · Kotak · YES · IndusInd · IDFC First · Federal · RBL · Bandhan · CSB · DCB · Karur Vysya · South Indian · City Union · Nainital · Tamilnad Mercantile

### Small Finance Banks
AU · Equitas · Jana · Ujjivan · ESAF · Fincare · Capital · Northeast · Suryoday · Unity

### Payments Banks
Airtel · Jio · India Post · Paytm · NSDL · Fino

### Others
Regional co-operative banks · NBFCs with SMS alerts

---

## Supported Transaction Channels

| Channel | Detection Method |
|---|---|
| UPI | Keyword "UPI" + VPA handle (@okaxis etc.) |
| Debit Card Swipe | "POS" keyword |
| Debit Card Online | "online" + card reference |
| Credit Card | Card number + merchant |
| Net Banking | "NEFT" / "IMPS" / "RTGS" keywords |
| ATM Withdrawal | "ATM" keyword |
| Auto-debit / NACH | "auto debit" / "NACH" / "mandate" |
| Cheque | "cheque" / "CHQ" + number |
| Wallet | Sender ID (Paytm, PhonePe etc.) |
| BNPL | Provider name in body |
| FASTag | "FASTag" / "toll" keyword |
| EMI | "EMI" + loan reference |

---

## Historical SMS Import (First Run)

```dart
class HistoricalImporter {

  Future<ImportResult> importHistory({int monthsBack = 6}) async {
    // Read all SMS from device
    final allMessages = await _smsReader.readAll();

    // Filter to bank messages only
    final bankMessages = allMessages.where(
      (m) => SenderWhitelist.isKnownBank(m.sender)
    ).toList();

    // Process each
    final results = <ParsedTransaction>[];
    for (final msg in bankMessages) {
      final parsed = await _parser.parse(msg.body, msg.sender);
      if (parsed != null) results.add(parsed);
    }

    return ImportResult(
      totalScanned: allMessages.length,
      bankMessages: bankMessages.length,
      transactionsFound: results.length,
      transactions: results,
    );
  }
}
```

On first run:
1. Scan last 6 months of SMS
2. Auto-discover all bank accounts
3. Pre-populate transaction history
4. User reviews and confirms
5. Sets opening balances
6. Ready — months of history instantly
# 04 · Smart Transaction Detection

> Beyond basic debit/credit — understanding what actually happened.

---

## All Transaction Types

| Type | How Detected | Special Handling |
|---|---|---|
| Expense | Debit keyword + merchant | Categorised automatically |
| Income | Credit keyword + known income sources | Tagged as income |
| Internal Transfer | Both accounts registered by user | NOT counted as expense |
| Credit Card Payment | Payment to registered CC account | Smart migration logic |
| Person Payment | UPI to individual / contact match | Identity resolution |
| Person Receipt | Credit from individual | Identity resolution |
| ATM Withdrawal | "ATM" keyword | Auto-linked to cash account |
| EMI Deduction | "EMI" keyword | Linked to loan account |
| Loan Repayment | Loan sender ID + repayment keyword | Reduces loan balance |
| Refund | Credit from merchant + "refund" keyword | Linked to original transaction |
| Cashback | "cashback" keyword | Tagged separately from income |
| Reversal | "reversed" / "failed" keyword | Linked to original |
| Auto-debit | "auto debit" / "NACH" | Recurring flag applied |
| Wallet Load | Wallet sender + credit | Adds to wallet balance |
| FASTag Toll | "FASTag" / "toll" | Location tagged if available |
| BNPL Repayment | BNPL provider sender | Linked to BNPL account |
| Interest Credit | "interest" + credit | Income sub-type |

---

## Internal Transfer Detection

### Setup
User pre-registers all their own accounts:
```
My accounts:
├── HDFC Savings XX1234
├── ICICI Savings XX5678
├── Kotak Savings XX9012
└── HDFC Credit Card XX3456
```

### Detection logic
```dart
class InternalTransferDetector {

  bool isInternalTransfer(ParsedSms parsed) {
    // Check if destination account matches any registered account
    final destinationAccount = parsed.merchant ?? parsed.toAccount;
    if (destinationAccount == null) return false;

    return _userAccounts.any((account) =>
      account.matchesIdentifier(destinationAccount)
    );
  }
}
```

### Result
- Marked as Internal Transfer
- NOT counted in expense or income reports
- Balance moves correctly between both accounts
- Shown separately in reports

---

## Credit Card Bill Payment — Smart Migration

This is the most complex detection in the app.

### Detection
```dart
bool isCreditCardPayment(ParsedSms parsed) {
  // Payment going to a registered credit card
  return _userCreditCards.any((card) =>
    parsed.merchant?.contains(card.lastFourDigits) == true ||
    parsed.toAccount == card.accountSuffix
  );
}
```

### Migration Flow
```
User pays ₹12,000 credit card bill
              │
              ▼
App detects: Credit Card Bill Payment
              │
              ▼
App asks user:
"Move this cycle's CC expenses to HDFC Savings?"
              │
        ┌─────┴─────┐
       Yes           No
        │             │
        ▼             ▼
All CC expenses    Just record
for this cycle     payment as-is
migrate to
HDFC Savings
        │
        ▼
CC balance resets to ₹0
No double counting — ever
```

### What migration means
- All credit card expenses for current billing cycle get reassigned to the paying bank account
- Credit card account balance resets to zero
- Net financial picture remains accurate
- No transaction is deleted — only reassigned

### Cycle tracking
```dart
class CreditCardCycle {
  final String cardAccountId;
  final DateTime cycleStart;
  final DateTime cycleEnd;
  final DateTime dueDate;
  final double totalDue;
  final double minimumDue;
  bool isPaid;
}
```

---

## ATM Withdrawal → Cash Account Auto-Link

```
ATM withdrawal SMS detected
          │
          ▼
Transaction created: HDFC Savings — ₹5,000 debit
          │
          ▼
Cash Account automatically credited ₹5,000
          │
          ▼
User logs cash expenses manually
OR splits the withdrawal:
  ₹2,000 → Auto
  ₹1,500 → Groceries
  ₹1,500 → Untracked
```

---

## Person vs Merchant Detection

### How it decides
```dart
enum PayeeType { person, merchant, unknown }

class PayeeClassifier {

  PayeeType classify(ParsedSms parsed) {

    // UPI handle present
    if (parsed.upiHandle != null) {
      // Contains digits → likely person's phone number
      if (_containsPhoneNumber(parsed.upiHandle!)) {
        return PayeeType.person;
      }
      // Known merchant UPI handle
      if (_isKnownMerchantHandle(parsed.upiHandle!)) {
        return PayeeType.merchant;
      }
    }

    // Name matches phonebook contact
    if (_matchesContact(parsed.merchant)) {
      return PayeeType.person;
    }

    // Known merchant dictionary
    if (_isKnownMerchant(parsed.merchant)) {
      return PayeeType.merchant;
    }

    return PayeeType.unknown;
  }

  bool _containsPhoneNumber(String handle) {
    return RegExp(r'\d{10}').hasMatch(handle);
  }
}
```

---

## Auto-Categorisation

### Category rules (applied in order)

```dart
class AutoCategoriser {

  TransactionCategory categorise(ParsedSms parsed) {

    final merchant = parsed.merchant?.toLowerCase() ?? '';
    final body = parsed.rawBody.toLowerCase();

    // Food & Dining
    if (_matches(merchant, ['swiggy', 'zomato', 'dominos', 'mcdonalds',
        'kfc', 'subway', 'pizza', 'restaurant', 'cafe', 'hotel'])) {
      return TransactionCategory.foodDining;
    }

    // Groceries
    if (_matches(merchant, ['bigbasket', 'grofers', 'blinkit', 'dmart',
        'reliance fresh', 'more supermarket', 'spencer'])) {
      return TransactionCategory.groceries;
    }

    // Travel
    if (_matches(merchant, ['irctc', 'makemytrip', 'goibibo', 'cleartrip',
        'ola', 'uber', 'rapido', 'redbus', 'indigo', 'spicejet', 'airindia'])) {
      return TransactionCategory.travel;
    }

    // Fuel
    if (_matches(merchant, ['petrol', 'bpcl', 'iocl', 'hpcl', 'reliance petroleum',
        'shell', 'fuel', 'fastrack'])) {
      return TransactionCategory.fuel;
    }

    // Shopping
    if (_matches(merchant, ['amazon', 'flipkart', 'myntra', 'ajio', 'meesho',
        'nykaa', 'snapdeal'])) {
      return TransactionCategory.shopping;
    }

    // Bills & Utilities
    if (_matches(merchant, ['electricity', 'bescom', 'msedcl', 'tata power',
        'airtel', 'jio', 'bsnl', 'vi ', 'vodafone', 'water board'])) {
      return TransactionCategory.billsUtilities;
    }

    // Health
    if (_matches(merchant, ['pharmacy', 'apollo', 'medplus', 'netmeds',
        '1mg', 'hospital', 'clinic', 'doctor', 'diagnostic'])) {
      return TransactionCategory.health;
    }

    // Entertainment
    if (_matches(merchant, ['netflix', 'hotstar', 'prime video', 'spotify',
        'youtube', 'bookmyshow', 'pvr', 'inox'])) {
      return TransactionCategory.entertainment;
    }

    // ATM
    if (body.contains('atm')) return TransactionCategory.atm;

    // EMI
    if (body.contains('emi')) return TransactionCategory.emi;

    // Default
    return TransactionCategory.uncategorised;
  }

  bool _matches(String text, List<String> keywords) {
    return keywords.any((k) => text.contains(k));
  }
}
```

### Learning layer
Every time user corrects a category:
```dart
// Save merchant → category mapping permanently
await _learningDB.saveMerchantCategory(
  merchant: 'SHARMA MEDICAL',
  category: TransactionCategory.health,
);

// Next time SHARMA MEDICAL appears — auto-categorised correctly
// No user input needed ever again
```

---

## Recurring Transaction Detection

```dart
class RecurringDetector {

  Future<bool> isRecurring(Transaction newTx) async {
    // Look for same merchant, similar amount, regular interval
    final similar = await _db.transactions
      .filter()
      .merchantEqualTo(newTx.merchant)
      .findAll();

    if (similar.length < 2) return false;

    // Check amount similarity (within 10%)
    final amountMatch = similar.every((tx) =>
      (tx.amount - newTx.amount).abs() / newTx.amount < 0.10
    );

    // Check interval regularity
    final intervalMatch = _hasRegularInterval(similar);

    return amountMatch && intervalMatch;
  }

  bool _hasRegularInterval(List<Transaction> transactions) {
    if (transactions.length < 2) return false;

    final intervals = <int>[];
    for (int i = 1; i < transactions.length; i++) {
      intervals.add(
        transactions[i].timestamp
          .difference(transactions[i-1].timestamp)
          .inDays
      );
    }

    // Check if intervals are consistent (within 3 days tolerance)
    final avg = intervals.reduce((a, b) => a + b) / intervals.length;
    return intervals.every((d) => (d - avg).abs() < 3);
  }
}
```

---

## Split / Return Detection

```dart
class SplitReturnDetector {

  // User paid ₹500 → later receives multiple smaller credits
  Future<SplitReturnSuggestion?> detectReturn(Transaction credit) async {

    // Look for recent debits of similar or larger amount
    final recentDebits = await _db.transactions
      .filter()
      .isDebitEqualTo(true)
      .timestampGreaterThan(
        DateTime.now().subtract(Duration(days: 90))
      )
      .findAll();

    for (final debit in recentDebits) {
      // Check if this credit could be full return
      if ((credit.amount - debit.amount).abs() < 1) {
        return SplitReturnSuggestion(
          originalTransaction: debit,
          returnTransaction: credit,
          type: ReturnType.full,
        );
      }

      // Check if this credit could be partial return
      final existingReturns = await _getLinkedReturns(debit.id);
      final totalReturned = existingReturns
        .fold(0.0, (sum, tx) => sum + tx.amount);
      final remaining = debit.amount - totalReturned;

      if ((credit.amount - remaining).abs() < 1 ||
          credit.amount < remaining) {
        return SplitReturnSuggestion(
          originalTransaction: debit,
          returnTransaction: credit,
          type: ReturnType.partial,
          remainingAfter: remaining - credit.amount,
        );
      }
    }

    return null;
  }
}
```

When detected, app asks:
> "₹100 received from Rahul — is this a return of your ₹500 payment on 20 May?"
# 05 · Identity Resolution System

> The app builds a private social financial graph — who you pay, who pays you, how much is pending.

---

## The Problem

The same person can appear many ways across transactions:

```
SMS text:        "RAHUL SHARMA"
UPI handle:      9876543210@okaxis
Phonebook:       "Rahul S" / "Rahul Sharma Work"
Previous tag:    "Rahul Bhai"
```

All the same person. The app figures this out automatically.

---

## Step 1 — Phone Number Detection in UPI Handle

```dart
class UpiPhoneExtractor {

  String? extractPhone(String upiHandle) {
    // e.g. 9876543210@okaxis → 9876543210
    final match = RegExp(r'^(\d{10})@').firstMatch(upiHandle);
    return match?.group(1);
  }

  Future<Contact?> matchToContact(String phone) async {
    final contacts = await _contactsReader.getAll();
    return contacts.firstWhereOrNull(
      (c) => c.phones.any((p) =>
        p.replaceAll(RegExp(r'\D'), '').endsWith(phone)
      )
    );
  }
}
```

If match found → auto-tag with contact name, no user input needed.

---

## Step 2 — Fuzzy Name Matching

```dart
class FuzzyNameMatcher {

  Future<List<ContactMatch>> findMatches(String smsName) async {
    final contacts = await _contactsReader.getAll();
    final results = <ContactMatch>[];

    for (final contact in contacts) {
      final score = _fuzzyScore(
        smsName.toLowerCase(),
        contact.displayName.toLowerCase()
      );
      if (score > 0.7) {
        results.add(ContactMatch(contact: contact, score: score));
      }
    }

    results.sort((a, b) => b.score.compareTo(a.score));
    return results;
  }

  double _fuzzyScore(String a, String b) {
    // Levenshtein distance normalised
    // + common word matching
    // + initials matching
    final commonWords = _commonWords(a, b);
    final levScore = 1 - (_levenshtein(a, b) / max(a.length, b.length));
    return (commonWords * 0.6) + (levScore * 0.4);
  }
}
```

---

## Step 3 — User Confirmation (Once Only)

When high confidence match found:

```
App shows:
┌─────────────────────────────────┐
│  Is "RAHUL SHARMA" the same as  │
│  Rahul Sharma in your contacts? │
│                                 │
│  [Yes, link them]  [No]  [Later]│
└─────────────────────────────────┘
```

### On "Yes"
- Permanently linked in identity database
- All past transactions retroactively linked to this person
- All future transactions auto-resolved — never asked again
- Person profile built immediately

### On "No"
- Treated as separate entity
- Not asked again for same combination

### On "Later"
- Reminded if same pattern appears 2+ more times
- Eventually auto-confirmed if pattern is strong enough

---

## Step 4 — Person Profile (Built Over Time)

```dart
class PersonProfile {
  final String id;
  final String displayName;         // As confirmed by user
  final List<String> phoneNumbers;
  final List<String> upiHandles;    // All linked UPI IDs
  final List<String> smsNameVariations; // RAHUL, RAHUL SHARMA, R SHARMA
  final double totalSentToThem;     // Lifetime
  final double totalReceivedFromThem; // Lifetime
  double get netBalance =>
    totalReceivedFromThem - totalSentToThem; // + means they owe you
  final List<String> groupIds;      // Groups they belong to
  final List<String> transactionIds; // All transactions with them
}
```

---

## Merchant Profile (Built Over Time)

```dart
class MerchantProfile {
  final String id;
  final String normalizedName;      // "Swiggy" (not "SWIGGY" or "SWGY")
  final List<String> smsVariations; // All name forms seen in SMS
  final TransactionCategory category;
  final double totalSpentLifetime;
  final int transactionCount;
  double get averageTransaction =>
    totalSpentLifetime / transactionCount;
  final Map<int, double> spendByDayOfWeek; // 0=Mon, 6=Sun
  final TransactionChannel mostUsedChannel;
  final DateTime lastTransactionDate;
}
```

---

## Identity Database Schema

```dart
// Drift table definitions

class PersonProfiles extends Table {
  IntColumn get id => integer().autoIncrement()();
  TextColumn get displayName => text()();
  TextColumn get phoneNumbers => text()(); // JSON array
  TextColumn get upiHandles => text()();   // JSON array
  TextColumn get smsVariations => text()(); // JSON array
  RealColumn get totalSent => real().withDefault(const Constant(0))();
  RealColumn get totalReceived => real().withDefault(const Constant(0))();
  DateTimeColumn get createdAt => dateTime()();
  DateTimeColumn get updatedAt => dateTime()();
}

class MerchantProfiles extends Table {
  IntColumn get id => integer().autoIncrement()();
  TextColumn get normalizedName => text()();
  TextColumn get smsVariations => text()();
  IntColumn get categoryId => integer()();
  RealColumn get totalSpent => real().withDefault(const Constant(0))();
  IntColumn get transactionCount => integer().withDefault(const Constant(0))();
  DateTimeColumn get lastSeen => dateTime()();
}
```
# 06 · Account Management

## Account Types

| Type | Source | Special Handling |
|---|---|---|
| Savings Account | Auto-discovered from SMS | Standard |
| Current Account | Auto-discovered | Standard |
| Credit Card | Auto-discovered | Cycle tracking, migration |
| Wallet | Auto-discovered (Paytm, PhonePe etc.) | Separate balance |
| Cash | Virtual — manual only | ATM auto-link |
| Loan Account | Auto-discovered | Outstanding balance |
| Joint Account | User flags manually | Shared indicator |

---

## Account Setup

Each account stores:
- User-defined name
- Bank / institution
- Account number (last 4 digits only)
- Account type
- Opening balance (user sets)
- Currency (default INR)
- User-picked colour & icon
- Include in net worth toggle
- Active / closed status

---

## Balance Management

```
Opening balance (user sets)
        +
All credits detected via SMS
        -
All debits detected via SMS
        =
Current running balance (auto-calculated)
```

### User confirms balance periodically
- User checks bank app / passbook
- Enters actual balance in app
- App compares with calculated balance

---

## Balance Discrepancy — Smart Reason System

When a difference is detected, app asks why:

| Reason | How App Handles It |
|---|---|
| SMS was deleted | Ghost transaction created for difference, flagged |
| Bank fee not via SMS | Manual transaction created |
| Interest credited | Income transaction created |
| Cheque cleared (no SMS) | Manual transaction created |
| Opening balance correction | Adjusts opening only, history preserved |
| I made a mistake | Helps find and fix specific transaction |
| Other | Free text saved, difference logged |

### After correction
- Correction point marked clearly in timeline
- All future transactions count from corrected balance
- Full audit trail maintained
- Previous history preserved exactly as-is

---

## Credit Card Specific

- Credit limit stored
- Available credit shown (limit - balance)
- Bill cycle start and end dates
- Bill amount per cycle
- Minimum due tracked
- Due date reminder
- Overlimit alert
- Utilisation % shown on card detail

---

## Cash Account

- Virtual account — no bank linked
- ATM withdrawals auto-increase cash balance
- User manually logs cash expenses
- Split ATM withdrawal into multiple expenses
- Cash income supported (rent received, sold something)
- Cash in hand always visible on dashboard

---

## Loan Account

- Outstanding principal balance
- EMI amount
- EMI due date
- Next due date reminder
- Tenure remaining (calculated)
- Total interest paid so far
# 07 · Transaction Management

## Transaction Card — What's Displayed

```
┌──────────────────────────────────────────┐
│ 🏦 HDFC XX1234          27 May, 2:30 PM │
│                                          │
│ Swiggy                         -₹450    │
│ Food & Dining · UPI     📍 Koramangala  │
│                                          │
│ #lunch  🔗 split with Rahul  ↻ recurring│
└──────────────────────────────────────────┘
```

### Fields shown on card
- Bank icon + masked account number
- Merchant / person name
- Amount (red = debit, green = credit)
- Category + payment channel
- Date & time
- Location (city/area if available)
- Tags
- Link indicator (linked to another transaction)
- Pending indicator (part of debt/split)
- Recurring indicator
- Confidence indicator (subtle — for auto-parsed)
- Edit indicator (if user modified)

---

## Full Transaction Editing

Every single field is editable by the user:

| Field | Editable |
|---|---|
| Amount | ✅ |
| Date & time | ✅ |
| Transaction type | ✅ |
| Category & subcategory | ✅ |
| Account | ✅ |
| Merchant / person name | ✅ |
| Payment method | ✅ |
| Tags | ✅ |
| Notes (free text) | ✅ |
| Location (override) | ✅ |
| Linked transactions | ✅ |
| Split details | ✅ |
| Debt details | ✅ |
| Recurring flag | ✅ |

---

## Edit History

- Every edit logged with timestamp
- Original parsed value vs current value always visible
- Revert to original anytime
- Optional reason for edit saved

---

## Manual Transaction Entry

Two modes:

### Quick Add (minimal friction)
- Amount
- Category
- Account
- Tap save — done

### Full Add
- All fields available
- Cash, manual bank, backdated, future-dated all supported

---

## Payment Method Detection

| Method | Detection |
|---|---|
| UPI | "UPI" keyword + @handle |
| Debit Card Swipe | "POS" keyword |
| Debit Card Online | "online" + card reference |
| Credit Card | Card digits + merchant |
| Net Banking | NEFT / IMPS / RTGS |
| ATM | "ATM" keyword |
| Auto-debit | "NACH" / "mandate" |
| Cheque | "CHQ" / cheque number |
| Wallet | Sender ID |
| BNPL | Provider name |
| FASTag | "toll" / "FASTag" |
| Cash | Manual only |

All auto-detected methods can be overridden by user. Correction saved and learned.
# 08 · Transaction List & Filters

## List View Grouping

- By date (default)
- By week
- By month
- By category
- By account
- By person
- By tag

---

## Sort Options

- Newest first (default)
- Oldest first
- Highest amount first
- Lowest amount first
- Alphabetical by merchant
- By category

---

## Filter — By Time

- Today / Yesterday
- This week / Last week
- This month / Last month
- This quarter / Last quarter
- This financial year (Apr–Mar)
- Last financial year
- Custom date range
- All time

---

## Filter — By Amount

- Any
- Less than ₹500
- ₹500 – ₹5,000
- ₹5,000 – ₹50,000
- Above ₹50,000
- Custom range
- Exact amount

---

## Filter — By Type

- All
- Expenses only
- Income only
- Internal transfers
- Credit card payments
- Refunds
- Debt payments
- Recurring only
- Non-recurring only
- Needs review
- Cash only
- Manual entries

---

## Filter — By Account

- All accounts
- Any specific account
- All savings / all credit cards / all wallets
- Cash only
- Exclude internal transfers

---

## Filter — By Category

- All categories
- Any specific category or subcategory
- Uncategorised only

---

## Filter — By Channel

- All / UPI / Debit card / Credit card
- Net banking / ATM / Auto-debit
- Cheque / Wallet / Cash / Manual

---

## Filter — By Person

- All transactions with a specific person
- Shows full history with them
- Outstanding balance shown at top

---

## Filter — By Tag

- Any user-created tag
- Multiple tags: AND / OR combinable
- Untagged only

---

## Filter — By Status

- All
- Auto-parsed high confidence
- Needs review (low confidence)
- Manually entered
- Linked transactions
- Has pending debt
- Has location
- No category assigned
- Edited by user
- Has notes

---

## Search

- Free text across merchant, person, notes, tags, reference number
- Amount search
- Search within active filters
- Recent searches saved
- Search suggestions

---

## Bulk Actions

- Select multiple transactions
- Bulk recategorise
- Bulk tag / untag
- Bulk delete
- Bulk link / unlink
- Bulk mark as reviewed
- Export selected

---

## Export

- CSV (Excel compatible)
- PDF statement
- Filter then export
- Share via WhatsApp / email / any app
# 09 · Debt & Split Tracking

## Debt Recording

- You paid for someone → record debt owed to you
- Someone paid for you → record debt you owe
- Standalone debt (no transaction required)
- Partial debt (you paid ₹1,000, your share ₹400 → ₹600 is debt)
- Multi-person debt (you paid ₹3,000 for 4 people)

---

## Smart Split Return Detection

```
User pays ₹500 on 1 May
              │
              ▼
App watches for return credits
              │
5 May: Rahul sends ₹100
10 May: Priya sends ₹100
15 May: Amit sends ₹100
              │
              ▼
App detects pattern, asks:
"3 people have paid you back ₹300.
Is this a return of your ₹500 on 1 May?
₹200 still pending."
              │
        User confirms
              │
              ▼
All linked automatically
₹200 debt still tracked
```

---

## Group Splits

- Create named groups (Goa Trip, Flatmates, Office etc.)
- Add expenses to group
- Split equally or custom amounts per person
- Track who paid what and who owes whom
- Net settlement (A owes B ₹500, B owes A ₹200 → A owes B ₹300 net)
- Settle up feature
- Group history
- Works completely offline

---

## Debt Dashboard

- Total others owe you
- Total you owe others
- Per-person breakdown
- Oldest unsettled debt flagged
- Per-debt reminder option

---

## Settlement

- Full settlement → debt cleared, transaction linked
- Partial settlement → remaining balance tracked
- Settlement via any payment method
- Auto-detected from SMS if paid digitally
# 10 · Transaction Linking

## Link Types

| Link Type | Example |
|---|---|
| Refund → original | Amazon refund → original purchase |
| Repayment → debt | Rahul pays back → your original payment |
| Return → split | Group member pays share → group expense |
| Related (manual) | Two transactions user marks as related |
| CC expense → bill | Credit card spend → bill payment migration |

---

## Linking Rules

- One transaction can link to multiple others
- Partial linking supported (₹200 of ₹500 linked, ₹300 remains)
- Remaining unlinked amount tracked separately
- Visual chain showing all linked transactions
- Circular link prevention built in

---

## Visual Chain Example

```
20 May: Paid ₹500 for dinner (original)
    └── Linked to:
        ├── 22 May: Rahul paid ₹125 (partial return)
        ├── 23 May: Priya paid ₹125 (partial return)
        └── 24 May: Amit paid ₹125 (partial return)
            Remaining: ₹125 still pending from Sanjay
```
# 11 · Recurring Transaction Intelligence

## Auto-Detection

- Same merchant + similar amount + regular interval → flagged
- User confirms once → tracked forever
- Interval detected: weekly / monthly / quarterly / annual
- Amount tolerance: ±10% (for variable utility bills)

---

## Recurring Management

- Upcoming payments calendar view
- "You have 3 bills due this week — ₹3,298 total"
- Alert if recurring payment missed (didn't arrive on expected date)
- Alert if recurring amount changed significantly
- Pause / resume tracking per item
- Annual cost summary per subscription
- Total subscription spend per month

---

## Recurring Categories

| Category | Examples |
|---|---|
| Subscriptions | Netflix, Spotify, Hotstar, YouTube |
| Utility Bills | Electricity, water, gas, broadband |
| Insurance | LIC, health, vehicle |
| Loan EMIs | Home, car, personal loan |
| SIP Investments | Mutual fund SIPs |
| Rent | Monthly rent payments |
| Memberships | Gym, club |

---

## Missed Payment Detection

```dart
// Check if expected recurring payment arrived
if (today > expectedDate && !paymentDetected) {
  notify("Your Netflix payment was expected today but not detected.
          Check if it went through.");
}
```
# 12 · Budget System

## Budget Setup

- Monthly budget per category
- Overall monthly spending limit
- Account-specific budgets
- Period budgets (trips, events)
- Wants vs needs split

---

## Smart Budget Suggestions

- Suggests budgets based on last 3 months average
- "Based on your history, ₹8,000 for food is realistic"
- User can accept, modify, or ignore

---

## Budget Alerts

| Threshold | Alert Type |
|---|---|
| 50% used | Optional gentle notification |
| 70% used | Nudge |
| 90% used | Warning |
| 100% exceeded | Alert with breakdown |

---

## Budget Intelligence

- Rollover option — unused budget carries to next month
- Projected end-of-month spend at current pace
- "At this rate you'll exceed food budget by ₹2,000"
- "Cutting Swiggy by ₹1,000 keeps you within budget"
# 13 · Dashboard

## Main Dashboard

- Total spent this month
- Total income this month
- Net this month (income − expenses)
- Savings rate %
- Month-on-month comparison
- Overall budget status bar
- Cash in hand
- Upcoming bills this week
- Pending debts summary (owe / owed)
- Recent transactions (last 5)
- Quick add button

---

## Spending Breakdown

- Category-wise donut chart
- Category-wise bar chart
- Day-wise spending graph (this month)
- Week-wise trend line
- Top 5 merchants this month
- Top 5 merchants this year

---

## Account Overview

- All accounts listed with current balance
- Total assets
- Total liabilities
- Net worth
- Account-wise spending this month

---

## Per-Account Dashboard

- Account name, bank, masked number
- Current balance (and last updated time)
- This month: spent / received / net
- Transaction list filtered to this account
- Edit balance option
- Credit limit & utilisation (for credit cards)

---

## Net Worth Dashboard

- All assets with values
- All liabilities with values
- Net worth = assets − liabilities
- Net worth trend chart over time
- Month-on-month change
# 14 · Insights & Reports

## Weekly Summary (every Sunday)

- Total spent this week
- Top 3 categories
- Biggest single transaction
- Comparison to last week
- One actionable insight

---

## Monthly Report

- Full month breakdown
- Category-wise analysis + charts
- Day-wise spending chart
- Merchant ranking (top spends)
- Account-wise summary
- Income vs expense
- Savings vs goal
- Recurring vs non-recurring split
- Comparison to last month and 3-month average

---

## Year in Review

- Total spent / income / net savings
- Top merchant of the year
- Top category
- Biggest single transaction
- Most active spending month
- Quietest month
- Most used payment method
- Total transaction count
- Subscription total for the year
- Month-wise chart
- Fun stats: "You ordered food 47 times this year"

---

## Custom Reports

- Any date range
- Any filter combination
- Category / account / person breakdown
- Export as PDF or CSV

---

## Merchant Intelligence

- Per-merchant: total, count, average, trend
- Category-wise merchant ranking
- Merchant spending trend over time
# 15 · Notifications & Alerts

## Transaction Alerts

- New transaction → silent processing (no noise)
- Needs review → gentle notification
- Unusual amount (3x+ normal) → verify prompt
- Duplicate transaction detected → alert
- Suspicious timing (late night) → optional alert

---

## Budget Alerts

- 70% / 90% / 100% thresholds
- Category-specific and overall

---

## Bill & Recurring Alerts

- Upcoming bill in X days (user configurable)
- Recurring payment missed
- Recurring amount changed significantly
- Credit card due date reminder
- Minimum due reminder
- EMI due reminder

---

## Debt Alerts

- Debt overdue reminder
- Settlement detected automatically from SMS
- New debt recorded

---

## Insight Notifications

- Weekly summary ready
- Monthly report ready
- "You've spent ₹10,000 on food — highest ever this month"
- "You saved ₹5,000 more than last month"

---

## Notification Settings

- Per alert type on/off
- Quiet hours (e.g. 10PM–8AM)
- Alert thresholds configurable
# 16 · Location Features

## Strategy — Passive Only

No background GPS. Passive approach only:

- Last known location attached if updated within X minutes of transaction
- Network-based coarse location (city / area level)
- Time-correlated location

This avoids:
- Play Store background location rejection
- Battery drain
- User distrust

---

## Location on Transaction

- City / area level shown
- User can manually override location
- User can manually add location if missing
- Location shown on map in transaction detail
- Filter by location (future)

---

## Why Not Background GPS

| Approach | Play Store Safe | Battery | Accuracy |
|---|---|---|---|
| Passive (last known) | ✅ | ✅ | City level |
| Background GPS | ❌ High rejection risk | ❌ Drains | GPS level |
| No location | ✅ | ✅ | None |

City-level accuracy is enough for expense analytics ("You spend most at Koramangala on weekends").
# 17 · Security & Privacy

## App Security

- PIN lock
- Biometric lock (fingerprint / face)
- Auto-lock after configurable time
- Screenshot prevention (toggle)
- Sensitive amounts hidden by default — tap to reveal

---

## Data Privacy

- All SMS processed on-device only
- No raw SMS ever transmitted anywhere
- Only parsed fields sent to AI (never full SMS, opt-in only)
- AI calls are always opt-in, never forced
- User controls exactly what leaves device

---

## SMS Monitoring Controls

- User-editable whitelist of monitored sender IDs
- User can exclude specific senders
- User can pause SMS monitoring entirely
- Monitoring status always visible in app

---

## Encryption

- Database encrypted at rest (SQLCipher via Drift)
- Sensitive preferences in flutter_secure_storage
- Backup encrypted before any export
# 18 · Backup & Restore

## Local Backup

- Full encrypted backup to device storage
- One-tap backup
- Auto backup option (daily / weekly)
- Includes all transactions, profiles, settings, learned patterns

---

## Cloud Backup (Optional — User Controlled)

- Google Drive backup
- Fully encrypted before upload
- User holds encryption key
- Never readable by anyone but the user

---

## Restore

- From local backup
- From Google Drive
- Partial restore (transactions only / settings only)
- Merge restore (combine with existing data)

---

## Data Ownership

- Full data export (JSON / CSV)
- User owns everything completely
- One-tap delete all data
# 19 · Goals & Savings

## Savings Goals

Each goal has:
- Name
- Target amount
- Target date (optional)
- Linked account (optional)
- Monthly contribution amount
- Progress bar
- Estimated completion date

---

## Smart Suggestions

- "You have ₹12,000 unallocated this month — add to a goal?"
- "Cutting Swiggy by ₹2,000/month gets you to your laptop goal 2 months faster"
- Suggests realistic targets based on spending history
# 20 · Tags System

## Tagging

- User creates any tags freely (#vacation, #business, #reimbursable)
- Multiple tags per transaction
- Tags auto-suggested based on merchant / category / pattern
- Tag colours — user picks
- Bulk tagging supported

---

## Tag Intelligence

- Filter by tag
- Combine tags in filters (AND / OR)
- Tag-wise spending reports
- Recurring tag patterns auto-detected

---

## Common Tag Examples

`#business` `#reimbursable` `#shared` `#vacation` `#gift`
`#emergency` `#medical` `#investment` `#online` `#cash`
# 21 · Gmail Integration (Phase 2)

## Overview

Gmail parsing is Phase 2 — Android SMS must be stable first.

---

## How It Works

```
User taps "Connect Gmail"
        │
        ▼
Google OAuth consent screen
        │
        ▼
App gets read-only Gmail access
        │
        ▼
App reads only bank / transaction emails
        │
        ▼
Same parsing pipeline as SMS
        │
        ▼
Duplicate detection
(same transaction in SMS and email → one record)
```

---

## What It Parses

- All major bank email formats
- Credit card statements
- Investment confirmations
- Bill payment emails
- Wallet transaction emails

---

## Why Phase 2

- Requires Google OAuth consent screen setup
- Privacy policy required
- Google review process (can take weeks)
- Core SMS feature must be solid first

---

## iOS Value

On iOS (Phase 3), Gmail parsing becomes the primary data source since SMS is impossible. Same Dart parsing code works — only ingestion layer changes.
# 22 · Onboarding

## First Run Flow

```
Step 1: Welcome screen
        Explain what the app does in plain language

Step 2: SMS permission request
        Clear explanation: "To auto-read your bank messages"
        Show what data stays on device

Step 3: Scan historical SMS
        "Scanning last 6 months of bank messages..."
        Progress shown

Step 4: Accounts discovered
        "Found 3 bank accounts and 1 credit card"
        User confirms each

Step 5: Set opening balances
        For each account — user enters current balance
        App calculates from there

Step 6: Category preferences
        Suggested budgets based on history
        User accepts or modifies

Step 7: App lock setup
        PIN or biometric

Step 8: Ready!
        Dashboard shows with months of history pre-populated
```

---

## Permission Handling

- Clear explanation before every permission request
- Graceful degradation if denied
- Never force — always explain benefit
- Permissions can be granted later from Settings
# 23 · UI & UX

## Design Language

- **Framework:** Flutter (Dart)
- **Feel:** iOS-quality spring animations on Android
- **Surfaces:** Bottom sheets only — no dialogs
- **Cards:** Expandable with spring physics
- **Menus:** Floating, gesture-dismissable
- **Feedback:** Snackbars only (no toasts)
- **Transitions:** No full-screen jumps
- **Haptics:** On key actions (tap, confirm, error)
- **Loading:** Skeleton loaders — no spinners
- **Latency:** Zero — local DB is instant

---

## Animation Style

```dart
// Every interactive element uses spring physics
spring(
  dampingRatio: Spring.DampingRatioMediumBouncy,
  stiffness: Spring.StiffnessLow,
)

// Cards scale on tap
// Lists animate on entry
// Bottom sheets follow finger physics
// Numbers count up on dashboard load
```

---

## Colour & Theme

- Dark mode — proper deep black (AMOLED)
- Light mode
- Follows system default
- Debit = red consistently throughout
- Credit = green consistently throughout
- Accent colour — user can pick (optional)

---

## Typography

- Clear hierarchy: amount large, merchant medium, metadata small
- Monospace for amounts (alignment in lists)
- No more than 3 font sizes per screen

---

## Key Screens

| Screen | Purpose |
|---|---|
| Dashboard | Overview, quick stats |
| Transaction List | Full history with filters |
| Transaction Detail | Full info + edit |
| Account Detail | Per-account view |
| Add Transaction | Quick and full modes |
| Debt & Splits | Who owes what |
| Reports | Charts and insights |
| Budgets | Per-category limits |
| Goals | Savings progress |
| Settings | All configuration |

---

## Accessibility

- Dynamic text size support
- High contrast mode
- Screen reader support
- No colour-only information

---

## Home Screen Widget

- Current month spending vs budget
- Cash in hand
- Quick add button
- Recent 3 transactions

---

## Quick Add

- One tap from anywhere
- Amount + category + account
- Pre-fills common patterns
- No need to open full app
# 24 · Settings & Configuration

## Account Settings
- Add / edit / delete accounts
- Reorder accounts
- Hide from dashboard
- Mark as closed

## Category Settings
- Add custom categories and subcategories
- Edit / delete / reorder
- Merge categories
- Category icons and colours

## Notification Settings
- Per alert type on/off
- Quiet hours
- Alert thresholds

## Privacy Settings
- App lock type (PIN / biometric)
- Auto-lock timer
- Screenshot prevention
- Blur sensitive amounts
- Pause SMS monitoring

## Parser Settings
- Monitored sender IDs (whitelist — user editable)
- AI fallback on/off
- Confidence threshold for auto-saving
- Unknown format handling preference

## Display Settings
- Default currency
- Date format
- Week start day (Sunday / Monday)
- Financial year start (April / January)
- Number format (Indian: 1,00,000 / International: 100,000)
- Default list grouping
# 25 · iOS Companion App (Phase 3)

## What It Can Do

- Manual transaction entry
- Gmail OAuth parsing (same Dart code, different data source)
- Full dashboard and reports
- All filters and search
- Budgets and goals
- Debt tracking (manual)
- Tags system
- Backup and restore

## What It Cannot Do

- SMS parsing — impossible on iOS (Apple policy, permanent)

## How It Gets Data

- Gmail OAuth as primary source
- Manual entry
- Optional sync from Android via encrypted backup export

## Why Flutter Makes This Easy

- All UI code is identical — zero rewrite
- All business logic (parsing, matching, scoring) is identical
- Only data ingestion layer changes (Gmail instead of SMS)
- Same Drift database, same Riverpod providers, same screens
# 26 · Future Roadmap

## Near Term (Post-MVP)

- Gmail integration (Phase 2)
- iOS companion app (Phase 3)
- Home screen widget
- Quick add from notification shade

## Medium Term

- International bank support (UAE, UK, US, Singapore)
- Multi-currency with exchange rates
- Investment tracking (stocks, mutual funds, crypto)
- On-device ML model (TensorFlow Lite) for improved parsing
- Web dashboard (view only)

## Long Term

- Family / shared finance mode
- Accountant / CA export portal
- Voice quick-add
- Wear OS widget
- Community pattern library (anonymised — never personal data)
- WhatsApp integration (forward bank messages to parse)

---

## What Will NOT Be Added

- Cloud-first architecture (privacy principle)
- Ads (user trust principle)
- Selling data (privacy principle)
- Mandatory AI dependency (offline-first principle)
