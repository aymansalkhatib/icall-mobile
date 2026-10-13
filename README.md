<div align="center">

<img src="assets/images/logo-dark.png" alt="iCall logo" width="160" height="160">

**Android app for mobile shop owners that sends credit and cash transfers through automated USSD codes and sells game top-ups from a prepaid wallet.** The shop owner enters the customer's phone number and the amount, and iCall identifies the carrier, uses the matching SIM card, runs the USSD request and records the result, so nobody has to memorize or type USSD codes. The same app sells in-game packages for mobile games, paid from a games wallet funded through Syriatel Cash or MTN Cash. The interface is in Arabic, laid out right to left, with light and dark themes.

[![Flutter](https://img.shields.io/badge/Flutter-02569B?logo=flutter&logoColor=white)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-0175C2?logo=dart&logoColor=white)](https://dart.dev)
[![Kotlin](https://img.shields.io/badge/Native%20plugin-Kotlin-7F52FF?logo=kotlin&logoColor=white)](https://kotlinlang.org)
[![Android](https://img.shields.io/badge/Android-up%20to%2012-3DDC84?logo=android&logoColor=white)](https://www.android.com)
[![State: Cubit](https://img.shields.io/badge/State-Cubit%20%28flutter__bloc%29-0098D8?logo=flutter&logoColor=white)](https://pub.dev/packages/flutter_bloc)
[![License: All rights reserved](https://img.shields.io/badge/License-All%20rights%20reserved-red.svg)](LICENSE)

</div>

## Highlights

- **Transfers without typing codes.** The Balance screen takes the customer's phone number, the credit amount and the price charged, and one Transfer button runs the operation. Two shortcuts under the form open My balance and the price tiers.
- **Carrier and SIM selection.** The carrier, Syriatel or MTN, is identified from the phone number. On a dual-SIM phone the SIM of that carrier is used for the request, and the network and SIM status are checked before anything is sent.
- **Results read from the USSD reply.** Android shows the carrier's reply to a USSD request in a system dialog. iCall uses an Android Accessibility Service to read that dialog, so each operation is saved with the result the carrier returned.
- **Transfer records.** A searchable list of credit and cash transfers, filtered by service provider and status. Each record shows the provider, phone number, amount, price, date and time, a success or failure badge and the message returned, for example a network error.
- **Game top-ups.** A searchable catalog of mobile games with category filters (All, Popular, New, Cards, Action, Adventure). A game page describes the game and lists its packages with prices. Before charging, a confirmation dialog shows the chosen package and the amount due, asks for the player ID and warns that a top-up is final and cannot be refunded.
- **Games wallet.** The wallet screen shows the current balance, the wallet status, the date of the last deposit and the most recent operations. Deposits come in through Syriatel Cash or MTN Cash and are marked pending until they are confirmed, and game top-ups are paid from the wallet balance.
- **Operation history.** A full list of wallet operations with search and filters by provider and status. Each entry carries a transaction ID, a signed amount, a timestamp and a status. Game purchases also show the player ID and the provider that paid for them.
- **Phone number sign-in.** Users sign in with a phone number, accept the terms and policies, and confirm with a six-digit code sent over WhatsApp.
- **Account activation and review.** A new account is activated with a one-time fee paid through Sham Cash or Syriatel Cash. The app shows the destination account, and the user enters the transaction number and the amount sent. The account then waits for approval by an administrator, and the waiting screen can check the account status or contact support.
- **Guided permissions.** A setup screen requests the phone permission, used to place the USSD calls, and the Accessibility Service, used to follow running operations. It explains why each one is needed and asks the user to restart the app after enabling the service.
- **Android version check.** On devices running a version newer than Android 12, the app opens an "unsupported Android version" screen with buttons to contact support or close the app.
- **Settings.** Profile name and photo, app language, appearance (theme and colors), a secret key and a distributor code that are renewed periodically, problem reports, app version information and sign out.

## Architecture

iCall is a Flutter application organized along clean architecture lines. Screens are driven by Cubits from `flutter_bloc`, dependencies are registered in a `get_it` service locator, and data models are generated with `freezed` and `json_serializable`. Dio sends HTTP requests to the remote API, and Hive CE keeps data on the device. All USSD work happens in a custom Flutter plugin written in Kotlin, which the Dart code reaches over a platform channel.

```mermaid
flowchart TB
    subgraph App["Flutter app (Dart)"]
        direction TB
        UI["Screens and widgets"] --> Cubits["Cubits<br/>flutter_bloc"]
        Cubits --> Data["Domain and data layers<br/>wired with get_it"]
    end

    Data -->|"HTTP · Dio"| API["Remote API"]
    Data --> Hive[("Hive CE<br/>local storage")]
    Data -->|"platform channel"| Plugin

    subgraph Native["Android (Kotlin)"]
        Plugin["USSD plugin"] -->|"network and SIM checks,<br/>USSD request"| Tel["Android telephony"]
        Tel --> Dialog["USSD reply dialog"]
        A11y["Accessibility Service"] -->|"reads"| Dialog
        A11y -->|"operation result"| Plugin
    end
```

### USSD plugin

Google has changed how Android handles USSD several times and added restrictions with each release, so one method does not work on every version. The plugin therefore:

- detects the Android version and uses the USSD method that works on it,
- checks the network and SIM status before it sends a request,
- supports phones with more than one SIM card,
- reads the operation result from the system dialog through the Accessibility Service.

### Transfer flow

1. The user enters the phone number and the amount on the Balance screen.
2. The app identifies the carrier from the phone number and picks the SIM card that belongs to it.
3. The plugin checks the network and SIM status and sends the USSD request with the method that fits the Android version.
4. Android shows the carrier's reply in a system dialog.
5. The Accessibility Service reads the reply, and the operation is stored in the transfer records with its result.

## Tech stack

| Area                 | Technology                     | Used for                                 |
| -------------------- | ------------------------------ | ---------------------------------------- |
| Framework            | Flutter, Dart                  | User interface and application logic     |
| State management     | `flutter_bloc` (Cubit)         | Screen state                             |
| Dependency injection | `get_it`                       | Service locator for cubits and services  |
| Networking           | `dio`                          | HTTP requests to the remote API          |
| Local storage        | `hive_ce`                      | On-device storage and caching            |
| Code generation      | `freezed`, `json_serializable` | Immutable data models and JSON mapping   |
| Connectivity         | `connectivity_plus`            | Checking whether the device is online    |
| Native code          | Kotlin                         | Custom USSD plugin                       |
| Android services     | Accessibility Service          | Reading USSD replies from system dialogs |

## Requirements

Running iCall needs:

- an Android phone with Android 12 or lower,
- a Syriatel or MTN SIM card, or one of each on a dual-SIM phone,
- the phone permission, requested by the app's setup screen,
- the iCall Accessibility Service turned on in Android settings, followed by a restart of the app,
- an internet connection to sign in,
- an account that has been activated and approved by an administrator.

## Repository contents

```text
icall-mobile/
├── assets/
│   ├── images/
│   │   └── logo-dark.png   # App logo
│   └── screenshots/        # 19 app screens: N.1.webp light theme, N.2.webp dark theme
├── LICENSE                 # Proprietary license, all rights reserved
└── README.md
```

## Screenshots

<p align="center">
  <img src="assets/screenshots/2.1.webp" width="19%" alt="Welcome">
  <img src="assets/screenshots/3.1.webp" width="19%" alt="Bills and game top-ups">
  <img src="assets/screenshots/4.1.webp" width="19%" alt="Wallet and operations">
  <img src="assets/screenshots/5.1.webp" width="19%" alt="Phone number sign-in">
  <img src="assets/screenshots/6.1.webp" width="19%" alt="WhatsApp verification code">
</p>

<p align="center">
  <img src="assets/screenshots/7.1.webp" width="19%" alt="Account activation">
  <img src="assets/screenshots/8.1.webp" width="19%" alt="Account pending approval">
  <img src="assets/screenshots/9.1.webp" width="19%" alt="Permissions setup">
  <img src="assets/screenshots/10.1.webp" width="19%" alt="Balance transfer">
  <img src="assets/screenshots/11.1.webp" width="19%" alt="Transfer records">
</p>

<p align="center">
  <img src="assets/screenshots/12.1.webp" width="19%" alt="Game catalog">
  <img src="assets/screenshots/13.1.webp" width="19%" alt="Game packages">
  <img src="assets/screenshots/14.1.webp" width="19%" alt="Top-up confirmation">
  <img src="assets/screenshots/15.1.webp" width="19%" alt="Games wallet">
  <img src="assets/screenshots/16.1.webp" width="19%" alt="Operation history">
</p>

<p align="center">
  <img src="assets/screenshots/17.1.webp" width="19%" alt="Settings">
  <img src="assets/screenshots/18.1.webp" width="19%" alt="Profile">
  <img src="assets/screenshots/19.1.webp" width="19%" alt="Report a problem">
  <img src="assets/screenshots/1.1.webp" width="19%" alt="Unsupported Android version">
</p>

<br>

<p align="center">
  <img src="assets/screenshots/2.2.webp" width="19%" alt="Welcome">
  <img src="assets/screenshots/3.2.webp" width="19%" alt="Bills and game top-ups">
  <img src="assets/screenshots/4.2.webp" width="19%" alt="Wallet and operations">
  <img src="assets/screenshots/5.2.webp" width="19%" alt="Phone number sign-in">
  <img src="assets/screenshots/6.2.webp" width="19%" alt="WhatsApp verification code">
</p>

<p align="center">
  <img src="assets/screenshots/7.2.webp" width="19%" alt="Account activation">
  <img src="assets/screenshots/8.2.webp" width="19%" alt="Account pending approval">
  <img src="assets/screenshots/9.2.webp" width="19%" alt="Permissions setup">
  <img src="assets/screenshots/10.2.webp" width="19%" alt="Balance transfer">
  <img src="assets/screenshots/11.2.webp" width="19%" alt="Transfer records">
</p>

<p align="center">
  <img src="assets/screenshots/12.2.webp" width="19%" alt="Game catalog">
  <img src="assets/screenshots/13.2.webp" width="19%" alt="Game packages">
  <img src="assets/screenshots/14.2.webp" width="19%" alt="Top-up confirmation">
  <img src="assets/screenshots/15.2.webp" width="19%" alt="Games wallet">
  <img src="assets/screenshots/16.2.webp" width="19%" alt="Operation history">
</p>

<p align="center">
  <img src="assets/screenshots/17.2.webp" width="19%" alt="Settings">
  <img src="assets/screenshots/18.2.webp" width="19%" alt="Profile">
  <img src="assets/screenshots/19.2.webp" width="19%" alt="Report a problem">
  <img src="assets/screenshots/1.2.webp" width="19%" alt="Unsupported Android version">
</p>

## License

Copyright (c) 2025-2026 Ayman Al-Khatib. All rights reserved.

This is proprietary software. The repository is public for viewing only, and no rights to use, copy, modify or distribute it are granted. See [LICENSE](LICENSE) for the full terms.
