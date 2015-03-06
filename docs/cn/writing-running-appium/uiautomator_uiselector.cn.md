## uiautomator UiSelector

Appium 能够使用 [UiSelectors](http://developer.android.com/tools/help/uiautomator/UiSelector.html) 执行搜索操作。
当然 [UiScrollable](http://developer.android.com/tools/help/uiautomator/UiScrollable.html)
也是支持的。

注意， 使用选择器索引是不可靠的，所以最好用实例代替。 下面是用 Ruby 针对包（ api demos ） 写的例子。


找到第一个文本控件。

```ruby
# ruby
first_textview = find_element(:uiautomator, 'new UiSelector().className("android.widget.TextView").instance(0)');
```

找到这个文本的第一个元素。

```ruby
# ruby
first_text = find_element(:uiautomator, 'new UiSelector().text("Animation")')
first_text.text # "Animation"
```

找到第一个可滚动的元素， 然后找到文本是 "Tabs" 的文本控件。
"Tabs" 元素就是将要滚动到的控件。

```ruby
# ruby
element = find_element(:uiautomator, 'new UiScrollable(new UiSelector().scrollable(true).instance(0)).getChildByText(new UiSelector().className("android.widget.TextView"), "Tabs")')
```

scrollIntoView 是一个特例，会返回滚动到指定控件的元素。
scrollIntoView 对任何的 UiSelector 都可以执行滚动操作。

```ruby
# ruby
element = find_element(:uiautomator, 'new UiScrollable(new UiSelector().scrollable(true).instance(0)).scrollIntoView(new UiSelector().text("WebView").instance(0));')
element.text # "WebView"
```