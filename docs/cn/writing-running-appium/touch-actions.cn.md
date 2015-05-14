## 移动手势的自动化

虽然Selenium WebDriver的规范支持数种手机交互的方式，但它的参数并不能简单地映射到底层设备使用的自动化函数 (像在iOS上的UIAutomation) 。为此，Appium在规范的最新版本中定义了新的触摸操作/多点触控 API
([https://dvcs.w3.org/hg/webdriver/raw-file/tip/webdriver-spec.html#multiactions-1](https://dvcs.w3.org/hg/webdriver/raw-file/tip/webdriver-spec.html#multiactions-1))。
注意，这跟在早期版本中使用原始JSON Wire Protocol 的触摸操作 API不同。

这些API可以让你使用多个驱动来建立任意手势。请参阅对应语言的Appium客户端文档，就可以找到使用这些API的例子。


### 触摸操作/多点触控 API的概述

### 触摸操作 (TouchAction) 

*TouchAction* 对象包含一连串的事件。

在所有的appium客户端库中，触摸对象创建并给出一连串的事件。

规范中的可用事件有：
 * 短按 (press) 
 * 释放 (release) 
 * 移动到 (moveTo) 
 * 点击 (tap) 
 * 等待 (wait) 
 * 长按 (longPress) 
 * 取消 (cancel) 
 * 执行 (perform) 

这里有一个通过伪代码创建动作的例子：

```center
TouchAction().press(el0).moveTo(el1).release()
```

上述模拟用户按下一个元素，滑动他的手指到另一个位置，然后从屏幕上释放其手指。

Appium按顺序执行这些事件。你可以添加一个 `wait` 事件来控制相应手势的时间。

appium客户端库有不同的方式来实现上述例子，比如：你可以传递一个坐标值或一个元素给 `moveTo` 事件。同时传递坐标和元素，会将坐标和元素对应起来，但这不是绝对的。


调用 `perform` 事件发送整个事件序列给appium，从而使触摸手势在设备上运行。

Appium客户端还允许人们直接通过驱动程序对象执行触摸操作, 而不是调用触摸操作对象的`perform`事件。


在伪代码中，以下两个是等价的：

```center
TouchAction().tap(el).perform()

driver.perform(TouchAction().tap(el))
```

### 多点触控 (MultiTouch) 

*MultiTouch* 对象是触摸操作的集合。

多点触控手势只有两个方法，添加 (`add`) 和执行 (`perform`) 。

`add` 用于将不同的触摸操作添加到一个多点触控中。

当 `perform` 被调用的时候，所有被添加到多点触摸中的触摸事件会被发送到appium并且被执行，就像它们同时发生一样。Appium会执行“触摸事件”中的第一个事件，然后第二个，以此类推。


用两只手指点击的代码示例：

```center
action0 = TouchAction().tap(el)
action1 = TouchAction().tap(el)
MultiAction().add(action0).add(action1).perform()
```



### 缺陷和解决方法

不幸的是有一个缺陷在 iOS 7.0~8.x 的模拟器上。ScrollViews，CollectionViews 和 TableViews 无法识别由 UIAutomation 创建的手势 （在 iOS 上 Appium 使用的是 UIAutomation ）。为了解决这个问题，我们提供了一个新的函数， scroll ，让你在大部分情况下做你想对这类控件做的操作——如名称所示——滚动。



**滚动**


要使用这特殊的功能，我们重写了driver中的 `execute` 和
`executeScript` 方法。 可以通过在命令前加 `mobile: ` 的前缀来使用滚动。
请参见下面的例子：

在滚动前，通过参数传入你想滚动的方向。

```javascript
// javascript
driver.execute("mobile: scroll", [{direction: 'down'}])
```

```java
// java
JavascriptExecutor js = (JavascriptExecutor) driver;
HashMap<String, String> scrollObject = new HashMap<String, String>();
scrollObject.put("direction", "down");
js.executeScript("mobile: scroll", scrollObject);
```

```ruby
# ruby
execute_script 'mobile: scroll', direction: 'down'
```

```python
# python
driver.execute_script("mobile: scroll", {"direction": "down"})
```

```csharp
// c#
Dictionary<string, string> scrollObject = new Dictionary<string, string>();
scrollObject.Add("direction", "down");
((IJavaScriptExecutor)driver).ExecuteScript("mobile: scroll", scrollObject));
```

```php
$params = array(array('direction' => 'down'));
$driver->executeScript("mobile: scroll", $params);
```

使用方向和元素来滚动的示例。

```javascript
// javascript
driver.execute("mobile: scroll", [{direction: 'down', element: element.value}]);
```

```java
// java
JavascriptExecutor js = (JavascriptExecutor) driver;
HashMap<String, String> scrollObject = new HashMap<String, String>();
scrollObject.put("direction", "down");
scrollObject.put("element", ((RemoteWebElement) element).getId());
js.executeScript("mobile: scroll", scrollObject);
```

```ruby
# ruby
execute_script 'mobile: scroll', direction: 'down', element: element.ref
```

```python
# python
driver.execute_script("mobile: scroll", {"direction": "down", element: element.getAttribute("id")})
```

```csharp
// c#
Dictionary<string, string> scrollObject = new Dictionary<string, string>();
scrollObject.Add("direction", "down");
scrollObject.Add("element", <element_id>);
((IJavaScriptExecutor)driver).ExecuteScript("mobile: scroll", scrollObject));
```

```php
$params = array(array('direction' => 'down', 'element' => element.GetAttribute("id")));
$driver->executeScript("mobile: scroll", $params);
```

**滑块的自动化**


**iOS**

 * **Java**

```java
// java
// 滑动值使用0到1之间的数字以字符串的形式表示
// 例如，“0.1”代表10％，“1.0”代表100％
WebElement slider =  driver.findElement(By.xpath("//window[1]/slider[1]"));
slider.sendKeys("0.1");
```

**Android**

与Android上的滑块进行交互的最佳方式是用触摸操作 (TouchActions) 。
