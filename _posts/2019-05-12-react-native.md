---
layout: post
title: Notes taken while setting up a modern React Native environemnt
tags:
  - react native
  - firebase
  - boilerplate
  - notes
---

# Hello World

I started by creating a GitHub repository, and initializing a React Native project using the React Native CLI.

```react-native init RNSprout```

The full instructions can be foune [here](https://facebook.github.io/react-native/docs/getting-started)

# Firebase

Next, I installed [react-native-firebase](https://rnfirebase.io/docs/v5.x.x/installation/initial-setup) v5.

At the time of this writing, v6 looks close to being ready, but not quite there.

### Steps

I followed the steps listed [here](https://rnfirebase.io/docs/v5.x.x/installation/ios)

1. `npm install --save react-native-firebase`
2. Add `GoogleService-Info.plist`
  - I already had a Firebase project setup, but there are instructions [here](https://firebase.google.com/docs/ios/setup#add_firebase_to_your_app).
  - Once I had the plist file, I moved it to `/src/ios/FirebaseDev/GoogleService-Info.plist`
3. Install the SDK using Cocoapods
  - `cd ios`
  - `pod update`
  - `pod init`
  - I added the following pods to my Podfile:
    ```
    pod 'Firebase/Core', '~> 5.20.1'
    pod 'Firebase/Auth', '~> 5.20.1'
    pod 'Firebase/Database', '~> 5.20.1'
    pod 'Firebase/Messaging', '~> 5.20.1'
    pod 'GoogleUtilities', '= 5.8.0'
    ```

    The reason for adding GoogleUtilities is because I was getting an "Undefined symbols" error.

    [This GitHub issue](https://github.com/invertase/react-native-firebase/issues/1975) helped me figure this out.
4. Use Firebase in App.js to be sure everything is setup correctly.
  ```
  import firebase from 'react-native-firebase'

  [...]

  componentDidMount = () => {
    firebase.auth().onAuthStateChanged(firebaseUser => {
      console.log('firebaseUser', firebaseUser)
    })
  }
  ```

# Victory Charts

[victory-native](https://github.com/FormidableLabs/victory-native) is a charting library for React Native. I followed the instructions in the readme to get a chart to load.

### Troubleshooting

After installing react-native-svg, my app was crashing with the error, "Invariant Violation: requireNative Component: "RNSVGPath" was not found in the UIManager"

This issue was solved by explicitely declaring some pod dependencies as detailed in [this article](https://sandstorm.de/de/blog/post/react-native-managing-native-dependencies-using-xcode-and-cocoapods.html).

--

As a general thing to do if there are errors, be sure that victory-native and react-native-svg are installed properly.
- Check package.json to verify both libraries are installed.
- Take a look at [manual linking](https://github.com/react-native-community/react-native-svg#manually) steps and double check that the lib is properly linked.
