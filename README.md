# SolydFlow Flutter SDK

<p align="center">
  <img src="https://via.placeholder.com/150x50?text=SolydFlow" alt="SolydFlow Logo" width="200"/>
  <br>
  <b>The Solid Revenue Infrastructure for African Mobile Apps.</b>
  <br>
  <br>
  <a href="https://solydflow.com"><img src="https://img.shields.io/badge/Status-Private%20Alpha-orange" alt="Status"></a>
  <a href="https://solydflow.com"><img src="https://img.shields.io/badge/Platform-Flutter-blue" alt="Platform"></a>
  <a href="https://solydflow.com"><img src="https://img.shields.io/badge/License-Commercial-green" alt="License"></a>
</p>

---

## What is SolydFlow?

SolydFlow is a hybrid revenue SDK designed specifically for the African market. It unifies **StoreKit (iOS)**, **Google Play Billing (Android)**, and **Local Mobile Money (Paystack/Flutterwave)** into a single, offline-first API.

Stop writing 3 different payment integrations. Write one line of code, and let SolydFlow handle the fragmentation, entitlement caching, and network retries.

## Key Features

- 🌍 **Hybrid Payments:** Seamlessly switch between Apple IAP and Mobile Money (M-Pesa, USSD, Bank Transfer) based on user location.
- ⚡ **Offline-First Entitlements:** Entitlements are cached and encrypted locally. Your app knows if a user is "Pro" even without internet access.
- 🧠 **Smart Retry Engine:** Built for unstable networks. If a webhook is delayed, SolydFlow polls and queues receipts to ensure no purchase is lost.
- 🔄 **Web-to-App Sync:** Instantly unlock features in the app after a web-based payment.

---

## Getting Started

### 1. Installation

Add the package to your `pubspec.yaml`:

```yaml
dependencies:
  solydflow_flutter: ^0.1.0
```

### 2. Initialization

Initialize the SDK in your `main.dart`. You only need your public API Key.

```dart
import 'package:solydflow_flutter/solydflow.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  
  // Initialize SolydFlow
  await SolydFlow.configure(
    apiKey: "sf_live_YOUR_API_KEY", 
    appUserID: "user_123" // Optional: Link to your internal Auth ID
  );
  
  runApp(MyApp());
}
```

### 3. Making a Purchase

You don't need to check `Platform.isIOS`. SolydFlow intelligently determines whether to show the native Apple/Google sheet or a localized Web Payment flow.

```dart
Future<void> buySubscription() async {
  try {
    // Attempt purchase for the 'premium_monthly' package.
    CustomerInfo customerInfo = await SolydFlow.purchasePackage("premium_monthly");
    
    if (customerInfo.entitlements.all["pro"]!.isActive) {
      print("Success! User is now Pro.");
      // Unlock features...
    }
  } on SolydFlowException catch (e) {
    // Standardized error messages (e.g., "Insufficient Funds") 
    // regardless of the underlying payment provider.
    print("Purchase failed: ${e.message}");
  }
}
```

### 4. Checking Status (Offline Supported)

This check is instant and synchronous after the first load, powered by our on-device cache.

```dart
void checkStatus() async {
  CustomerInfo info = await SolydFlow.getCustomerInfo();
  
  if (info.entitlements.all['pro']?.isActive ?? false) {
    // Grant access to Pro features
  } else {
    // Lock features
  }
}
```

---

## Roadmap

- [x] **Core Entitlement Engine** (Offline Caching)
- [x] **Paystack Integration** (Nigeria/Ghana/Kenya)
- [ ] **StoreKit 2 Support** (iOS)
- [ ] **Google Billing 6 Support** (Android)
- [ ] **Smart Paywalls** (Remote Config)

## Security & Compliance

SolydFlow does not process card data directly. We delegate sensitive transactions to certified PCI-DSS providers (Apple, Google, Paystack, Flutterwave, etc). All local data is encrypted using AES-256.

---

<p align="center">
  Built with you in mind.
  <br>
  <a href="https://solydflow.com">Get Early Access</a>
</p>