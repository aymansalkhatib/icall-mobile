<div align="center">
   <img width=100% src="https://capsule-render.vercel.app/api?type=waving&height=100&color=0:667eea,100:764ba2&section=header" alt=""/>
</div>

<div align=center>
    <img src="assets/images/logo-dark.png" alt="iCall" width="200" height="200">
</div>

<br>
<br>
<br>

<div align="center">

[![Flutter](https://img.shields.io/badge/Flutter-%5E3.8.0-02569B?logo=flutter&logoColor=white)](https://flutter.dev/)
[![Dart](https://img.shields.io/badge/Dart-%5E3.8.0-0175C2?logo=dart&logoColor=white)](https://dart.dev/)
[![BLoC](https://img.shields.io/badge/BLoC-Pattern-2ecc71)](https://bloclibrary.dev/)
[![Clean Architecture](https://img.shields.io/badge/Clean-Architecture-1abc9c)](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)

[![flutter_bloc](https://img.shields.io/badge/flutter__bloc-%5E9.1.1-02569B?)](https://pub.dev/packages/flutter_bloc)
[![dio](https://img.shields.io/badge/dio-%5E5.9.0-009688?)](https://pub.dev/packages/dio)
[![hive_ce](https://img.shields.io/badge/hive__ce-%5E2.11.3-FFCA28?)](https://pub.dev/packages/hive_ce)
[![get_it](https://img.shields.io/badge/get__it-%5E8.2.0-43B02A?)](https://pub.dev/packages/get_it)
[![freezed](https://img.shields.io/badge/freezed-%5E3.2.3-7B1FA2?)](https://pub.dev/packages/freezed)
[![connectivity_plus](https://img.shields.io/badge/connectivity__plus-%5E6.1.5-FF5722?)](https://pub.dev/packages/connectivity_plus)

</div>

<br>

# iCall

**iCall** is a mobile application designed for shop owners who transfer mobile credit and recharge games. The app simplifies the process by automating USSD code execution, eliminating the need to memorize and manually enter complex codes.

The application automatically detects the carrier type from the phone number, selects the appropriate SIM card, and executes the transfer operation. It also includes a wallet system for game recharging.

<br>

## Features

- **Automated Credit Transfer** - Enter the phone number and amount, the app handles the rest
- **Smart SIM Detection** - Automatically identifies carrier type (MTN/Syriatel) from phone number
- **Multi-SIM Support** - Works with dual SIM devices, selecting the correct SIM for each operation
- **Game Recharging Wallet** - Built-in wallet system for game top-ups
- **Transaction History** - Complete log of all transfer operations
- **Wallet Management** - Track balance and recharge operations
- **Network Status Verification** - Checks network and SIM status before executing operations

<br>

## Technical Highlights

### Custom USSD Plugin

One of the main challenges was handling USSD operations across different Android versions. Google has changed the USSD API implementation multiple times, adding more restrictions with each version.

The solution was building a custom Flutter plugin in Kotlin that:

- Detects Android version and selects the appropriate method
- Verifies network and SIM status before execution
- Supports multiple SIM cards
- Uses Accessibility Service to read operation results from system dialogs

<br>

## Tech Stack

| Category             | Technology                 |
| -------------------- | -------------------------- |
| Framework            | Flutter                    |
| State Management     | BLoC / Cubit               |
| Dependency Injection | Get It                     |
| Local Storage        | Hive CE                    |
| Network              | Dio                        |
| Code Generation      | Freezed, JSON Serializable |
| Native Plugin        | Kotlin                     |

<br>

## Screenshots

### Light Theme

<div align="center" style="margin-bottom:4px;">
  <img src="assets/screenshots/2.1.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/3.1.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/4.1.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/5.1.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/6.1.webp" width="19%" />
</div>

<div align="center" style="margin-bottom:4px;">
  <img src="assets/screenshots/7.1.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/8.1.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/9.1.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/10.1.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/11.1.webp" width="19%" />
</div>

<div align="center" style="margin-bottom:4px;">
  <img src="assets/screenshots/12.1.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/13.1.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/14.1.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/15.1.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/16.1.webp" width="19%" />
</div>

<div align="center">
  <img src="assets/screenshots/17.1.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/18.1.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/19.1.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/1.1.webp" width="19%" />
</div>

<br>

### Dark Theme

<div align="center" style="margin-bottom:4px;">
  <img src="assets/screenshots/2.2.webp" width="19%" style="margin-right:0.3%"/>
  <img src="assets/screenshots/3.2.webp" width="19%" style="margin-right:0.3%"/>
  <img src="assets/screenshots/4.2.webp" width="19%" style="margin-right:0.3%"/>
  <img src="assets/screenshots/5.2.webp" width="19%" style="margin-right:0.3%"/>
  <img src="assets/screenshots/6.2.webp" width="19%" />
</div>

<div align="center" style="margin-bottom:4px;">
  <img src="assets/screenshots/7.2.webp" width="19%"  style="margin-right:0.3%" />
  <img src="assets/screenshots/8.2.webp" width="19%"  style="margin-right:0.3%" />
  <img src="assets/screenshots/9.2.webp" width="19%"  style="margin-right:0.3%" />
  <img src="assets/screenshots/10.2.webp" width="19%" style="margin-right:0.3%"  />
  <img src="assets/screenshots/11.2.webp" width="19%" />
</div>

<div align="center" style="margin-bottom:4px;">
  <img src="assets/screenshots/12.2.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/13.2.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/14.2.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/15.2.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/16.2.webp" width="19%" />
</div>

<div align="center">
  <img src="assets/screenshots/17.2.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/18.2.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/19.2.webp" width="19%" style="margin-right:0.3%" />
  <img src="assets/screenshots/1.2.webp" width="19%" />
</div>
