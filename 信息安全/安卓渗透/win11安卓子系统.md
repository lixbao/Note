# 使用Win11安装Android应用



# 先决条件申请

进行Android应用安全测试经常需要以下应用。并非所有这些对于 WSA 本身都不是必需的，但通常对 Android 测试和连接很有用。

- [Android Studio](https://developer.android.com/studio)是任何 Android 测试的必备工具。它提供了一个调试器，以及一种以易于理解的格式查看 APK 组件的方法。
- [ ](https://developer.android.com/studio/releases/platform-tools)[Android Debug Bridge](https://developer.android.com/studio/command-line/adb) (ADB)需要[Android SDK 工具](https://developer.android.com/studio/releases/platform-tools)，它将用于从命令行连接到 WSA、传输文件和安装应用程序。

- 我们将在安装之前使用[Objection](https://github.com/sensepost/objection)修补 APK，并对目标应用程序执行一些检测。objection具有[Wiki](https://github.com/sensepost/objection/wiki/Patching-Android-Applications)中提到的各种先决条件，也应该安装。
- Windows 11 – 在 Windows 功能设置中启用虚拟机平台。
- 最后，我们将需要[Python](https://www.python.org/downloads/windows/)来运行objection。





[适用于 Android™️ 的 Windows 子系统 | Microsoft Docs](https://docs.microsoft.com/zh-cn/windows/android/wsa/)