<<<<<<< HEAD
# Appium 故障调试

如果你遇到问题，先不要提交 ticket 到 github 或者发信去 appium-disscuss 邮件列表求教，这里教你如何处理。

## 一般问题

* 确保你是根据 [README](README.md) 里面的入门指南按部就班的。
* 确保你的系统配置好了(比如， xCode 是最新的，Android SDK 安装好了， `ANDROID_HOME` 配置好了，参照 [setup instructions](running-on-osx.md) ).
* 确保你应用的路径正确。

## 如果你从 Appium.app 运行的话 (通过 dmg 安装的)
 
* 升级应用，然后重启。 如果你提示你无法升级，从[appium.io](http://appium.io)重新下载。

## 如果你从源代码运行 Appium 的话

* 在源代码目录下运行 `git pull` 确保你使用的是最新的代码。
* 根据你想要自动化的平台运行 `reset.sh` :
    
    ./reset.sh               # 所有平台
    ./reset.sh --ios         # ios-only
    ./reset.sh --android     # android-only
    ./reset.sh --selendroid  # selendroid-only
* 你可能会用到 `--dev`，如果你想下载和构建测试应用的话。
* 你也能使用 `appium-doctor` 来自动验证所有依赖是否符合。如果从源代码运行，你可以使用`bin/appium-doctor.js` 或者 `node bin/appium-doctor.js`。
* 如果你升级到 Android SDK 22，然后得到了如下错误：
   `{ANDROID_HOME}/tools/ant/uibuild.xml:155: SDK does not have any Build Tools installed.`
在 Android SDK 22 里，platform 和 build 工具被拆分到它们各自项中去。你需要确保安装了 build-tool 和 platform-tool。

## Android

* 确保 Android 模拟器启动并运行着。
* 有时候需要运行 `adb kill-server && adb devices`。它可以帮助重置和Android设备之间的连接。
* Make sure you know about the `app-package`, `app-activity`, and `app-wait-activity` desiredCapabilities (see [this doc](running-tests.md) for more information).
* 你得知道 `app-package`， `app-activity` 和 `app-wait-activity` (详见 [文档](running-tests.md).

## IOS

* 确保 Instruments.app 没打开。
* If you're running the simulator, make sure your actual device is not plugged in
* 如果你运行模拟器，确保没有连接真机。
* 确保你的 Setting 里面 accessibility 辅助功能已经关闭 
* 确保 APP 是为这个版本的模拟器编译的。
* 如果你曾经用 sudo 运行过 Appium， 你需要先删除`/tmp/instruments_sock`， `sudo rm /tmp/instruments_sock`。然后不要使用 sudo 启动 Appium。
* 如果你第一次运行 Appium，先授权使用 Instruments。通常会有一个对话框弹出让你输入你的密码。如果你从源代码运行 Appium，你只需简单的运行 `sudo grunt authorize`。如果用 npm 安装的话，运行 `sudo authorize_ios` 即可。
* 如果你看到 `iOS Simulator failed to install the application.` 而且 App 的路径都正确的，那么重启下电脑看看。

## Webview/Hybrid/Safari app 支持

* 确保真机上的'Web Inspector'激活了。
* 确保你激活了模拟器上的 Safari 的开发模式。Safari - Advance Preferences- Developer menu


## FirefoxOS

* 确保 Boot-to-Gecko 模拟器启动并运行着。
* 确保模拟器的屏幕是亮着并没用锁屏的(可能需要重启 B2G).

## 告诉社区

如果你通过以上方法都不能解决问题，你可以做：

如果你可以确认你发现的是个bug，来给我们报bug吧。[issue tracker](https://github.com/appium/appium/issues)

如果你的 Appium 无法正常工作，然后错误信息不够清晰，欢迎加入[mailing list](https://groups.google.com/d/forum/appium-discuss)。 给大家发邮件询问，但是请包含以下信息：

* 你是如何运行 Appium 的 (Appium.app, npm, source)
* 客户端和服务端的错误信息
* 除了上述， 贴出 Appium 服务器端的输出也非常重要，特别是运行在 verbose 模式。这样我们可以分析诊断问题在哪里。

## Known Issues

* 如果你从 Node 官网安装的 Node，那需要你使用 sudo 运行 npm。这其实并不好，试试看用 `brew install node`。
* 使用 proxy，Webview 可以支持 iOS 真机。 见 [discussion](https://groups.google.com/d/msg/appium-discuss/u1ropm4OEbY/uJ3y422a5_kJ).
* 有时候， iOS 的 UI 元素在定位到之后几毫秒会突然变得无效。这会导致一个类似`(null) cannot be tapped`的错误。唯一的解决方法就是把finding-and-acting的代码放到 retry 块里。参见 `mobile: findAndAct` [finding elements doc page](finding-elements.md)
* 如果你通过 MacPorts 安装的 node 和 npm，那么确保 MacPorts 的 bin 文件夹 (`/opt/local/bin`) 被添加到环境变量 `PATH` 中去。

## Specific Errors
=======
## Appium 故障排除

当你遇到问题时，请不要急着将问题提交到Github，也不用急着发到[appium-discuss discussion group](https://discuss.appium.io)，也许你可以在本文中找到答案。

### 常见问题解决办法

* 确保你的每一个步骤都是遵循 [入门指南](/docs/cn/README.cn.md) 来做的。
* 确保你的系统配置正确。（例如：Xcode是否升级到了最新版本，Android SDK是否有设置到环境变量`ANDROID_HOME`中去。）
* 确保你的应用存放路径没有错误。

### Appium.app运行出现问题的解决办法

* 升级Appium.app后重新打开即可解决。如果提示你不能升级，则需要重新下载Appium.app，下载地址：[appium.io](http://appium.io)


### 通过源码启用Appium出现问题的解决办法

* 使用`git pull`拉取最新源码，确保运行的代码是当前最新版本。
* 针对你所自动化的平台，运行`reset.sh`命令：

|命令                  | 说明 |
|-------------------------|-------------|
|./reset.sh               | # all |
|./reset.sh --ios         | # ios-only |
|./reset.sh --android     | # android-only |
|./reset.sh --selendroid  | # selendroid-only |

* 当你需要下载以及构建测试应用时，运行`reset.sh`时你需要用到`--dev`指令。
* 你也可以使用`appium-doctor`来自动检测你的环境依赖都是否正常。如果你是使用源码运行，则需要使用到`bin/appium-doctor.js`或`node bin/appium-doctor.js`。
* 当你将Android SDK升级到22后，可能出现如下错误：
 `{ANDROID_HOME}/tools/ant/uibuild.xml:155: SDK does not have any Build Tools installed.`
这是因为在Android SDK 22中，platform 和 build 工具被分拆到他们各自的SDK管理包中去了。你需要确保你的机器上正确安装了build-tools 和 platform-tools。

### Android常见问题解决办法

* 确保 Android 模拟器启动并运行着。
* 出现设备连接问题时，运行`adb kill-server && adb devices`是非常有效的。它能够帮助重置和连接Android设备。
* 请确保环境变量 ANDROID_HOME 指向的是正确的Android SDK的路径。

### IOS常见问题解决方案

* 确保Instruments.app是关闭的。
* 如果你是使用模拟器运行的，请不要将真机设备连接电脑。
* 确保模拟器或真机中，设置里面的accessibility辅助功能是关闭状态的。
* 确保App是编译在当前运行的模拟器上。
* 确保App是编译在合适的模拟器（或真机）上，不然会出现`posix spawn`的报错。（比如：运行在debug模式下的模拟器）
* 如果你曾经用 sudo 运行过 Appium， 你需要先删除/tmp/instruments_sock， 执行`sudo rm /tmp/instruments_sock`。然后在不适用SUDO的情况下再次启动Appium即可。
* 第一次运行Appium时，需要对Instruments进行授权。不然的话会经常弹出对话框要求你输入密码。如果你从源代码运行 Appium，你只需在主分支上运行`sudo grunt authorize`来回避该弹窗。如果用 npm 安装的话，运行 `sudo authorize_ios` 即可。注意，当你每次安装了新版本的xcode，你都需要重复以上操作。
* 如果检查路径正确，但仍然报 `iOS Simulator failed to install the application.`的错误的时候，请尝试重启你的电脑。

### Webview/Hybrid/Safari 应用支持

* 确保真机上的'Web Inspector'为打开状态。
* 确保打开了Safari的开发模式。（Safari - Advance Preferences- Developer menu for
  simulators）
* 确保由client library提供的Appium命令-`context`能够正常得对contexts进行切换。
* 当你尝试打开代理的时候，出现如下错误：select_port() failed，请参考[discussion](https://groups.google.com/forum/#!topic/appium-discuss/tw2GaSN8WX0)

### FirefoxOS常见问题解决办法

* 确保 Boot-to-Gecko 模拟器启动并运行着。
* 确保模拟器的屏幕是亮着并无锁屏的(可能需要重启 B2G).

### 到社区寻求帮助

若通过上述方法你的问题依然没有得到解决，你可以：

如果你的 Appium 无法正常工作，然后错误信息不够清晰，欢迎加入 [discussion group](https://discuss.appium.io)中发表你的问题，你的问题需要包括以下内容：

* 你是如何运行Appium的？(Appium.app, npm, source)
* 你使用的是什么操作系统？
* 你使用的是什么设备？版本是什么？ (i.e. Android 4.4, or iOS 7.1)
* 你使用的是真机还是模拟器？
* 给出你得到的客户端和服务端的出错日志 (比如,"我的Python代码中报了如下错误：balabala，在Appium server中的输出内容如链接中所示"）
* 除了上述， 贴出 Appium 服务器端的输出也非常重要，特别是运行在 verbose 模式。这样我们可以分析诊断问题在哪里。

如果你确信你发现的是一个BUG，请到[issue tracker](https://github.com/appium/appium/issues)中提交一个issue，并将BUG的内容描述清楚。

### 已知问题

* 如果你从 Node 官网安装的 Node，那需要你使用 sudo 运行 `npm`。 但这么做并不是非常理想。请尝试从
  [n](https://github.com/visionmedia/n) 获取node 或运行`brew install node`来安装 。
* Webview通过代理可以支持iOS真机设备，请参考[discussion](https://groups.google.com/d/msg/appium-discuss/u1ropm4OEbY/uJ3y422a5_kJ)
* 有时候， iOS 的 UI 元素在定位到之后几毫秒会突然变得无效。这会导致一个类似(null) cannot be tapped的错误。唯一的解决方法就是把finding-and-acting的代码放到 retry 块里。
* 如果你是通过MacPorts安装了Node和Npm，你必须确保MacPorts的bin文件夹已经被添加到环境变量`PATH`中去，不然Appium会出现难以找到可执行`node`的情况。


### 特定的错误
>>>>>>> FETCH_HEAD

|Action|Error|Resolution|
|------|-----|----------|
|Running reset.sh|xcodebuild: error: SDK "iphonesimulator6.1" cannot be located|安装 iPhone 6.1 SDK 或者 使用单独的 SDK 构建 待测应用 比如： `grunt buildApp:UICatalog:iphonesimulator5.1`|
<<<<<<< HEAD
=======
|Running reset.sh|Warning: Task "setGitRev" not found. Use --force to continue.|使用`git submodule update --init`更新模块并再次运行`reset.sh`|
|Running reset.sh|`[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:2.3.2:compile (default-compile) on project selendroid-server: Compilation failure [ERROR] Failure executing javac, but could not parse the error: [ERROR] [ERROR] [ERROR] The system is out of resources. [ERROR] Consult the following stack trace for details. [ERROR] java.lang.StackOverflowError `|`export MAVEN_OPTS="-Xms1024m -Xmx2048m -Xss2048k"`|
|Running ios test|`[INST STDERR] posix spawn failure; aborting launch`|你的应用没有正确编译在模拟器或真机上。|
|Running mobile safari test|`error: Could not prepare mobile safari with version '7.1'`|你可能需要在此运行授权脚本以保证使iOS SDK文件为可写状态。 E.g., `sudo authorize_ios`|
>>>>>>> FETCH_HEAD
