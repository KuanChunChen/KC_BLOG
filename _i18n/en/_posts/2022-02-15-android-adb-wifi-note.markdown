---
layout: post
title: "Embrace Wireless: A Guide to Using ADB for Wireless Connection to Android Devices!"
date: 2022-02-15 13:46:32 +0800
image: others/adb_wifi.png
tags: [Android,adb]
categories: Android教學
excerpt: "This article teaches you how to use ADB to connect to Android devices via Wi-Fi, freeing you from the hassle of cables and making development and testing easier."
---

<div class="c-border-main-title-2">Introduction</div>
This article primarily documents how to use the adb wifi CLI to connect to an Android phone.<br>
It is suitable for users who want to develop and debug in a wireless network environment.<br>
In the past, we often used the features of Android Studio to connect phones,
but using adb wifi is also a good method.<br>
Therefore, I specifically searched for related information and compiled it into notes,<br>
for my future reference and to hopefully help other users!<br>

<div class="table_container">
    <span style="text-align:center;">Additionally, Android 11 has introduced another method for testing or debugging:</span><br>
   <a href="{{site.baseurl}}/2022/02/22/android-adb-wifi-note-detail/">
     <img src="/images/others/adb_wifi.png" alt="Cover" width="30%"/>
   </a>
   <a href="{{site.baseurl}}/2022/02/22/android-adb-wifi-note-detail/">Unleash the Power of Android 11 ADB Wireless Debug: From Wired to Wireless, Explore a Freer Debugging Experience!</a>
</div><br>

<div class="c-border-main-title-2">Practical Steps</div><br>

1. Ensure the computer and Android phone are on the same local network.<br>
2. Connect the Android phone to the computer using a USB cable and enable developer mode.<br>
3. Use the following command to find the phone's IP address:<br>

```linux
adb shell ifconfig
```
4. You will find an IP similar to 192.168.xxx.xxx.<br>
<img src="/images/others/ipconfig.png"/><br>
5. Use the following command to switch to TCP/IP mode:<br>
```linux
adb tcpip <port>
```
-> You can specify the port yourself.<br>

6. Finally, use the command:<br>
```
adb connect 192.168.0.101:5555
```
(5555 is the port you set earlier)
At this point, you can control your phone using adb wifi.

Supplement:
Later, I encountered a method to connect to an Android TV,
but there was no USB slot available.
In this case, you can go to Settings -> Wi-Fi -> find the IP under the same network,
so you don't need to use commands.

<div class="c-border-content-title-4">Here's a Tip:</div>
Previously, I always used the adb wifi plugin downloaded from Android Studio to connect to the phone (the effect of this plugin is the same as above).<br>
Since the port set by the plugin is always the same,<br>
and others in the company also use the same port settings or the same plugin,<br>
if you don't change the port,<br>
and your IP happens to be assigned to a colleague's old IP,<br>
there is a chance that using the command on the same local network,<br>
you can install APKs on your phone through your IP and port.<br>
However, this happens occasionally.<br>
You can use scrcpy to see what others are doing XD (just kidding).<br><br>
So it's better to set different ports.

<div class="c-border-main-title-2">Other Notes</div>
- Additional details for adb connection below Android 10:
  - To connect using `adb connect`, **you need to connect the USB cable at least once** to set your TCP/IP port.
  After that, you can connect directly with the same IP and port without the cable.

  - Some so-called adb wifi plugins usually have a default port of 5555, so if the phone has already set the adb TCP/IP port,
  someone on the same network who knows your IP can easily try to connect and control the phone.
  (Tested here, if `adb tcpip` is not set, using the default 5555 will be refused)
  ![adb-connect.png](/images/others/adb-connect.png)
  - To disconnect, use `adb disconnect <ip>:<port>`.
  - It mainly operates under the adb server, using `adb kill-server` will also disconnect.
  - To find the phone's local IP, you can use `adb shell ifconfig`, which will show something similar:
  ```
  wlan0   Link encap:Ethernet  HWaddr F0:XX:B7:XX:XX:97
         inet addr:192.168.X01.XXX  Bcast:192.XXX.X01.255  Mask:255.255.255.0
         inet6 addr: fe80::fxxxx:x2xx:fee1:7d97/64 Scope: Link
         UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
         RX packets:543 errors:0 dropped:0 overruns:0 frame:0
         TX packets:574 errors:0 dropped:0 overruns:0 carrier:0
         collisions:0 txqueuelen:1000
         RX bytes:198035 TX bytes:125461
  ```
  or use this CLI `adb shell ip route | awk '{print $9}'` to directly get the target IP.<br>
  ![adb-ip-photo.png](/images/others/adb-ip-photo.png)

It looks like you haven't pasted the Markdown content yet. Please provide the content you want translated, and I'll handle the translation while adhering to the specified rules.
