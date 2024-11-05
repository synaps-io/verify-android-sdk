# Synaps Verify

> Synaps Android Verify SDK

[![](https://jitpack.io/v/synaps-hub/verify-android.svg)](https://jitpack.io/#synaps-hub/verify-android)
[![License][license-image]][license-url]

**Synaps is an all-in-one compliance platform**. It offers a simple, fast and secure way to meet compliance requirements at scale.

[Visit Synaps.io](https://synaps.io) | [Read the Synaps documentation](https://docs.synaps.io)

<div align=center>
	<img width="55%" src=media/synaps-verify.jpeg>
</div>

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
    implementation("com.github.synaps-io:verify-android-sdk:0.6.0")
}
```


---

## The Basics

By default, when opening the Verify sdk screen, [Camera](https://developer.android.com/reference/android/Manifest.permission#CAMERA) and [NFC](https://developer.android.com/reference/android/Manifest.permission#NFC) permissions will be requested if they have not already been granted. You are free to make the request before opening Verify to integrate it into your user experience.
More information about requesting permission in the [Android documentation](https://developer.android.com/training/permissions/requesting)

Please note that NFC may not be available on some devices. This will be particularly true for emulators. To check whether the device supports NFC, try to find the NfcAdapter. If it's null, then the device doesn't support NFC.
More information about NfcAdapter in the [Android documentation](https://developer.android.com/reference/android/nfc/NfcAdapter#getDefaultAdapter(android.content.Context))
```kotlin
val adapter: NfcAdapter? = NfcAdapter.getDefaultAdapter(context)
// OR
val manager: NfcManager = context.getSystemService(Context.NFC_SERVICE) as NfcManager
val adapter: NfcAdapter? = manager.getDefaultAdapter()
```

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

## Fragment (view legacy)

If you want more flexibility in using the power of fragments, you can create an instance of `VerifyFragment with the parameters you want to integrate. Once created, simply add the fragment instance to your navigation stack.

### Kotlin  Usage:
```kotlin
val fragment = VerifyFragment.newInstance(
	sessionId = binding.editText.text.toString(),
	lang = VerifyLang.FRENCH,
	tier = "...",
	delegate = object : VerifyFragmentDelegate {
		override fun onReady() {
			...
		}

		override fun onFinish() {
			...
		}
	}
)

requireActivity().supportFragmentManager
	.beginTransaction()
	.replace(R.id.container, fragment)
	.commitNow()
```

### Attributes list

| Attribute name | Attribute type | Default | Required | Description |
| ------------------ |----------------| ------- | -------- | ----------------------------------------------------------------------------- |
| `sessionId`  | `string`       | `''`  | Yes  | Session can be referred as a customer verification session. [More info](https://help.synaps.io/manager-1/sessions) |
| `lang` | `VerifyLang`   | `VerifyLang.ENGLISH` | No | Event listener called on every open/close action |
| `tier` | `string`       | `null` | No | Tier is a simply way to divide your workflow into small pieces. It is very useful when you offer different features based on the verification level of your customer. [More info](https://docs.synaps.io/manager-1/apps/individual/tiers) |
| `delegate` | `VerifyFragmentDelegate`   | `null` | No  | An object to pass callbacks to the fragment, such as `onReady` or `onFinish  |

If you want to take advantage of the NFC feature, it's important to override the [`onNewIntent`](https://developer.android.com/reference/android/app/Activity#onNewIntent(android.content.Intent)) method of the activity that hosts the fragment and call `onNewIntent` of `VerifyFragment` to transmit the intent containing the nfc packets to the SDK.
### Kotlin sample:
```kotlin
class SampleActivity : Activity() {
    override fun onNewIntent(intent: Intent) {
        super.onNewIntent(intent)
        supportFragmentManager.fragments.forEach { fragment: Fragment ->
            if (fragment is VerifyFragment && fragment.isVisible) {
                fragment.onNewIntent(intent)
            }
        }
    }
}
```


## Common

It is possible to display a specific log level in Logcat. By default, logs are disabled with `LogType.NONE`.
To enable logs :
```kotlin
Verify.logType = LogType.DEBUG
```
### LogType list :
- `LogType.NONE`
- `LogType.WARNING`
- `LogType.ERROR`
- `LogType.DEBUG`

## License

BSD 3-Clause © [Synaps](https://www.synaps.io/)

[license-image]: https://img.shields.io/github/license/synaps-io/verify-ios-sdk?color=blue
[license-url]: LICENSE