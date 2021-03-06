# 六、web控件的交互进阶
## （一）Actions
1.官方文档：https://selenium-python.readthedocs.io/api.html

2.selenium的两个接口：
- ActionChains：执行PC端的鼠标点击、双击、右键、拖拽等事件
- TouchActions：模拟PC和移动端的点击、滑动、拖拽、多点触控等多种收拾操作

## （二）ActionChains的方法列表：
ActionChains类是selenium对鼠标、键盘操作的

1.常用操作包括：
- click(on_element=None)——单击鼠标左键
- click_and_hold(on_element=None)——点击鼠标左键，不松开
- context_click(on_element=None)——点击鼠标右键
- double_click(on_element=None)——双击鼠标左键
- drag_and_drop(sourse,targer)——拖拽到某个元素到目标位置后松开
- drag_and_drop_by_offset(sourse,xoffest,yoffest)——拖拽到某个坐标然后松开
- move_by_offset(xoffset,yoffset)——鼠标从当前位置移动到某个位置
- move_to_element(to_element)——鼠标移动到某个元素
- move_to_element_with_offset(to_element,xoffset,yoffset)——移动到距某个元素（左上角坐标）多少距离的位置
- perform()——执行链中的所有动作
- release(on_element=None)——在某个元素位置松开鼠标左键
- send_keys(keys_to_send)——发送某个键当前焦点的元素
- key_down(value,element=None)——按下键盘上的某个键
- key_up(value,element=None)——松开某个键

2.动作链接ActionChains

1）执行原理：
- 调用ActionChains的方法时，不会立即执行，而是将所有的操作，按顺序存放在一个队列里，当你调用perform()方法时，队列中的事件会依次执行

2）基本用法：
- 生成一个动作action=ActionChains(driver)
- 动作添加方法1 action.方法1
- 动作添加方法2  action.方法2
- 调用perform()方法执行（action.perform()）

3）具体写法：

链接写法：
- ActionChains(driver).move_to_element(element).click(element).perform

分布写法：
- actions =ActionChains(drvier)
- actions.move_to_element(element)
- actions.click(element)
- actions.perform()


3.ActionChains用法1

1）用法一：点击，右键，双击操作
- actions = ActionChains(driver)
- action.click(element)
- action.double_click(element)
- action.context_click(element)
- action.perform()


示例：
```python
from time import sleep
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

class TestAction():

    def setup(self):
        self.driver = webdriver.Chrome()
        self.driver.get("http://sahitest.com/demo/clicks.htm")
        self.driver.implicitly_wait(5)

    def teardown(self):
        self.driver.quit()

    def test_action(self):
        ele_click = self.driver.find_element_by_xpath('//input[@value="click me"]')
        ele_doubleClick=self.driver.find_element_by_xpath('//input[@value="dbl click me"]')
        ele_rightClick = self.driver.find_element_by_xpath('//input[@value="right click me"]')
        action = ActionChains(self.driver)
        action.click(ele_click)
        action.double_click(ele_doubleClick)
        action.context_click(ele_rightClick)
        sleep(3)
        action.perform()
        sleep(3)
```

4.ActionChains用法2

1）用法二：鼠标移动到某个元素上
- action =ActionChains(self.driver)
- action.move_to_element(element)
- action.perform()


示例：
```python
from time import sleep
import pytest
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

class TestAction():

    def setup(self):
        self.driver = webdriver.Chrome()
        self.driver.implicitly_wait(5)

    def teardown(self):
        self.driver.quit()

    def test_move(self):
        self.driver.get("https://www.baidu.com/")
        self.driver.maximize_window()
        ele = self.driver.find_element_by_id("s-usersetting-top")
        action = ActionChains(self.driver)
        action.move_to_element(ele)
        action.perform()
        sleep(3)
```

5.ActionChains用法3-拖拽

1）拖拽示例1：
```python
from time import sleep
import pytest
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

class TestAction():

    def setup(self):
        self.driver = webdriver.Chrome()
        self.driver.implicitly_wait(5)

    def teardown(self):
        self.driver.quit()

    def test_dragdrop(self):
        self.driver.get("http://sahitest.com/demo/dragDropMooTools.htm")
        self.driver.maximize_window()
        ele_drag = self.driver.find_element_by_id("dragger")
        ele_drag2 = self.driver.find_element_by_xpath('//div[@class="item"]')
        action = ActionChains(self.driver)
        action.drag_and_drop(ele_drag,ele_drag2).perform()
        sleep(3)
```

2）拖拽示例2：
```python
def test_dragdrop(self):
    self.driver.get("http://sahitest.com/demo/dragDropMooTools.htm")
    self.driver.maximize_window()
    ele_drag = self.driver.find_element_by_id("dragger")
    ele_drag2 = self.driver.find_element_by_xpath('//div[@class="item"]')
    action = ActionChains(self.driver)
    action.click_and_hold(ele_drag).release(ele_drag2).perform()
    sleep(3)
```

3）拖拽示例3：
```python
def test_dragdrop(self):
    self.driver.get("http://sahitest.com/demo/dragDropMooTools.htm")
    self.driver.maximize_window()
    ele_drag = self.driver.find_element_by_id("dragger")
    ele_drag2 = self.driver.find_element_by_xpath('//div[@class="item"]')
    action = ActionChains(self.driver)
    action.click_and_hold(ele_drag).move_to_element(ele_drag2).release().perform()
    sleep(3)
```

