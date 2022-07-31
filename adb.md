设置全局代理

adb connect 127.0.0.1:58526
adb shell settings put global http_proxy 192.168.43.237:8080

删除全局代理

adb shell settings delete global http_proxy
adb shell settings delete global global_http_proxy_host
adb shell settings delete global global_http_proxy_port
adb reboot

adb -s device1 reboot（重启指定设备）

adb devices（查看当前连接的ADB设备）

adb kill-server（关闭adb服务）

taskkill /f /im adb.exe（杀adb进程-关服务不可用时使用）

adb -s device1 shell（连接指定设备的shell）

adb shell （su转root）

adb install .\pivaa.objection.apk（adb安装程序）

adb push .\burpwsa.cer /storage/emulated/0/Download（adb上传文件）

adb shell reboot -p（关机）

## 在Android上安装Burp证书

将burp证书从DER转换为PEM。如果您懒惰，则可以在此存储库上下载PEM文件。

certutil -hashfile cacert.pem MD5

```bash
openssl x509 -inform DER -in cacert.der -out cacert.pem
# Get subject_hash_old (or subject_hash if OpenSSL < 1.0)
openssl x509 -inform PEM -subject_hash_old -in cacert.pem |head -1
mv cacert.pem 9a5ba575.0
```

将PEM文件安装到设备上的“系统受信任的凭据”中。

```perl
adb root
adb remount  
adb push 9a5ba575.0 /system/etc/security/cacerts/  
adb shell "chmod 644 /system/etc/security/cacerts/9a5ba575.0"
adb shell "reboot" 
```

如果/ system无法安装，则必须先安装。

```bash
adb root
adb shell
Check mounting list
cat /proc/mounts
#/dev/block/bootdevice/by-name/system /system ext4 ro,seclabel,relatime,discard,data=ordered 0 0
mount -o rw,remount -t rfs /dev/block/bootdevice/by-name/system /system
adb push 9a5ba575.0 /system/etc/security/cacerts/  
adb shell "chmod 644 /system/etc/security/cacerts/9a5ba575.0"
adb shell "reboot" 
```



# [检查Android架构]
adb shell getprop | grep abi
# [尝试使用此命令获取简单输出]
adb shell getprop ro.product.cpu.abi

#[列出所有已安装的应用程序]
adb shell pm list packages -f | grep -i 'testing'

# [在android上跟踪日志]
adb logcat | grep com.app.testing

# [将应用程序安装到设备]
adb install app.testing.apk

# [获取应用程序的完整路径]
adb shell pm path com.example.someapp

# [下载apk到开发机器]
adb pull /data/app/com.example.someapp-2.apk

# [在应用程序上下载活动]
adb shell dumpsys activity top | grep ACTIVITY

# [在adb shell中创建新文件]
cat > filename.xml
# [可以使用以下方法向文本文件添加行：]
cat >> filename.xml
# [两个命令都可以使用ctrl-D终止]

# [下载内存]
adb shell dumpsys meminfo com.package.name

### 安装证书时的坑

**注意点：**

1. 苹果安装证书后，需要完全信任证书；
   `设置->通用->关于本机->证书信任设置->开启针对根证书启用完全信任`
2. 安卓 7.0 及以上版本安装默认不信任用户自身安装的证书，解决办法有:

- 导出 burp 证书 cacert.der
- 将 der 格式转为 pem：`openssl x509 -inform DER -in cacert.der -out cacert.pem`
- 计算证书 MD5：`openssl x509 -inform PEM -subject_hash_old -in cacert.pem | head -1`
  burp 的证书计算结果为 `9a5ba575`
- 修改证书名：`mv cacert.pem 9a5ba575.0`
- 默认 /system 分区为 Read-only，执行以下三条即可解除限制、才可添加证书到 system 分区

```verilog
adb root  # 很多时候会发现用不了，需要 adb shell 后执行 " mount -o remount, rw /system "，复制完证书之后记得恢复 " mount -o remount, ro /system "
adb disable-verity 
adb reboot
```

- 重新挂载分区

```undefined
adb root 
adb remount
```

- push 证书：`adb push ./9a5ba575.0 /system/etc/security/cacerts/`
- 给证书加权限，并重启

```dockerfile
adb  shell
chmod 644 /system/etc/security/cacerts/9a5ba575.0
reboot
```

**小 tips**：可以直接安装证书为用户证书，然后在 [magisk](https://github.com/topjohnwu/Magisk) 中安装 [MagiskTrustUserCerts](https://github.com/NVISOsecurity/MagiskTrustUserCerts) 插件即可，这样可以有效解决 安卓 10 采用了的安全策略-将系统分区 /system 挂载为只读，就算你 root 了也没用这种情况
