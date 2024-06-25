---
layout: post
title: "Android 15 Beta Version Released! How Should Developers Respond to New Version Restrictions?"
date: 2024-05-16 13:59:07 +0800
image: cover/android-version-adaptation-15.png
tags: [Android]
categories: SDK升級
excerpt: "This article introduces the development challenges and solutions brought by the Android 15 version upgrade."
---

<div class="c-border-content-title-4">Introduction</div>

Android 15 has recently released its Beta version<br>
The new version is called `VanillaIceCream`<br>
<img src="/images/android15/002.png" width="80%"><br>

Here is the roadmap:<br>
<img src="/images/android15/001.png" width="80%"><br>
For detailed schedule, please refer to: <a href="https://developer.android.com/about/versions/15/overview?hl=zh-cn">Official Website</a>
<br>
<div class="c-border-content-title-4">All Applications on Android 15 Will Be Affected</div>

<div class="c-border-content-title-1">Package Behavior Modifications</div>
* In the class `ApplicationInfo`<br>
There used to be a flag named `FLAG_STOPPED`<br>
Its behavior indicated whether the app was in a `stopped state`<br>
Previously, the `stopped state` was set when the user force-stopped the application, and the application would remain in this state.<br>

* On the other hand, to let the system determine that the app has left the `stopped state`<br>
Previously, this could be done by `the user clicking to open the app` or some interactive windows opening the app to change the state<br>

  &#x21AA; Here is the official `original text`:<br>
  `directly launching the app or indirectly interacting with the app.`<br>
  `(through the sharesheet or a widget, selecting the app as live wallpaper, etc.)`<br>
  It indicates that in Android 14, the state could be changed by `the user clicking to open the app` and `some interactive windows indirectly opening the app`<br>
  Examples given include `sharesheet`, `widget`, `selecting as wallpaper`, etc.<br>

* Compared to the latest `Android 15` system, the `FLAG_STOPPED` has the following modifications and extensions.<br>
  1. Now, when the system determines `FLAG_STOPPED`, it will stop all `pending intents` operations<br>
  2. When the system detects `leaving` FLAG_STOPPED, it will send an `ACTION_BOOT_COMPLETED` broadcast<br>
     The official statement indicates that developers can use this broadcast to restore pending intents.<br><br>

  &#x21AA; Here is the official `original text` explaining the adjustments to `FLAG_STOPPED` in version 15:<br>
`Apps should only be removed from the stopped state through direct or indirect user action.`<br>
  This means that only direct or indirect actions will change the system's determination of the `stopped state`<br>

* From the above, it can be understood that the determination of this state has become stricter<br>

  &#x21AA; However, the original text above does not clearly define what `indirect user action` means.<br>
  Refer to future updates if encountered

* Additionally, the `Application StartInfo.wasForceStopped()` method can be used to determine the above state.

<div class="c-border-content-title-1">Support for 16 KB Page Sizes</div>

