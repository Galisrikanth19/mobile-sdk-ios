# Known Issues

## Table of Contents
- [CoreNFC Issues with Xcode 12](#corenfc-issues-with-xcode-12)
- [Custom Theme Issues](#custom-theme-issues)
  - [Language Localization Issues](#language-localization-issues)
    - [Localizable.strings File](#localizable.strings-file)
    - [Language Changes at Runtime](#language-changes-at-runtime)
- [User Was Not Asked for Face Capturing](#user-was-not-asked-for-face-capturing)
- [Country Missing from the Country List](#country-missing-from-the-country-list)

## CoreNFC Issues with Xcode 12 and Xcode 12.1

Building an application including NetverifyFace framework with a simulator target using Xcode12 might result in the following error:

_Building for iOS Simulator, but linking in dylib built for iOS, file '.../mobile-sdk-ios/frameworks/NetverifyFace.framework/NetverifyFace' for architecture arm64_

This seems to be an issues with the linking against `Core NFC`. Apple is aware of this and [will resolve it in Xcode 12.2.](https://developer.apple.com/documentation/xcode-release-notes/xcode-12_2-beta-release-notes#Simulator) The problem seems to occur on iOS 14 simulators only. Any version lower than that should not have any issues.

## Library Not Loaded: Image Not Found

Building an application using Objective C might result in the following error:

_Termination Description: DYLD, Library not loaded: @rpath/libswiftCore.dylib | Referenced from: {PATH} | Reason: image not found_

Please make sure `ALWAYS_EMBED_SWIFT_STANDARD_LIBRARIES` is set to `Yes` in the __Build Options.__

## Custom Theme Issues

### Language Localization Issues

Please make sure to select your project in the project and targets list in the __Project Navigator,__ navigate to the __Info__ tab. In the __Localizations__ section, make sure that _"Use Base Internationalization"_ is checked. Otherwise, the system will fall back on the default localization.

To select a different language use the “+” button in the __Localizations__ section. This will let you choose a new language you want to support from a dropdown list. Please refer to the [full list of languages supported by Jumio](../docs/README.md#language-localization) for more details. Adding a new language from the list will generate files under a new language project folder named `[new language].lproj` For example, if Japanese support is added, a folder named `ja.lproj` will be created.

#### Localizable.strings File

The `Localizable-<YOUR_PRODUCT>.strings` file makes it possible to easily add translations as key-value pairs. Adapt the values to your required language as needed and add it to your app or framework project. Again, please make sure to mark the project as _Localizable._ After SDK updates, make sure to check whether the content of this localization file is up to date, as individual strings may have changed.

__Note:__ Refer to the transition guides for possible updates.

#### Language Changes at Runtime
Runtime language changes _within_ the SDK or separate language support (meaning the SDK language differs from the overall device language) is not possible. This goes against Apple's basic iOS user model for switching languages in the Settings app.

## User Was Not Asked for Face Capturing

If there is an issue with the user journey skipping over the face capturing and not asking the user to take a selfie, please make sure the parameter `config.enableIdentityVerification` is set to `true`. If it is set to `false` the 3D-Liveness step won't be performed. 

## Country Missing from the Country List

Countries with documents that need barcode functionality (e.g. US and Canadian driver licenses) might not be available if the necessary frameworks are missing:

`MicroBlink.framework`  
`NetverifyBarcode.framework`

Frameworks and instructions on how to integrate them [can be found here.](../docs/README.md#integration)

__Note:__ Version numbers may vary.
