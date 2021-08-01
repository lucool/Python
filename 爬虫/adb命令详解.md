adb命令：

1.

查看打开app的package和appActivity

```
adb shell >  dumpsys activity top | grep ACTIVITY
```

2.启动app参数

```
desired_caps = {
    'platformName':'Android',
    'deviceName':'MI_NOTE_3',
    'appPackage':'com.wuba',
    'appActivity':'.home.activity.HomeActivity',
    'skipServerInstallation': True,
    'skipDeviceInitialization': True
}
```

