# 九、selenium处理多浏览器
## （一）不同浏览器兼容性测试
1.chrome\firefox\headless 等浏览器的自动化支持

2.传不同参数来测试不同的浏览器，用例做浏览器兼容性测试

示例：（主要展示base文件中的内容，Windows环境上未能实现，还需找原因）
```python
import os

from  selenium import webdriver

class Base():
    def setup(self):
        browser = os.getenv("browser")
        if browser == 'firefox':
            self.driver =webdriver.Firefox()
        elif browser == 'headless':
            self.driver = webdriver.PhantomJS()
        else:
            self.driver = webdriver.Chrome()

        self.driver.implicitly_wait(5)
        self.driver.maximize_window()


    def teardown(self):
        self.driver.quit()

```

注：
在mac上，可执行：
```
browser=firefox pytest test_selenium/test_hogwarts.py
```

在windows上，需执行：
```
set browser=firefox pytest test_selenium/test_hogwarts.py
（使用该方法也没有实现）
```
