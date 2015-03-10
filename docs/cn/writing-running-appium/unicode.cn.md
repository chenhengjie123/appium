## 多语言支持

有一个关于非拉丁字符有关的语法规范问题，对于带音标的字符，就会有多种编码形式。这样，对于
`é`这样的字母，有两种编码方式：一种是单独的字符`é`（音标和小写的拉丁字母E组合在一起），
另一种是音标跟在字母后边。为了解决这个问题，存在一个'标准'，
让["每个字符都有一个唯一的二进制表示"](http://www.unicode.org/reports/tr15/)。

幸运的是，正规的ASCII文本（即不需要是格式化的问本）不需要进行任何改变，并且多次执行
操作也不会有影响。因此，标准函数可以随意使用，而不用担心产生风险和不良影响。

```javascript
// javascript
var unorm = require('unorm');

'some ASCII text' === unorm.nfd('some ASCII text');
unorm.nfd('Adélaïde Hervé') === unorm.nfd(unorm.nfd('Adélaïde Hervé'));
```

所以，在测试的时候对Unicode字符的处理，你需要对字符进行格式化，确保期望的值和接收到的值一致。
有很多方法可以用来格式化，所以你要确保执行的是同样的方法！

```javascript
// javascript
var unorm = require('unorm');
driver
  .elementByAccessibilityId('find')
    .text()
    .then(function (txt) {
      unorm.nfd(txt).should.be(unorm.nfd("é Œ ù ḍ"));
    });
```

一个由不同unicode文本编码导致的问题的标志是断言失败但报告却显示两个看起来一模一样的字符串：

```shell
AssertionError: expected 'François Gérard' to deeply equal 'François Gérard'
      + expected - actual

      +"François Gérard"
      -"François Gérard"
```

当发生只因编码导致的问题时，输出_看上去_一样。从标准的角度，它们的编码应也和它们看上去那样相同。


### 查找器 (Finder)

需要被查找的字符也应该需要格式化。比如，你在一个iOS的app上有一个叫做`Найти`的按钮，你也应该在find命令中格式化它。

```javascript
// javascript
var unorm = require('unorm');
driver
  .findElementByXPath(unorm.nfd("//UIAButton[@name='Найти']"))
    .should.eventually.exist;
```

否则这个按钮可能无法被找到。


### 文本框 (Text Field)

默认情况下，iOS和Android的自动化工具都不支持向输入框输入非ASCII字符。

#### iOS

Appium 完全绕过键盘直接向iOS设备的输入框发送非ASCII字符。虽然这让这些文本在测试中被成功输入，但必须记住由键盘输入触发的业务逻辑将不会被测试到。

像上边说的一样，断言收到的文本前应该先格式化它。

```javascript
// javascript
var unorm = require('unorm');
var testText = unorm.nfd("é Œ ù ḍ");
driver
  .elementsByClassName('UIATextField').at(1)
    .sendKeys(testText)
    .text()
    .should.become(testText)
  .nodeify(done);
```

#### Android

通过下载并安装一个[特殊键盘](https://github.com/appium/io.appium.android.ime) ， Android 可以支持输入 Unicode 字符，这个输入法允许文本通过ASCII在Appium和被测应用之间进行通讯。

为了使用这个功能，将`unicodeKeyboard`设置为`true`。如果想要键盘设置在测试完成后自动回到原始状态，
将`resetKeyboard`设置为`true`。否则Appium测试结束后，Appium的Unicode键盘仍然会被激活。

翻译备注：这个Unicode键盘并非虚拟键盘，在界面上不会显示出来，所以要进行其他类型的测试必须切换回其他输入法。

所以测试可以通过`send_keys`向输入框输入Unicode字符。

```javascript
// javascript
var desired = {
  app: '/path/to/app',
  deviceName: 'Android Emulator',
  deviceVersion: '4.4',
  platformName: 'Android',
  unicodeKeyboard: true,
  resetKeyboard: true
};
var testText = 'é Œ ù ḍ';
driver
  .elementByClassName('android.widget.EditText')
  .sendKeys(testText)
  .text()
  .should.eventually.become(testText)
  .nodeify(done);
```
