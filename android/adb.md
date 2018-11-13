adb简单使用
==========

## 连接指定设备

```
$ adb -s device_name shell

$ adb install name.apk

# /data/app目录下
# com.taobao.taobao-1 = com.taobao.taobao
$ adb uninstall package_name

## 所有安装的包名
$ adb shell pm list package

## 传送文件
$ adb push from to
$ adb pull from to

## 截图
$ adb shell screencap phone_path

# 获取包名和活动名
# 1
# package: name='com.ss.android.ugc.aweme'
# launchable-activity: name='com.ss.android.ugc.aweme.splash.SplashActivity'
$ Android\Sdk\build-tools\28.0.3\aapt.exe dump badging C:\Users\atled\Downloads\douyin.apk

# 2
# adb shell > logcat | grep cmp
```