* The official strategy for RAM usage has been optimized<br>
Previously, Android only supported `4 KB memory page sizes`<br>
Now it has been increased to `16 KB page sizes`<br>
It is expected to be available on Google Play next year (it is already available on version 15, but it is uncertain whether apps will be required to compile with 16KB at that time &#x2728;)<br>	
The main purpose of increasing the page size is to optimize `RAM-intensive tasks`, making them more efficient<br>
However, this is an optimization for software<br>
It is mainly aimed at ensuring that apps can run smoothly and be compatible when future `manufacturers develop larger and more efficient RAM`<br>
 ✅  `Page size` is used to represent the unit of operation in memory, usually a power of 2, such as 2KB, 4KB, 8KB, etc.<br>

* Impact on `app developers`:<br>
  &#x21AA; If you use Ndk for compilation, or directly or indirectly use SDK for compilation, you may need to reconfigure the app for `16 KB page sizes`<br><br>
<img src="/images/android15/004.png" width="80%"><br><br>
  &#x21AA; Projects should not be significantly affected in the `short term`<br>
  &#x21AA; In the `future`, when officially launched on Google Play, you may encounter issues related to `16 KB page sizes`<br><br>
<img src="/images/android15/006.png" width="80%"><br>

 ✅   If you encounter `compatibility issues`, you can refer to the <a href="https://developer.android.com/guide/practices/page-sizes#build">official website</a> to adjust the compilation method.

* To determine if you will be affected, you can use `APK Analyzer` in `Android Studio` to check if native code is used in the compilation (or use `disassembly` to check)<br>
  &#x21AA; Drag the apk into Android Studio<br>
  &#x21AA; Check if any `.so` files (shared object files) appear in the lib folder<br>
If they appear, it means your app `has` native code compiled<br>
If `no lib or .so` appears, it means it is likely `not` native code<br>
<img src="/images/android15/005.png" width="80%"><br>

✅ The official statement indicates that if `native code` is used, you need to `rebuild the app` for 16KB devices.

Here are some preliminary judgment references:<br>
↪ Using any C/C++ (native) code, usually involving NDK or JNI.<br>
↪ Using any third-party libraries that involve native libraries.<br>
↪ Using any third-party builders that use native libraries.<br>

* The image below shows the performance improvement of the system after optimizing the `16 KB page sizes` feature<br>
<img src="/images/android15/007.png" width="80%">

<div class="c-border-content-title-1">New Private Space Feature</div>
This feature adds a hidden space for apps or a similar app lock<br>
A password is required to open and access the app<br>
However, the `official currently indicates there is a bug`<br>
<img src="/images/android15/008.png" width="80%"><br>
Testing on the emulator also couldn't find this feature<br>
However, the official mentioned that this feature will restrict app visibility<br>
`Because apps in the private space have restricted visibility,`<br>
You can check later if there are any issues when putting your app into it<br>
For example: It is uncertain if features like `QueryAllPackagesPermission` will behave as expected<br>
You can test it later<br>

<div class="c-border-content-title-1">Minimum Target SDK Changed to 24</div>
* Similar to the previous Android 14 version, this is to prevent exploiting old APIs<br>
In `Android 15`, the target `needs` to be greater than 24 to install<br>
If not, it will show `INSTALL_FAILED_DEPRECATED_SDK_VERSION`<br>
You can use the following command to install non-compliant apks<br><br>

  ```
  adb install --bypass-low-target-sdk-block FILENAME.apk
  ```

#### UI/UX Adjustments
<div class="c-border-content-title-1">UI/UX Adjustments</div>
* Removed `predictive back animations` from developer options<br>
And allowed developers to set it in the app<br><br>

  ```
  <application
      ...
      android:enableOnBackInvokedCallback="true"
      ... >
  ...
  </application>
  ```
  ✅ Check what `predictive back animations` are: <a href="https://developer.android.com/guide/navigation/custom-back/predictive-back-gesture">Reference</a>



<div class="c-border-content-title-1">Deprecations</div>
Routine deprecation of some APIs:
<a href="https://developer.android.com/about/versions/15/deprecations">Reference API deprecations</a><br><br>

<div class="c-border-content-title-4">Impacts on Apps Targeting Android 15</div>
<div class="c-border-content-title-1">Adjustments Related to Foreground Services</div>
* Prohibited from starting the following foreground services during the BOOT_COMPLETED broadcast
   - dataSync
   - camera
   - mediaPlayback
   - phoneCall
   - mediaProjection
   - microphone
 
&#x21AA; Forcing a start will throw `ForegroundServiceStartNotAllowedException`

* Ultimate judgment for `dataSync`<br>
   &#x21AA; The dataSync service can now only run for 6 hours within a 24-hour period. When the time is up, the system will call `Service.onTimeout(int, int)`<br>
   At this point, you must call `Service.stopSelf()` within a few seconds of receiving the timeout<br>
   &#x21AA; If `stopSelf` is not called when the time is up, an error will occur<br>
  `A foreground service of ##fgs_type did not stop within its timeout: ##component_name.`<br>
   &#x21AA; When the system calls `Service.onTimeout(int, int)`, the service is no longer considered a foreground service<br>
   &#x21AA; In the current beta2 version, the thrown error is considered an `ANR`, but it will be changed to an `exception` in subsequent versions.<br>
   &#x21AA; The above restrictions apply to all dataSync services collectively. For example, if 4 hours of dataSync have already been executed within 24 hours, other dataSync services can only run for 2 more hours
   &#x21AA; Alternatively, it is recommended to migrate to other methods: <a href="https://developer.android.com/about/versions/15/changes/datasync-migration">Alternative Solutions</a>


* New foreground service type: <a href="https://developer.android.com/about/versions/15/behavior-changes-15#mediaprocessing-fgs-type">mediaProcessing</a><br>
  &#x21AA; This type follows the same rules as dataSync above<br>
  &#x21AA; The `mediaProcessing` service can now only run for 6 hours within a 24-hour period. When the time is up, the system will call `Service.onTimeout(int, int)`<br>
  At this point, you must call `Service.stopSelf()` within a few seconds of receiving the timeout<br>
  &#x21AA; When the system calls `Service.onTimeout(int, int)`, the service is no longer considered a foreground service<br>
  &#x21AA; The official alternative solutions differ from those for dataSync: <a href="https://developer.android.com/develop/background-work/services/foreground-services#purpose-built-apis">Alternative Solutions</a>

* To reset the timer for the above types, the documentation mentions<br>
The timer is reset when the user brings the app to the foreground<br>   
<img src="/images/android15/004.png" width="80%"><br>

* You can also try using special cases <br>
   - <a href="https://developer.android.com/about/versions/14/changes/fgs-types-required?authuser=7&hl=zh-tw#special-use">Click here to view</a>
   - However, the official review and approval are required for listing
<div class="c-border-content-title-1">Restrictions on Starting Foreground Services via SYSTEM_ALERT_WINDOW</div>
* Previously, starting a foreground service via SYSTEM_ALERT_WINDOW would work even if the app was in the background<br>
  Now an additional step is required: an overlay window must be started via `TYPE_APPLICATION_OVERLAY` and it must be visible<br>
↪ If the new requirement is not met, `ForegroundServiceStartNotAllowedException` will be thrown.<br>

<div class="c-border-content-title-1">Do Not Disturb Mode Behavior Changes</div>

* Apps targeting SDK Android 15 and above no longer support setting the `Do Not Disturb (DND)` `global state` or `policy`<br>
↪ Affects apps that previously set this through `setInterruptionFilter(INTERRUPTION_FILTER_ALL)`<br>

<div class="c-border-content-title-1">Changes for OpenJDK 17</div>

* Android 15 has adjusted the API usage for this, so take note if you are using it<br>
An exception is now thrown when using the following functions to format strings<br>
  - String.format(String, Object[])
  - String.format(Locale, String, Object[])
  - Formatter.format(String, Object[])
  - Formatter.format(Locale, String, Object[])

↪ When misusing `$0`, it will throw
  `IllegalFormatArgumentIndexException`: Illegal format argument index = 0

  ```kotlin
  //work
  val formattedString = String.format("Name: %1$s, Age: %2$d", name, age);
  //exception
  val formattedString = String.format("Name: %0$s, Age: %1$d", name, age);
  ```

* Adjustments to the `Random` class for this <a herf="https://bugs.openjdk.org/browse/JDK-8301574">issue</a>
Now `Random.ints()` will not return the same value as `Random.nextInt()`
So now you `should not` expect the two to be `==`
  - Random.ints(long)
  - Random.ints(long, int, int)
  - Random.ints(int, int)
  - Random.ints()

<div class="c-border-content-title-1">Safer Background Activity Launch</div>
* Since Android 10, background activity launches have been restricted, and `Android 15 adds additional controls` to `prevent malicious background apps` from bringing other apps to the foreground.
* A new flag can now be set to prevent a background activity from opening another app's activity

  ```
  <application android:allowCrossUidActivitySwitchFromBelow="false" >
  ```

  The actual modification is: if the activity to be opened and the top app in the stack `do not match in UID`, it cannot be opened
  ↪ Used to prevent an app from opening another different app

* Other restrictions on background activity launches
   - Now `PendingIntent` defaults to `blocking background activity launches`

<div class="c-border-content-title-1">User Experience UI Improvements</div>
* This adjustment is aimed at improving user experience, briefly described here:
  - `Edge-to-edge enforcement`: Enforced edge-to-edge, on Android 15 the screen will be forced to inset from the edges
  However, it is mentioned that using `material 3` will not be affected, and testing shows that `apps using material 3` indeed have the same UI as before<br>
  <img src="/images/android15/011.png" width="80%"><br>
  Other UIs that may be affected should be noted<br>
  <img src="/images/android15/010.png" width="80%"><br><br>
  &#x21AA; The third image above shows adjustments made using windowInsets.getInsets or similar<br>
  Alternatively, you can use material 3<br>
  
  - `elegantTextHeight attribute defaults to true`: The elegantTextHeight attribute defaults to `true`<br>
  <img src="/images/android15/012.png" width="80%"><br>

  - `Stable configuration`: Adjustments to configuration-related behaviors, which may affect screen orientation and system bar size judgments<br>
	<a href="https://developer.android.com/about/versions/15/behavior-changes-15#stable-configuration">Click here to see</a>
  - `Locale-aware default line height for EditText`: Adjusts the height of EditText based on different languages<br>
  	<img src="/images/android15/013.png" width="80%"><br>
  	✅ Check if the changes are acceptable<br>
  To cancel, you can directly set the `useLocalePreferredLineHeightForMinimum` attribute to false
  - `TextView width changes for complex letter shapes`: Adjustments to the default text width assignment rules<br>
  Giving more space to complex letters<br>
  ✅ To `disable/enable`, directly set the attribute `setShiftDrawingOffsetForStartOverhang`<br>

* For other UI/UX details, refer to the more complete information here: <a href="https://developer.android.com/about/versions/15/behavior-changes-15#ux">Click here</a>

<div class="c-border-content-title-1">New Camera and Media Restrictions</div>
* To request `audio focus`, it must now be the `top app` or an `audio-related foreground service`, otherwise it will return AUDIOFOCUS_REQUEST_FAILED.
* Currently, the following are considered `audio-related foreground services`:
   - mediaPlayback
   - camera
   - microphone
   - phoneCall
   <br>
   ✅ Learn about audio focus <a href="https://developer.android.com/media/optimize/audio-focus">Click here to view</a><br>

<div class="c-border-content-title-1">Updated non-SDK restrictions</div>
* Regular version updates include: <a href="https://developer.android.com/guide/app-compatibility/restrictions-non-sdk-interfaces#list-names">Click here to view</a><br>
   ✅ `Non-SDK`: Java methods within the scope. These interfaces are internal implementation details of the SDK and may be modified at any time without notice to developers.
