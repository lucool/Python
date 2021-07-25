# Appium原理与安装

本教程讲解如何使用Appium进行手机应用的自动化。

学习本课程前，强烈推荐先学习 Selenium Web 自动化课程

## Appium 用途和特点

[点击这里，边看视频讲解，边学习以下内容](https://www.bilibili.com/video/av92305394?p=2)

`Appium` 是一个移动 App （手机应用）自动化工具。

手机APP 自动化有什么用？

- 自动化完成一些重复性的任务

  比如微信客服机器人

- 爬虫

  就是通过手机自动化爬取信息。

  为什么不通过网页、HTTP 爬取呢？有的系统没有网页，也不方便通过HTTP爬取

- 自动化测试

  很多企业里面有这样的需求



`Appium` 自动化方案的特点：

- 开源免费

- 支持多个平台

  iOS （苹果）、安卓 App 的自动化都支持。

- 支持多种类型的自动化

  支持 苹果、安卓 应用 原生界面 的自动化

  支持 应用 内嵌 WebView 的自动化

  支持 手机浏览器 中的 web网站自动化

  支持 flutter 应用的自动化

- 支持多种编程语言

  像 Selenium 一样， 可以用多种编程语言 调用它 开发自动化程序。

## 自动化原理

我们先来看一下Appium自动化的原理图

![image](https://img-blog.csdnimg.cn/20200223122125834.png)

这图是不是很眼熟？

对啦，和Selenium 原理图很像。因为 Appium自动化架构就是借鉴的Selenium。

大家看看这幅图， 包含了 3个主体部分 ： 自动化程序、Appium Server、移动设备

- 自动化程序

  自动化程序是由我们来开发的，实现具体的 手机自动化 功能。

  要发出具体的指令控制手机，也需要使用 **客户端库**。

  和Selenium一样，Appium 组织 也提供了多种编程语言的客户端库，包括 java，python，js， ruby等，方便不同编程语言的开发者使用。

  我们需要安装好客户端库，调用这些库，就可以发出自动化指令给手机。

- Appium Server

Appium Server 是 Appium 组织开发的程序，它负责管理手机自动化环境，并且转发 自动化程序的控制指令 给 手机，并且转发 手机给 自动化程序的响应消息。

- 手机设备

  我们这里说的手机设备，其实不仅仅是手机，包括所有 苹果、安卓的移动设备，比如：手机、平板、智能手表等。

  为了直观方便的讲解，这里我们简称： 手机

  当然手机上也包含了 我们要自动化控制的 手机应用APP。

  手机设备为什么能 接收并且处理自动化指令呢？

  因为，Appium Server 会在手机上 安装一个 自动化代理程序， 代理程序会等待自动化指令，并且执行自动化指令

比如：要模拟用户点击界面按钮，Appium 自动化系统的流程是这样的：

- 自动化程序 调用客户端库相应的函数， 发送 `点击元素` 的指令（封装在HTTP消息里）给 Appium Server
- Appium Server 再转发这个指令给 手机上的自动化代理
- 手机上的自动化代理 接收到 指令后，调用手机平台的自动化库，执行点击操作，返回点击成功的结果给 Appium Server
- Appium Server 转发给 自动化程序
- 自动化程序了解到本操作成功后，继续后面的自动化流程

其中，自动化代理控制，使用的什么库来实现自动化的呢？

如果测试的是苹果手机， 用的是苹果的 XCUITest 框架 （IOS9.3版本以后）

如果测试的是安卓手机，用的是安卓的 UIAutomator 框架 (Android4.2以后)

这些自动化框架提供了在手机设备上运行的库，可以让程序调用这些库，像人一样自动化操控设备和APP，比如：点击、滑动，模拟各种按键消息等。

## 自动化环境搭建

[点击这里，边看视频讲解，边学习以下内容](https://www.bilibili.com/video/av92305394?p=3)

本教程主要讲解 安卓APP的自动化。

环境搭建需要下载安装不少软件，而且还有不少是国外网站下载的。

为了方便各位朋友，白月黑羽把这些软件 最新的安装包 都放在如下的百度网盘链接中了，请大家自行下载。

链接：https://pan.baidu.com/s/19C9fGmoXne8DgfXhrTB2TQ

提取码：kgwb

### 安装client编程库

根据原理图， 我们知道自动化程序需要调用客户端库和 Appium Server 进行通信。

因为我们介绍Python语言开发，所以当然是用pip安装，如下

```
pip install appium-python-client
```

### 安装Appium Server

Appium Server 是用 nodejs 运行的，基于js开发出来的。

Appium组织为了方便大家安装使用，制作了一个可执行程序 Appium Desktop，把 nodejs 运行环境、Appium Server 和一些工具 打包在里面了，只需要简单的下载安装就可以了。

可以从 上面给出的百度网盘连接 下载安装： `Appium-windows-1.15.1.exe`



附加信息： Appium Desktop官方下载，[点击这里打开下载页面](https://github.com/appium/appium-desktop/releases/latest)

### 安装JDK

本教程主要讲解 安卓APP的自动化，必须要安装安卓SDK（后面会讲到），而安卓SDK需要 JDK 环境。

可以从 上面给出的百度网盘连接 下载安装： `jdk-8u211-windows-x64.exe`



安装好之后，还需要添加一个环境变量 `JAVA_HOME` ，指定 值 为 jdk安装目录，比如

```py
JAVA_HOME   d:\tools\java\jdk1.8.0_211
```

具体操作参考视频讲解。

### 安装 Android SDK

对于安卓APP的自动化，Appium Server 是需要 Android SDK的。

因为要用到里面的一些工具，比如 要执行命令设置手机、传送文件、安装应用、查看手机界面等。

可以从 上面给出的百度网盘连接 下载最新的 Android SDK文件包： `androidsdk.zip` ，并且解压，即可。



解压完成后，需要 配置一下 添加一个 环境变量 `ANDROID_HOME` ，设置值为sdk包解压目录，比如 `d:\tools\androidsdk`



另外，还推荐大家配置环境变量 PATH ，加入 adb所在目录， `d:\tools\androidsdk\platform-tools\`

注意：是 `添加` 该目录到环境变量PATH中， `！！！不是替换！！！` ，否则会导致系统命令都找不到的严重后果，初学者 请对照视频讲解操作。

### 连接手机

上述的软件环境都准备好以后，要自动化手机APP，需要：

- 在你运行程序的电脑上 用 USB线 连接上 你的安卓手机
- 进入 `手机设置` -> `关于手机` ，不断点击 `版本号` 菜单（7次以上），
- 退出到上级菜单，在开发者模式中，启动USB调试

如果手机连接USB线后，手机界面弹出 类似 如下提示。

![image](https://img-blog.csdnimg.cn/20200210132442926.png)

选择 允许USB调试。



注意：

有的手机系统，可能需要一些额外的选项需要设置好。

比如，有的手机，开发者选项里 需要打开 `允许通过USB安装应用` 等。

总之，给USB开发调试 尽可能方便的控制手机。



连接好以后，打开命令行窗口， 执行 `adb devices -l` 命令来列出连接在电脑上的安卓设备。

如果输出 类似如下的内容：

```py
List of devices attached
4d0035dc767a50bb        device product:t03gxx model:GT_N7100 device:t03g
```

表示电脑上可以查看到 连接的设备，就可以运行自动化程序了。

## 一个例子

[点击这里，边看视频讲解，边学习以下内容](https://www.bilibili.com/video/av92305394?p=4)

下面是一段使用 Appium 自动化的打开 B站 应用，搜索 `白月黑羽` 发布的教程视频，并且打印视频标题的示例。

```py
from appium import webdriver
from appium.webdriver.extensions.android.nativekey import AndroidKey

desired_caps = {
  'platformName': 'Android', # 被测手机是安卓
  'platformVersion': '8', # 手机安卓版本
  'deviceName': 'xxx', # 设备名，安卓手机可以随意填写
  'appPackage': 'tv.danmaku.bili', # 启动APP Package名称
  'appActivity': '.ui.splash.SplashActivity', # 启动Activity名称
  'unicodeKeyboard': True, # 使用自带输入法，输入中文时填True
  'resetKeyboard': True, # 执行完程序恢复原来输入法
  'noReset': True,       # 不要重置App
  'newCommandTimeout': 6000,
  'automationName' : 'UiAutomator2'
  # 'app': r'd:\apk\bili.apk',
}

# 连接Appium Server，初始化自动化环境
driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps)

# 设置缺省等待时间
driver.implicitly_wait(5)

# 如果有`青少年保护`界面，点击`我知道了`
iknow = driver.find_elements_by_id("text3")
if iknow:
    iknow.click()

# 根据id定位搜索位置框，点击
driver.find_element_by_id("expand_search").click()

# 根据id定位搜索输入框，点击
sbox = driver.find_element_by_id('search_src_text')
sbox.send_keys('白月黑羽')
# 输入回车键，确定搜索
driver.press_keycode(AndroidKey.ENTER)

# 选择（定位）所有视频标题
eles = driver.find_elements_by_id("title")

for ele in eles:
    # 打印标题
    print(ele.text)

input('**** Press to quit..')
driver.quit()
```

运行代码前，要先 `运行 Appium Desktop`

[点击这里，观看视频里面对上述代码的讲解](https://www.bilibili.com/video/av92305394?p=5)

上面的代码只是抓取一页视频标题，要自动化滚动抓取所有的视频标题，`实战班学员` ，请联系老师获取参考代码。

`游客` 也可以 分享课程 领取参考代码，[点击这里查看](http://www.python3.vip/adv/bonus/appium1/)

## 查找 应用 Package 和 Activity

[点击这里，边看视频讲解，边学习以下内容](https://www.bilibili.com/video/av92305394?p=6)

### 没有apk

如果你应用已经安装在手机上了，可以直接打开手机上该应用，进入到你要操作的界面

然后执行

```py
adb shell dumpsys activity recents | find "intent={"
```

会显示如下，最近的 几个 activity 信息，

```py
intent={act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=tv.danmaku.bili/.ui.splash.SplashActivity}
intent={act=android.intent.action.MAIN cat=[android.intent.category.HOME] flg=0x10000300cmp=com.huawei.android.launcher/.unihome.UniHomeLauncher}
intent={flg=0x10804000 cmp=com.android.systemui/.recents.RecentsActivity bnds=[48,1378][10322746]}
intent={flg=0x10000000 cmp=com.tencent.mm/.ui.LauncherUI}
```

其中第一行就是当前的应用，我们特别关注最后

```bash
cmp=tv.danmaku.bili/.ui.splash.SplashActivity
```

应用的package名称就是 `tv.danmaku.bili`

应用的启动Activity就是 `.ui.splash.SplashActivity`

### 有apk

如果你已经获取到了 apk，在命令行窗口执行

```
d:\tools\androidsdk\build-tools\29.0.3\aapt.exe dump badging d:\tools\apk\bili.apk | find "package: name="
```

输出信息中，就有应用的package名称

```py
package: name='tv.danmaku.bili' versionCode='5531000' versionName='5.53.1' platformBuildVersionName='5.53.1' compileSdkVersion='28' compileSdkVersionCodename='9'
```

在命令行窗口执行

```
d:\tools\androidsdk\build-tools\29.0.3\aapt.exe dump badging d:\tools\apk\bili.apk | find "launchable-activity"
```

输出信息中，就有应用的启动Activity

```py
launchable-activity: name='tv.danmaku.bili.ui.splash.SplashActivity'  label='' icon=''
```

# 定位元素

## 代码规则

[点击这里，边看视频讲解，边学习以下内容](https://www.bilibili.com/video/av92305394?p=7)

从示例代码，大家就可以发现，和Selenium Web自动化一样，要操作界面元素，必须先 定位（选择）元素。

Appium是基于Selenium的，所以 和 Selenium 代码 定位元素的 基本规则相同：

- `find_element_by_XXX` 方法，返回符合条件的第一个元素，找不到抛出异常
- `find_elements_by_XXX` 方法，返回符合条件的所有元素的列表，找不到返回空列表
- 通过 `WebDriver` 对象调用这样的方法，查找范围是整个界面
- 通过 `WebElement` 对象调用这样的方法，查找范围是该节点的子节点

## 界面元素查看工具

[点击这里，边看视频讲解，边学习以下内容](https://www.bilibili.com/video/av92305394?p=8)

做 Selenium Web 自动化的时候，要找到元素，我们是通过浏览器的开发者工具栏来查看元素的特性，根据这些特性（属性和位置），来定位元素

Appium 要自动化手机应用，同样需要工具查看界面元素的特征。

常用的查看工具是： Android Sdk包中的 `uiautomateviewer` 和 Appium Desktop 中的 `Appium Inspector`

### uiautomateviewer

安卓查看APP界面元素，最常用的就是 Android SDK 中的工具 `uiautomateviewer` ，它在SDK目录目录 的 tools\bin 目录中

和Selenium一样，我们要定位选择元素，也是根据元素的特征，包括

- 元素的属性
- 元素的相对位置（相对父元素、兄弟元素等）

具体细节，参考视频里面的讲解。

### Appium Inspector

Appium Desktop 中的 `Appium Inspector` 也可以查看元素。

它的一个优点是可以直接验证 选择表达式是否能定位到元素

参考视频里面的讲解。

## 定位元素的方法

[点击这里，边看视频讲解，边学习以下内容](https://www.bilibili.com/video/av92305394?p=9)

### 根据ID

在Selenium Web自动化教程里，我们说过，如果能根据ID选择定位元素，最好根据ID，因为通常来说ID是唯一的，所以根据ID选择 效率高。

在安卓应用自动化的时候，同样可以根据ID查找。

但是这个ID ，是安卓应用元素的 `resource-id` 属性

使用如下代码

```py
driver.find_element_by_id('expand_search')
```

具体细节，参考视频里面的讲解。

### 根据CLASS NAME

安卓界面元素的 class属性 其实就是根据元素的类型，类似web里面的tagname， 所以通常不是唯一的。

通常，我们根据class 属性来选择元素， 是要选择多个而不是一个。

当然如果你确定 要查找的 界面元素的类型 在当前界面中只有一个，就可以根据class 来唯一选择。

使用如下代码

```py
driver.find_elements_by_class_name('android.widget.TextView')
```

### 根据ACCESSIBILITY ID

元素的 content-desc 属性是用来描述该元素的作用的。

如果要查询的界面元素有 content-desc属性，我们可以通过它来定位选择元素。

使用如下代码

```py
driver.find_element_by_accessibility_id('找人')
```

### Xpath

Appium 也支持通过 Xpath选择元素。

但是其可靠性和性能不如 Selenium Web自动化。因为Web自动化对Xpath的支持是由浏览器实现的，而Appium Xpath的支持是 Appium Server实现的。

毕竟，浏览器产品的成熟度比Appium要高很多。

当然，Xpath是标准语法，所以这里表达式的语法规则和 以前学习的Selenium里面Xpath的语法是一样的，比如

```py
driver.find_element_by_xpath('//ele1/ele2[@attr="value"]')
```



注意：

selenium自动化中， xpath表达式中每个节点名是html的tagname。

但是在appium中， xpath表达式中 每个节点名 是元素的class属性值。

比如：要选择所有的文本节点，就使用如下代码

```py
driver.find_element_by_xpath('//android.widget.TextView')
```

如果自学速度慢、难点搞不定，欢迎来报 实战班 学习。
白月黑羽 1对1 指导，大量练习实战，学习效果是自学没法比的。还有商业项目实战。

[点击这里查看实战班介绍](http://www.python3.vip/adv/vipcourse/) 咨询微信：byhy444

### 安卓 UIAutomator

[点击这里，边看视频讲解，边学习以下内容](https://www.bilibili.com/video/av92305394?p=10)

根据id，classname， accessibilityid，xpath，这些方法选择元素，其实底层都是利用了安卓 uiautomator框架的API功能实现的。

参考 这里的谷歌安卓官方文档介绍： https://developer.android.google.cn/training/testing/ui-automator

也就是说，程序的这些定位请求，被Appium server转发给手机自动化代理程序，就转化为为uiautomator里面相应的定位函数调用。

其实，我们的自动化程序，可以直接告诉 手机上的自动化代理程序，让它 调用UI Automator API的java代码，实现最为直接的自动化控制。

主要是通过 UiSelector 这个类里面的方法实现元素定位的，比如

```py
code = 'new UiSelector().text("热门").className("android.widget.TextView")'
ele = driver.find_element_by_android_uiautomator(code)
ele.click()
```

就是通过 text 属性 和 className的属性 两个条件 来定位元素。



UiSelector里面有些元素选择的方法 可以解决 前面解决不了的问题。

比如

- `text` 方法

  可以根据元素的文本属性查找元素

- `textContains`

  根据文本包含什么字符串

- `textStartsWith`

  根据文本以什么字符串开头

- `textmartch` 方法

  可以使用正则表达式 选择一些元素，如下

  ```py
  code = 'new UiSelector().textMatches("^我的.*")'
  ```



UiSelector 的 `instance` 和 `index` 也可以用来定位元素，都是从0开始计数， 他们的区别：

- instance是匹配的结果所有元素里面 的第几个元素
- index则是其父元素的几个节点，类似xpath 里面的*[n]



UiSelector 的 `childSelector` 可以选择后代元素，比如

```py
code = 'new UiSelector().resourceId("tv.danmaku.bili:id/recycler_view").childSelector(new UiSelector().className("android.widget.TextView"))'

ele = driver.find_element_by_android_uiautomator(code)
```

注意： childSelector后面的引号要框住整个 子 uiSelector 的表达式

目前有个bug：只能找到符合条件的第一个元素，参考appium 在github上的 issues：

https://github.com/appium/java-client/issues/150

# 界面操作 和 adb 命令

## 界面操作

### click点击

最常见的操作之一，使用 WebElement 对象的 `click` 方法， 示例代码就讲过，不再赘述

### tap点按

WebElement 对象的 `tap` 方法和 click 类似，都是点击界面。

但是最大的区别是， tap是 `针对坐标` 而不是针对找到的元素。

为了保证自动化代码在所有分辨率的手机上都能正常执行，我们通常应该使用click方法。

但有的时候，我们难以用通常的方法定位元素， 可以用这个tap方法，根据坐标来点击

既然tap是用坐标来点击界面的，我们怎么知道这个元素的坐标呢？

大家还记得用inspect 查看该元素的属性中，有一个 `bounds` 属性吗？

它就是表示元素的左上角，右下角坐标的 坐标。

我们还可以使用 UIAutomatorviewer 直接光标移动，看右边的属性提示。

tap 方法可以像这样进行调用

```py
driver.tap([(850,1080)],300)
```

它 有 两个参数：

- 第一个参数是个列表，表示点击的坐标。

  注意最多可以有5个元素，代表5根手指点击5个坐标。所以是list类型。

  如果我们只要模拟一根手指点击屏幕，list中只要一个元素就可以了

- 第二个参数 表示 tap点按屏幕后 停留的时间。

  如果点按时间过长，就变成了长按操作了。

### 输入

最常见的操作之一， 使用 WebElement 对象的 `send_keys` 方法， 示例代码就讲过，不再赘述

### 获取界面文本信息

可以通过 WebElement 对象的 `.text` 属性获取该对象的文本信息，示例代码就讲过，不再赘述。

### 滑动

我们做移动app测试的时候，经常需要滑动界面。

怎么模拟滑动呢？ WebDriver对象的 swipe方法，就提供了这个功能

比如

```py
driver.swipe(start_x=x, start_y=y1, end_x=x, end_y=y2, duration=800)
```

前面4个参数 是 滑动起点 和 终点 的x、y坐标。

第5个参数 duration是滑动从起点到终点坐标所耗费的时间。

注意这个时间非常重要，在屏幕上滑动同样的距离，如果时间设置的很短，就是快速的滑动。

比如：一个翻动新闻的界面，快速的滑动，就会是扫动的动作，会导致内容 `惯性` 滚动很多。

如果自学速度慢、难点搞不定，欢迎来报 实战班 学习。
白月黑羽 1对1 指导，大量练习实战，学习效果是自学没法比的。还有商业项目实战。

[点击这里查看实战班介绍](http://www.python3.vip/adv/vipcourse/) 咨询微信：byhy444

### 按键

前面的示例代码中已经使用过 调用 `press_keycode` 方法，就能模拟 按键动作，包括安卓手机的实体按键和 键盘按钮。

如下代码所示

```py
from appium.webdriver.extensions.android.nativekey import AndroidKey

# 输入回车键，确定搜索
driver.press_keycode(AndroidKey.ENTER)
```

按键的定义，可以参考这篇文档 https://github.com/appium/python-client/blob/master/appium/webdriver/extensions/android/nativekey.py

### 长按、双击、移动

Appium的 TouchAction 类提供了更多的手机操作方法，比如：长按、双击、移动

参考源代码中的注释 https://github.com/appium/python-client/blob/master/appium/webdriver/common/touch_action.py

比如，下面就是一个长按的例子

```py
from appium.webdriver.common.touch_action import TouchAction
# ...
actions = TouchAction(driver)
actions.long_press(element)
actions.perform()
```

### 查看通知栏

- 打开通知栏

  安卓手机， 查看通知栏的动作可以是从屏幕顶端下滑来查看通知。

  我们刚刚学过滑动，感兴趣的朋友可以自己试试，关键是要找对滑动的起始点和滑动距离。

  更方便的，我们可以使用如下代码，直接打开通知栏

  ```py
  driver.open_notifications()
  ```

  通知栏里面的元素，自动化的方法 和 前面介绍的App界面元素自动化是一样的。

- 收起通知栏

  收起通知栏，可以使用前面介绍的模拟按键， 发出返回按键 的方法。

## adb 命令

这里我们给大家 介绍一个android sdk里面的命令行工具 `adb` 。

adb 全程 `Android Debug Bridge`，这个adb 使用非常广泛。

它可以与 Android 手机设备进行通信，它可进行各种设备操作。

比如： 安装应用和调试应用，传输文件，甚至登录到手机设备上shell的进行访问，就像远程登录一样

这个adb 在 sdk的 `platform-tools` 目录下面， 请大家确保路径在path环境变量中。

Appium 对anroid的自动化就非常依赖这个adb工具。 执行自动化过程中，有很多内部操作，比如获取设备信息，传送文件到手机，安装apk，启动某些程序等，都是通常这个adb实现的。



大家想想我们学习了adb命令，对我们的自动化程序有什么用例呢？

既然这是个命令，就可以使用 Python的 `os.system()` 或者 `subprocess` 来自动化调用它，完成我们的各种自动化需求。

比如，我们自动化过程中，可能需要截屏手机，并且下载到指定目录中，就可以在我们的Python程序中这样写

```py
import os
os.system('adb shell screencap /sdcard/screen3.png && adb pull /sdcard/screen3.png')
```

特别是，还可以通过adb 使用 am(activity manager) 和pm (package manager) 两个工具， 可以启动 Activity、强行停止进程、广播 intent、修改设备屏幕属性、列出应用、卸载应用等。



大家可以[点击这里查看官方文档中介绍的adb命令](https://developer.android.google.cn/studio/command-line/adb.html#devicestatus)

下面我们列出了一些场景的adb命令

### 查看连接的设备

```py
adb devices -l
```

### 列出文件和传输文件

- 查看目录

```py
adb shell ls /sdcard
```

- 上传

```py
adb push wv.apk /sdcard/wv.apk
```

- 下载

```py
adb pull /sdcard/new.txt
```

### 截屏

```py
adb shell screencap /sdcard/screen.png
```

截屏后的文件存在手机上，可以使用 adb pull 下载下来

### shell

登录到手机设备上shell的进行访问，就像远程登录一样，可用来在连接的设备上运行各种命令。

大家可以 执行一下 adb shell 然后执行各种 安卓支持的 Linux命令，比如 ps、netstat、netstat -an|grep 4724、 pwd、 ls 、cd 、rm 等。

执行quit退出 shell