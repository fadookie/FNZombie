# FNZombie

Custom implementation of zombie objects for debugging bad access crashes. This library is an alternative, more customizable implementation of Apple's built-in `NSZombie`.

Please see:
* https://funcorp.dev/blog/stopping_nszombie_invasion
* https://developer.apple.com/documentation/xcode/investigating-memory-access-crashes
* https://developer.apple.com/documentation/xcode/investigating-crashes-for-zombie-objects

How to call from a Swift app:

Import the SwiftPM package:
```swift
let package = Package(
    // ...
    dependencies: [
        .package(url: "https://github.com/funcorp/FNZombie.git", .branch("master"))
    ],
    targets: [
        .target(
            name: "<MY TARGET NAME>",
            dependencies: [
                .product(name: "FNZombie", package: "FNZombie")
            ]
        )
    ]
)
```

Import the package somewhere early on in your init and initialize the zombie service, ex:

```swift
import FNZombie

@main
class MyAppDelegate: iOSHostApplication {

    override func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

        FNZombieService.sharedInstance().enable(withBufferSize: 128) // This buffer size can be whatever you want

    }
}
```

Now, run your app and check the logs for any zombie object calls when the app crashes.