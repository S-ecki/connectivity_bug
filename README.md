# connectivity_plus bug
This is a minimal reproducible example of an unexpected behaviour in the [connectivity_plus plugin](https://pub.dev/packages/connectivity_plus), a widely used Dart package to discover network connectivity.

### Problem Case
When listening to the `Connectivity().onConnectivityChanged` stream and inspecting the first emitted event, the behaviour is different when the event is expected to be `ConnectivityResult.none` compared to any other case.

Concretely, the stream does not emit any event when the app is started without any connection (expected `ConnectivityResult.none`), but it does emit an event when the app is started with any connection (eg wifi) and then the connection is turned off.

The problem only occurs for the very first event and only if it should be `ConnectivityResult.none`. It was only tested on Android for now, but with multiple API Levels.

### Steps to Reproduce

#### Happy path
1. Make sure you have a working internet connection
2. Run the app on an Android device or emulator
3. See `ConnectivityResult.wifi` or `ConnectivityResult.mobile` on the screen
4. Turn off the internet connection
5. See `ConnectivityResult.none` on the screen

#### Error path
1. Make sure you have no internet connection before (hot re-)starting the app
2. Run the app on an Android device or emulator
3. See `no stream event yet` 
4. Wait and see that there will never be an event
5. Turn on the internet connection
6. See correct `ConnectivityResult` again
