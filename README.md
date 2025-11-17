# FlutterUPI Plugin

A flutter plugin to invoke UPI apps on the phone for Android and get the transaction information in response. This plugin supports only Android as of now.

## Getting Started

Simply import the plugin

```
import 'package:flutter_upi/flutter_upi.dart';
```

And then use the `initiateTransaction` method as shown in the code below.

```
String response = await FlutterUpi.initiateTransaction(
    app: FlutterUpiApps.PayTM,
    pa: "receiver@upi",
    pn: "Receiver Name",
    tr: "UniqueTransactionId",
    tn: "This is a transaction Note",
    am: "5.01",
    mc: "YourMerchantId", // optional
    cu: "INR",
    url: "https://www.google.com",
);

print(response);

```

The `response` is a `String` that contains all the relevant information. Here is how the String looks like.

```
txnId=PTM2008fadf6e7242a4a86d72daef6efa66&responseCode=0&ApprovalRefNo=913338799016&Status=SUCCESS&txnRef=TR1234
```
Please note that some parameters in the response can be undefined when using different apps.

### Parsing the Response
You can write your own logic to parse the response string or you can use the `FlutterUpiResponse` class to create a `Map` out of it.
```
FlutterUpiResponse flutterUpiResponse = FlutterUpiResponse(response);
print(flutterUpiResponse.txnId); // prints transaction id
print(flutterUpiResponse.txnRef); //prints transaction ref 
print(flutterUpiResponse.Status); //prints transaction status
print(flutterUpiResponse.approvalRefNo); //prints approval reference number
print(flutterUpiResponse.responseCode); //prints the response code
```

## Supported Apps and Platforms
As of now the plugin only supports Android. Since I am not an iOS developer, I have only been able to write the code for Android. If you are interested, feel free to get in touch or create PR if you can do this for iOS as well.

The plugins supports three apps as of now which I have tested this plugin with. You can use the predefined constants in the `FlutterUpiApps` class and pass it to the `app` named argument in the `initiateTransaction` method.

# flutter_upi

![Pub Version](https://img.shields.io/pub/v/flutter_upi.svg)
![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)
![GitHub stars](https://img.shields.io/github/stars/rajeevsingh3108/UPI-Flow?style=social)

A Flutter plugin to invoke UPI apps on the device and receive transaction responses.

Version: 0.0.3  •  Author: Rajeev Singh

Repository: https://github.com/rajeevsingh3108/UPI-Flow

---

<!-- Demo GIF (if available) -->
![Demo](demo.gif)

## Overview

`flutter_upi` provides a small, focused API to launch installed UPI apps (Google Pay, PhonePe, PayTM, BHIM, etc.) with payment parameters and receive a UPI response string. Use the `FlutterUpiResponse` helper to parse that string into fields.

> Note: Android support is primary and tested; iOS code is present but behaviour may vary depending on installed apps and their URL schemes. Test on real devices.

## Features

- Initiate UPI payments from Flutter
- Receive and parse UPI response string
- Constants for common UPI app package IDs

## Installation

Add the plugin to your `pubspec.yaml` (local path while developing, or replace with version when published):

```yaml
dependencies:
  flutter_upi:
    path: .
```

Install:

```bash
flutter pub get
```

## Usage (null-safety)

Import the package and call `FlutterUpi.initiateTransaction`. This example shows a small, safe usage pattern with error handling.

```dart
import 'package:flutter_upi/flutter_upi.dart';

Future<void> startUpiPayment() async {
  try {
    final response = await FlutterUpi.initiateTransaction(
      app: FlutterUpiApps.GooglePay,
      pa: 'merchant@upi',
      pn: 'Merchant Name',
      tr: 'TXN123456',
      tn: 'Payment for order #1234',
      am: '10.00',
      cu: 'INR',
      url: 'https://example.com/txninfo',
    );

    if (response == null || response.isEmpty) {
      print('No response or user cancelled');
      return;
    }

    final parsed = FlutterUpiResponse(response);
    print('Status: ${parsed.Status}');
    print('TxnId: ${parsed.txnId}');
  } catch (e) {
    print('UPI transaction failed: $e');
  }
}
```

### Parameters

- `app` (String, required): Target UPI app package id (use `FlutterUpiApps` constants).
- `pa` (String, required): Payee VPA (e.g. `merchant@upi`).
- `pn` (String, required): Payee name.
- `mc` (String, optional): Merchant code.
- `tr` (String, required): Transaction reference id.
- `tn` (String, required): Transaction note.
- `am` (String, required): Amount (string, e.g. `"10.00"`).
- `cu` (String, required): Currency (usually `"INR"`).
- `url` (String, optional): URL to pass to the UPI app.

### Response

The plugin returns a raw string from the UPI app. Use `FlutterUpiResponse` to parse it into fields like `txnId`, `responseCode`, `ApprovalRefNo`, `Status`, and `txnRef`.

Example response:

```
txnId=...&responseCode=...&ApprovalRefNo=...&Status=SUCCESS&txnRef=...
```

## Example App

See the `example/` folder for a working demo. To run it:

```bash
cd example
flutter run
```

## Supported Apps

Use the constants in `FlutterUpiApps` to target common apps such as:

- `FlutterUpiApps.GooglePay`
- `FlutterUpiApps.PayTM`
- `FlutterUpiApps.PhonePe`
- `FlutterUpiApps.BHIMUPI`
- `FlutterUpiApps.MiPay`
- `FlutterUpiApps.AmazonPay`
- `FlutterUpiApps.TrueCallerUPI`
- `FlutterUpiApps.MyAirtelUPI`

Not all apps expose identical response fields — handle missing/optional fields defensively.

## Error / Edge Cases

Common special responses you should handle in your app:

- `app_not_installed` — target app not installed
- `invalid_params` — invalid request parameters
- `user_canceled` — user cancelled the flow
- `null_response` — no data returned

## Platform Notes

### Android

- Primary target platform. Launching UPI intents is handled in `android/src/main/java/...`.

### iOS

- iOS files exist in `ios/` but UPI behavior varies by app and installed URL schemes. Test on real devices.

## Contributing

Contributions welcome — open issues or pull requests at the project repository. When contributing:

- Add or update tests where appropriate
- Run `flutter analyze` and `flutter test`
- Keep changes focused and documented

## License

See the `LICENSE` file at the project root for license terms.

## Changelog

See `CHANGELOG.md` for release notes and history.

---

I've added badges, improved examples to be null-safe, fixed formatting, and embedded the demo GIF if present. If you want I can also:

- Commit the README change to git with a message `docs: improve README (badges, example, demo)`
- Add pub.dev / GitHub Action badges that are linked to actual pipelines (requires repo setup)
- Shorten the README or add a Table of Contents







