---
layout: post
title: "Unleashing the Power of Android 11 adb Wireless Debug: From Wired to Wireless, Explore a Freer Debugging Experience!"
date: 2022-02-22 15:16:12 +0800
image: cover/android-adb-wirless-share-1.png
tags: [Android,adb]
categories: Android教學
excerpt: "Want to unlock the true potential of your Android 11? Explore wireless adb wireless debug! From wired to wireless, make your debugging experience freer and more convenient."
---

<div class="c-border-main-title-2">Introduction</div>
In response to project needs,
we are constantly looking for new ways to use ADB Debug,<br>
and recently discovered a method introduced by Android.<br>
Here, we share our research findings on adb wireless debug.


<div align="start" class="table_container">
  If you are not yet familiar with using ADB wifi for debugging, we have previously shared related insights, which you might find useful.<br>
  <a href="{{site.baseurl}}/2022/02/15/android-adb-wifi-note/">
    <img src="/images/others/adb_wifi.png" alt="Cover" width="30%"/>
  </a>
  <a href="{{site.baseurl}}/2022/02/15/android-adb-wifi-note/">Embrace Wireless: Guide to Using adb to Wirelessly Connect to Android Devices!</a>
</div><br>

<div class="c-border-main-title-2">Research on the New adb Wireless Debug in Android 11
  <a style ="color:white;" herf="https://developer.android.com/studio/command-line/adb#connect-to-a-device-over-wi-fi-android-11+">Refer to this article</a>
</div>

<div class="c-border-content-title-4"><b style="color:red;">Android 11</b> and later versions have added the wireless debug feature. The environment requirements are:</div>
  1. Android 11 or above<br>
  2. The connected computer or the self-built environment must have a platform tool SDK version greater than 30.0.0 (<b style="color:red;">adb version</b> can check the current SDK version)<br>
  3. Must be on the same local network<br>
<div class="c-border-content-title-4">Actual Operation <b>**(No USB Cable Needed)**</b></div>
  1. On the Android phone, go to settings, enable developer mode, and find 'Wireless debugging' to turn it on<br>
  2. Enter the Wireless debugging submenu<br>
  3. Click pair device with code to view the IP, port, and pairing code.<br>
  4. On the terminal, enter the command <b>adb pair `ipaddr`:`port`</b> to pair<br>
  5. When <b>Enter pairing code:</b> appears, enter the pairing code seen in step 3.<br>

<div class="c-border-content-title-4">Features</div>
  1. In Wireless debugging, you can see the devices that have been paired and manage or delete them<br>
  2. It will automatically reconnect the next time you restart<br>
  3. Pairing can be done without a USB cable<br>
  4. Unlike the old version (below Android 10) adb connecting to wifi, the old version requires manual reconnection each time<br>
    - Brief description of the adb connection method below Android 10 **(Requires USB Cable)**<br>
    Switch tcpip port: <b style="color:red;">adb tcpip <port></b><br>
    <b style="color:red;">adb connect <ip>:<port></b> Enter the IP and the port just used to connect<br>

<div class="c-border-main-title-2">Other Notes</div>

  - In Android 11 AOSP, check the WirelessDebuggingFragment in the developer options to see how the pairing code is generated<br>
    -> AOSP folder path:<br>
     `/Android/11/packages/apps/Settings/com/android/settings/development/WirelessDebuggingFragment.java`<br>

    Here, it looks like a special key is needed to generate it,<br>
    Not sure if the `application layer` can achieve this <br>
    Those interested in researching can check it out in AOSP<br>

  - Some Android 11 devices do not support it, meaning manufacturers might lock this feature, such as LG phones<br>
     -> You won't see the wireless debugging option in the developer options,<br>
     So whether it is supported or not depends on whether the OTA provided by the manufacturer includes it<br>

  - `adb connect <ip>:<port> connection` <br>
     -> Use this to test if you can connect to the IP:port displayed on the wireless debug page<b style="color:red;">IP:port</b><br>
     But this command is used to connect to the tcpip IP (requires a cable)<br>
     Another use is to restore a device that has been paired under wireless debugging<br>
     So it can only be used to manually restore the connection<br>
