# BaiDuWenKu

## 环境配置
* IDE pycharm
* python3.x
* Anaconda3

## 代码分析
* 分析文章的页面结构，以此[文章](https://wenku.baidu.com/view/1d03027280eb6294dd886cb7.html?from=search)为例
* 通过网址我们可以观察到，打开文章链接之后，可能有的文章显示不全需要点击`继续阅读`按钮之后，才能看到所有的内容<br>
  ```python
  if __name__ == "__main__":
    browser = webdriver.Chrome()
    browser.get("https://wenku.baidu.com/view/1d03027280eb6294dd886cb7.html?from=search")
    #获取点击继续阅读按钮
    goBtn = browser.find_element_by_class_name("goBtn")
    goBtn.click()
  ```
