---
layout: post
title: "Android 11 adb wireless debugの力を解放：有線から無線へ、より自由なデバッグ体験を探るチュートリアル！"
date: 2022-02-22 15:16:12 +0800
image: cover/android-adb-wirless-share-1.png
tags: [Android,adb]
categories: Android教學
excerpt: "Android 11の真の力を引き出したいですか？無線adb wireless debugを探索しましょう！有線から無線へ、デバッグ体験をより自由で便利にします。"
---

<div class="c-border-main-title-2">前言</div>
プロジェクトのニーズに応じて、
私たちは常にADBデバッグの新しい方法を探しています。<br>
最近、Androidが新しい方法を発表しました。<br>
ここではadb wireless debugの研究成果を共有します。


<div align="start" class="table_container">
  ADB wifiを使ったデバッグに慣れていない方のために、以前の関連情報も共有していますので、ご参考ください。<br>
  <a href="{{site.baseurl}}/2022/02/15/android-adb-wifi-note/">
    <img src="/images/others/adb_wifi.png" alt="Cover" width="30%"/>
  </a>
  <a href="{{site.baseurl}}/2022/02/15/android-adb-wifi-note/">無線を活用：adbを使用してAndroid実機に無線で接続する方法</a>
</div><br>

<div class="c-border-main-title-2">Android 11 の新しいadb wireless debugの研究
  <a style="color:white;" href="https://developer.android.com/studio/command-line/adb#connect-to-a-device-over-wi-fi-android-11+">こちらを参照</a>
</div>

<div class="c-border-content-title-4"><b style="color:red;">Android 11</b> 以降のバージョンでwireless debug機能が追加されました。まず環境が必要です：</div>
  1. Android 11 以上<br>
  2. 接続するPCまたは自身の環境のplatform tool SDKバージョンが30.0.0以上であること (<b style="color:red;">adb version</b> で現在のSDKバージョンを確認)<br>
  3. 同じネットワーク内にいること<br>

<div class="c-border-content-title-4">実際の操作 <b>**(USBケーブル不要)**</b></div>
  1. Androidスマートフォンの設定で開発者モードを有効にし、「Wireless debugging」をオンにします<br>
  2. Wireless debuggingのサブメニューに入ります<br>
  3. 「pair device with code」をクリックして、IP、ポート、ペアリングコードを確認します<br>
  4. ターミナルで <b>adb pair `ipaddr`:`port`</b> コマンドを入力してペアリング<br>
  5. 「Enter pairing code:」と表示されたら、手順3で確認したペアリングコードを入力<br>

<div class="c-border-content-title-4">特徴</div>
  1. Wireless debuggingではペアリングされたデバイスを確認および管理できます<br>
  2. 次回からは自動的に再接続<br>
  3. USBケーブル不要でペアリング可能<br>
  4. 旧バージョン（Android 10以下）のadbの無線接続とは異なり、毎回手動で再接続する必要があります<br>
    - Android 10以下のadb接続方法の簡略説明  **(USBケーブル必要)**<br>
    TCP/IPポートの切り替え：<b style="color:red;">adb tcpip <port></b><br>
    <b style="color:red;">adb connect <ip>:<port></b> でIPとポートを入力して接続<br>

<div class="c-border-main-title-2">その他のメモ</div>

  - Android 11 AOSP内のdevelop optionにあるWirelessDebuggingFragmentでペアコードの生成方法を確認できます<br>
    -> AOSPフォルダのパス：<br>
  `/Android/11/packages/apps/Settings/com/android/settings/development/WirelessDebuggingFragment.java`<br>

    ここで見た結果、特殊なキーが必要なようです。<br>
    `アプリケーション層` でできるかどうかは不明です。<br>
    興味がある方はAOSP内を調べてみてください。<br>

  - 一部の機種ではAndroid 11がサポートされていません。つまり、メーカーがこの機能をロックする可能性があります。例：LGのスマートフォン<br>
     -> 開発者オプションにwireless debuggingオプションが表示されない場合があります。<br>
     したがって、実際にサポートされているかどうかは、メーカーが提供するOTAに依存します。<br>

  - `adb connect <ip>:<port> 接続` <br>
     -> これを使用して、wireless debuggingページに表示される<b style="color:red;">IP:port</b>に接続できるかどうかをテストします。<br>
     ただし、このコマンドはtcpipのipに接続するためのもの（ケーブルが必要）<br>
     もう一つは、wireless debuggingでペアリングされたデバイスを復元するためのものです。<br>
     したがって、手動で接続を復元するためにのみ使用できます。<br>
