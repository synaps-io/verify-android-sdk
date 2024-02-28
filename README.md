# Synaps Verify

> Synaps Android Verify SDK

[![](https://jitpack.io/v/synaps-hub/verify-android.svg)](https://jitpack.io/#synaps-hub/verify-android)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

**Synaps is an all-in-one compliance platform**. It offers a simple, fast and secure way to meet compliance requirements at scale.

[Visit Synaps.io](https://synaps.io) | [Read the Synaps documentation](https://docs.synaps.io)

![enter image description here](https://storage.googleapis.com/synaps-docs-media/synaps-verify.png)

## Gradle Dependency
To get a Git project into your build:

**Step 1.** Add the JitPack repository to your build.gradle(.kts) file or settings.gradle(.kts)

```kotlin
maven {
	url = uri("https://jitpack.io")
}
```


**Step 2.** Add the dependency

```kotlin
dependencies {
...
    implementation("com.github.synaps-io:verify-android-sdk:0.1.0")
}
```


---

## The Basics

By default, when opening the Verify sdk screen, [Camera](https://developer.android.com/reference/android/Manifest.permission#CAMERA) and [NFC](https://developer.android.com/reference/android/Manifest.permission#NFC) permissions will be requested if they have not already been granted. You are free to make the request before opening Verify to integrate it into your user experience.
More information about requesting permission in the [Android documentation](https://developer.android.com/training/permissions/requesting)

## Compose
Verify SDK provide a simple way to use our service with Android Compose. Just call the VerifyView composable function. A session ID is required.

### Kotlin  Usage:
```kotlin
VerifyView(
	sessionId = "...",
	lang = VerifyLang.FRENCH,
	tier = "...",
	onReady = {
		...
	},
	onFinish = {
		...
	}
)
```

### Attributes list

| Attribute name | Attribute type | Default | Required | Description |
| ------------------ |----------------| ------- | -------- | ----------------------------------------------------------------------------- |
| `sessionId`  | `string`       | `''`  | Yes  | Session can be referred as a customer verification session. [More info](https://help.synaps.io/manager-1/sessions) |
| `lang` | `VerifyLang`   | `VerifyLang.ENGLISH` | No | Event listener called on every open/close action |
| `tier` | `string`       | `null` | No | Tier is a simply way to divide your workflow into small pieces. It is very useful when you offer different features based on the verification level of your customer. [More info](https://docs.synaps.io/manager-1/apps/individual/tiers) |
| `onReady` | `() => Void`   | `null` | No  | Event listener called when the page is fully loaded |
| `onFinished` | `() => Void`   | `null` | No  | Event listener called when the user finished verification |

## Activity (view legacy)

If you're less familiar with compose, you can use our service by calling an Android Activity.

**First,** you need to add `VerifyActivity` in your Android Manifest:

```xml
<activity
	android:name="io.synaps.sdk.activity.VerifyActivity"
	android:screenOrientation="portrait"
	android:exported="true"
	tools:ignore="LockedOrientationActivity"/>
```

**Second,** To launch the Verify activity, you need to create an [`intent`](https://developer.android.com/reference/android/content/Intent) and transmit a list of attibutes. Some parameters are optional :

### Attributes list

| Attribute name | Attribute type | Default | Required | Description |
| ------------------ |----------------| ------- | -------- | ----------------------------------------------------------------------------- |
| `sessionId`  | `string`       | `''`  | Yes  | Session can be referred as a customer verification session. [More info](https://help.synaps.io/manager-1/sessions) |
| `lang` | `VerifyLang`   | `VerifyLang.ENGLISH` | No | Event listener called on every open/close action |
| `tier` | `string`       | `null` | No | Tier is a simply way to divide your workflow into small pieces. It is very useful when you offer different features based on the verification level of your customer. [More info](https://docs.synaps.io/manager-1/apps/individual/tiers) |


### Kotlin  Usage:
```kotlin
val intent = Intent(this, VerifyActivity::class.java)
intent.putExtra("sessionId", "...")
intent.putExtra("lang", VerifyLang.ENGLISH)
intent.putExtra("tier", "...")
startActivity(intent)
```
### Android Jetpack Navigation
You can also use [Android Jetpack Navigation](https://developer.android.com/guide/navigation/use-graph/pass-data) instead of basic Intent. Please add in your navigation graph:
```xml
<argument
	android:name="sessionId"
	app:argType="string" />
<argument
	android:name="lang"
	android:defaultValue="ENGLISH"
	app:argType="io.synaps.sdk.core.VerifyLang" />
<argument
	android:name="tier"
	app:argType="string"/>
```

## License

Apache 2.0 © [Synaps](https://www.synaps.io/)