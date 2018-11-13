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
```
