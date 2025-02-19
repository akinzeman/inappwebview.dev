---
sidebar_position: 8
date: 2022-12-10 12:00:00
---

# Web Storage Manager

The `WebStorageManager` class implements a singleton object (shared instance) which manages the web storage used by `WebView` instances.

On Android, it is implemented using the [WebStorage](https://developer.android.com/reference/android/webkit/WebStorage.html) class.
On iOS and MacOS, it is implemented using [WKWebsiteDataStore.default()](https://developer.apple.com/documentation/webkit/wkwebsitedatastore) instance.

:::info Note for iOS
  Available from iOS 9.0+.
:::

## Basic Usage

On Android, the `WebStorageManager` class is used to manage the JavaScript storage APIs provided by the WebView.
It manages the Application Cache API, the Web SQL Database API and the HTML5 Web Storage API.

On iOS and MacOS, the `WebStorageManager` class represents various types of data that a website might make use of.
This includes cookies, disk and memory caches, and persistent data such as WebSQL, IndexedDB databases, and local storage.

Example:
```dart
// get the WebStorageManager instance
WebStorageManager webStorageManager = WebStorageManager.instance();

if (!kIsWeb) {
  if (defaultTargetPlatform == TargetPlatform.android) {
    // if current platform is Android, delete all data.
    await webStorageManager.deleteAllData();
  } else if (defaultTargetPlatform == TargetPlatform.iOS ||
      defaultTargetPlatform == TargetPlatform.macOS) {
    // if current platform is iOS or macOS, delete all data for "flutter.dev".
    var records = await webStorageManager.fetchDataRecords(
        dataTypes: WebsiteDataType.values);
    var recordsToDelete = <WebsiteDataRecord>[];
    for (var record in records) {
      if (record.displayName == 'flutter.dev') {
        recordsToDelete.add(record);
      }
    }
    await webStorageManager.removeDataFor(
        dataTypes: WebsiteDataType.values, dataRecords: recordsToDelete);
  }
}
```
