# 五、web控件定位与常见操作
## （一）selenium点击与输入
1.定义
- find_element(By.ID,'kw').send_keys("testing")
- find_element(By.ID,'su').click()

2.XPath

1）定义：
- XML Path Language
- 用于解析html 和 xml

2）xpath的位置：

![selenium5-1](https://github.com/tete1987/picture_resource/blob/master/selenium/selenium5-1.png)

xpath可以完美使用与appuim，selenium中。

3）xpath的使用
|表达式| 结果|
|--|--|
|/bookstore/book[1] |选取属于bookstore 子元素的第一个book元素|
|/bookstore/book[last()] |选取属于bookstore| 子元素的最后一个book元素|
|/bookstore/book[last()-1] |选取属于bookstore 子元素的倒数第二个book元素|
|/bookstore/book[position()-1] |选取最前面的两个属于bookstore 元素的子元素的book元素|
|//title[@lang='eng']| 选取所有title元素，且这些元素拥有值为eng的lang属性|
|/bookstore/book[price>25.00] |选取bookstore 元素的所有book元素，且其中的price元素的值必须大于25|
|/bookstore/book[price>25.00]/title |选取bookstore 元素的book元素的所有title元素，且其中的price元素的值必须大于25|
|nodename |选取此节点的所有子节点|
|/| 从根节点选取|
|//| 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置|
|.| 选取当前节点|
|.. |选取当前节点的父节点|
|@ |选取属性|

示例：（定位百度页面中的“网页”元素）

![selenium5-2](https://github.com/tete1987/picture_resource/blob/master/selenium/selenium5-2.png)

1）分析：“网页”的根元素为 id="s_tab"，"网页"是这个元素的子孙元素，写法应为：
```'//*[@ id="s_tab"]//b'```

为验证是否正确，可在页面的console中输入：
```$x('//*[@ id="s_tab"]//b')```

2）也可以使用数字定位（例如定位“资讯”）：
```$x('//*[@ id="s_tab"]//a[1]')```

3）定位最后一个：
```$x('//*[@ id="s_tab"]//a[last()]')```


2.CccSelector

css也可以使用与appuim，selenium中，但对于appium的原生控件不支持。

1）常用css selector
|选择器 |例子 |例子描述|
|--|--|--|
|.class |.intro| 选择class=“intro”的所有元素|
|#id |#firstname| 选择 id=“firstname”的所有元素|
|* |*| 选择所有元素|
|element |p |选择所有<p>元素|
|element,element| div，p |选择所有<div>元素和所有<p>元素|
|element element| div p |选择所有<div>元素内部的所有<p>元素|
|element>element |div>p |选择父元素<div>元素的所有<p>元素|
|element+element| div+p| 选择紧接在<div>元素之后的所有<p>元素|
|[attibute] |[target]| 选择带有target 属性所有元素|
|[attibute=value]| [target=_blank] |选择target=“_blank”的所有元素|
|:nth-child(n)| p:nth-child(2)| 选择属于其父元素的第二个子元素的每个<p>元素|
|element1~element2| p~ul |选择前面有<p>元素的每个<ul>元素|

以上的例子使用css进行演示：

1）使用id进行定位：
```$('kw')```


2）定位 id="s_tab"，"网页"是这个元素的子孙元素：
```$(#s_tab b)```


3）定位 id="s_tab"的最后一个元素：
```$(#s_tab a:nth-last-child(1))```


4）示例：
```python
from selenium import webdriver
from selenium.webdriver.common.by import By

class TestS01():
    def setup(self):
        self.driver =webdriver.Chrome()
        self.driver.get("https://www.baidu.com/")

    def test_css(self):
        self.driver.find_element(By.CSS_SELECTOR,'#kw').send_keys("abc")
        self.driver.find_element(By.ID,'su').click()

```
