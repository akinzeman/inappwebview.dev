---
sidebar_position: 2
date: 2022-12-10 12:00:00
---

# Debugging WebViews

## Debugging info

WebViews and some controllers have debugging info enabled by default when your flutter app is running in debug mode (based on the `kDebugMode` flutter constant value).

You can change the `DebugLoggingSettings` properties of the following static properties:
- `InAppWebView` and `HeadlessInAppWebView` through `WebView.debugLoggingSettings`;
- `ChromeSafariBrowser.debugLoggingSettings`;
- `InAppBrowser.debugLoggingSettings` for InAppBrowser specific events, otherwise `WebView.debugLoggingSettings`;
- `FindInteractionController.debugLoggingSettings`;
- `PullToRefreshController.debugLoggingSettings`;
- `WebAuthenticationSession.debugLoggingSettings`.

For example, to disable all WebView debugging info:
```dart
WebView.debugLoggingSettings.enabled = false;
```

Instead, to exclude debugging info of specific WebView events:
```dart
WebView.debugLoggingSettings.excludeFilter.add(
  RegExp(r"onScrollChanged")
);
```

`WebView.debugLoggingSettings` comes with a default list of excluded events
and `maxLogMessageLength` value:
```dart
DebugLoggingSettings(
  maxLogMessageLength: 1000,
  excludeFilter: [
    RegExp(r"onScrollChanged"),
    RegExp(r"onOverScrolled"),
    RegExp(r"onReceivedIcon")
]);
```

## Inspect Android WebViews
On Android, in order to enable/disable debugging WebViews using `chrome://inspect/#devices` on Chrome, you should use the `AndroidInAppWebViewController.setWebContentsDebuggingEnabled(bool debuggingEnabled)` static method.

For example, you could call it inside the main function:
```dart
Future main() async {
  WidgetsFlutterBinding.ensureInitialized();

  if (!kIsWeb && defaultTargetPlatform == TargetPlatform.android) {
    await InAppWebViewController.setWebContentsDebuggingEnabled(true);
  }

  runApp(new MyApp());
}
```

## Inspect iOS and MacOS WebViews
On iOS and MacOS, debugging WebViews on Safari through developer tools is always enabled. There isn't a way to enable or disable it.