6.ActionChains用法4

1）用法4：ActionChains模拟按键方法：
模拟按键有多种方法，能用win32 api来实现，能用Sendkeys来实现，也可以用selenium的WebElement对象的send_keys()方法来实现，这里ActionChains类也提供了几个模拟按键的方法。

2）基本用法：
- Action = ActionChains(driver)
- action.send_keys(Keys.BACK_SPACE)
- 或者action.key_down(Keys.CONTROL).send_keys('a').key_up(Keys.CONTROL)
- action.perform()

3）测试案例：
- 打开网址：http://sahitest.com/demo/label.htm
- 定位两个输入框 e1，e2
- 向输入框e1中输入文字：username
- 使用全选，复制，粘贴到输入框e2中

示例1：
```python
在base中写一个通用的类：
from  selenium import webdriver

class Base():
    def setup(self):
        self.driver = webdriver.Chrome()
        self.driver.implicitly_wait(5)
        self.driver.maximize_window()


    def teardown(self):
        self.driver.quit()
```

在执行的类中继承：
```python
from time import sleep

import pytest
from selenium.webdriver import ActionChains
from selenium.webdriver.common.keys import Keys

from selenium_code.base import Base
from selenium import webdriver

class TestActionKeys(Base):
    def test_action_key(self):
        self.driver.get("http://sahitest.com/demo/")
        self.driver.find_element_by_link_text('Label Page').click()
        text1 =self.driver.find_element_by_xpath('/html/body/label[1]/input')
        text2 = self.driver.find_element_by_xpath('/html/body/label[2]/table/tbody/tr/td[2]/input')
        action = ActionChains(self.driver)
        text1.click()
        action.send_keys("AAAA").perform()
        action.key_down(Keys.CONTROL).send_keys('a').key_up(Keys.CONTROL).perform()
        action.key_down(Keys.CONTROL).send_keys('c').key_up(Keys.CONTROL).perform()
        text2.click()
        action.key_down(Keys.CONTROL).send_keys('v').key_up(Keys.CONTROL).perform()
        sleep(3)
```

示例2：
```python
from time import sleep

from selenium.webdriver import ActionChains
from selenium.webdriver.common.keys import Keys

from selenium_code.base import Base

class TestActionKeys02(Base):
    def test_action_keys02(self):
        self.driver.get("http://sahitest.com/demo/")
        self.driver.find_element_by_link_text('Label Page').click()
        text1 = self.driver.find_element_by_xpath('/html/body/label[1]/input')
        action = ActionChains(self.driver)
        text1.click()
        action.send_keys("username").pause(1)
        action.send_keys(Keys.SPACE).pause(1)
        action.send_keys("tom").pause(1)
        action.send_keys(Keys.BACK_SPACE).perform()
        sleep(3)
```

## （三）TouchActions用法
1.介绍：
https://selenium-python.readthedocs.io/api.html#module-selenium.webdriver.common.touch_actions

1）TouchActions类似于ActionChains，ActionChains只是针对PC端程序鼠标模拟的一系列操作，对H5页面操作时，无效的。TouchActions可以对H5页面操作，通过TouchAction可以实现点击，滑动，拖拽，多点触控，以及模拟手势的各种操作。

2）手势控制：
- tap ——在指定元素上敲击
- double_tap——在指定元素上双敲击
- tap_and_hold——在指定元素上点击但不释放
- move——手势移动指定偏移（未释放）
- release——释放手势
- scroll——手势点击并滚动
- scroll_from_element——从某个元素位置开始手势点击并滚动（向下滑动为负数，向上滑动为正数）
- long_press——长按元素
- flick——手势滑动
- flick_element——从某个元素位置开始手势滑动（向上滑动为负数，向上滑动为负数）
- perform——执行
- 
2.TouchAction实例

1）打开Chrome

2）打开URL：http://www.baidu.com

3）向搜索框中输入‘selenium测试’

4）通过TouchAction点击搜索框

5）滑动到底部，点击下一页

6）关闭Chrome

示例：
```python
在base中写一个通用的类：
from  selenium import webdriver

class Base():
    def setup(self):
        self.driver = webdriver.Chrome()
        self.driver.implicitly_wait(5)
        self.driver.maximize_window()


    def teardown(self):
        self.driver.quit()
```

在执行的类中继承：

```python
from time import sleep

from selenium.webdriver import TouchActions

from selenium_code.base import Base


class TestTouchAction(Base):
    def test_touch_action(self):
        self.driver.get("http://www.baidu.com")
        ele=self.driver.find_element_by_id("kw").send_keys('selenium测试')
        ele_click=self.driver.find_element_by_id("su")
        action = TouchActions(self.driver)
        action.tap(ele_click)
        action.perform()
        action.scroll_from_element(ele_click,0,10000).perform()
        sleep(3)
```

注：如果不知道偏移量，就输入一个非常大的值就可以了。
注意：
如果页面报不是标准w3c的问题，可在setup方法中的driver上添加一个option，改为：
```python
def setup(self):
    option = webdriver.ChromeOptions()
    option.add_experimental_option("w3c",False)
    self.driver = webdriver.Chrome(options=option)
```
