# 十、执行JavaScript脚本
- 使用selenium直接在当前页面中进行js交互
- 常用的集中操作使用js实现

## （一）js的处理
1.selenium能够执行js，这使得selenium拥有更为强大的能力。既然能执行js，那么js能做的事，selenium应该大部分也能做。
- 直接使用js操作页面，能解决很多click()不生效的问题。
- 页面滚动到底部、顶部
- 处理富文本、时间空间的输入

2.例如js代码：
```
window.alert('selenium弹框测试')
a = document.getElementById('kw').value
document.title
JSON.stringify(performance.timing)
```

3.selenium如何调用js，selenium提供了一个内置的方法execute_script()
- driver.execute_script("window.alert(selenium弹框测试)")
- driver.execute_script("a=document.gerElementById('kw').value;window.alert(a)")

4.如何返回呢？
- driver.execute_script("return  document.getElementById('kw').value")

## （二）selenium中如何调用js
1.execute_script:执行js

2.return：可以返回js的返回结果

3.execute_script：argument传参

## （三）js处理-案例
1.案例1-滑动
1）场景：页面显示的数据比较多，需要点击底部的对象，我们就需要把鼠标移动到底部，才可以点击对象

案例1：滑动到浏览器底部或者顶部

1）打开百度首页

2）输入搜索关键字

3）点击搜索后，跳转到搜索结果页

4）滑动到底部点击“下一页”

可以先在页面的console中测试JavaScript，查看是否可以成功。


![selenium-js1](https://github.com/tete1987/picture_resource/blob/master/selenium/selenium-js1.png)

示例1：
```python
from time import sleep

from selenium.webdriver.common.by import By

from selenium_code.base import Base


class TestJs(Base):
    def test_js_scroll(self):
        self.driver.get("https://www.baidu.com/")
        self.driver.find_element_by_id("kw").send_keys("测试")
        element = self.driver.execute_script("return document.getElementById('su')")
        element.click()
        sleep(1)
        self.driver.execute_script("return document.documentElement.scrollTop=10000")
        sleep(2)
        self.driver.find_element(By.XPATH,'//*[@id="page"]//a[10]').click()
        sleep(2)
```

示例2（也可以增加页面性能数据）：
```python
from time import sleep

from selenium.webdriver.common.by import By

from selenium_code.base import Base


class TestJs(Base):
    def test_js_scroll(self):
        self.driver.get("https://www.baidu.com/")
        self.driver.find_element_by_id("kw").send_keys("测试")
        element = self.driver.execute_script("return document.getElementById('su')")
        element.click()
        sleep(1)
        self.driver.execute_script("return document.documentElement.scrollTop=10000")
        sleep(2)
        self.driver.find_element(By.XPATH,'//*[@id="page"]//a[10]').click()
        sleep(2)
        for code in [
            'return document.title','return JSON.stringify(performance.timing)'
        ]:
            print(self.driver.execute_script(code))
```

2.案例2-时间控件：

大部分时间空间都是readonly属性，需要手动去选择对应的时间，手工测试中很容易做到，自动化中对控件的操作可以使用js来操作。

1）处理时间控件思路：
- 需要取消日期的readonly属性；
- 给value赋值；
- 写js代码来实现如上的1，2点，再webdriver对js进行处理；

2）测试案例2：

步骤：
- 打开网页https://www.12306.cn/index/
- 修改出发日期为2020-12-30
- 打印出发日期
- 关闭网址

同样可以现在页面的console中进行测试：

![selenium-js2](https://github.com/tete1987/picture_resource/blob/master/selenium/selenium-js2.png)

示例3：
```python
from selenium_code.base import Base
from time import sleep

class TestJS(Base):
    def test_datetime(self):
        self.driver.get("https://www.12306.cn/index/")
        time_element = self.driver.execute_script("a=document.getElementById('train_date');a.removeAttribute('readonly')")
        self.driver.execute_script("document.getElementById('train_date').value='2020-12-30'")
        print(self.driver.execute_script(" return document.getElementById('train_date').value"))
        sleep(3)
```
