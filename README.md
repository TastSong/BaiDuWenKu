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
  运行之后，你会发现，在pycharm的控制台报错,selenium.common.exceptions.WebDriverException: Message: unknown error:<br>
  Element <span class="moreBtn goBtn">...</span> is not clickable at point (449, 565). Other element would receive<br>
  the click: <div class="content" id="reader-evaluate-content-wrap" data-id="1d03027280eb6294dd886cb7" data-value="-1"<br>
  data-doc-value="0">...</div>,这个错误的意思是不能去点击这个标签，它可以去点击这个div。为什么会这样呢？细心的朋友可能会<br>
  看见上图的右下角有一个箭头，仔细看有一句style属性是,overflow:hidden这句话的意思是隐藏这个标签，所以才导致这个错误的发生。<br>
  selenium的[python api链接](http://selenium-python.readthedocs.io/api.html)，解决办法如下:<br>
  ```python
  if __name__ == "__main__":
    browser = webdriver.Chrome()
    browser.get("https://wenku.baidu.com/view/1d03027280eb6294dd886cb7.html?from=search")
    #找到继续阅读按钮的上一级div,banner-more-btn是div的类名用.,ID用#
    hidden_div = browser.find_element_by_css_selector("#html-reader-go-more")
    #获取阅读按钮
    gotBtn = browser.find_element_by_css_selector("#html-reader-go-more .banner-more-btn")
    actions = webdriver.ActionChains(browser)
    actions.move_to_element(hidden_div)
    actions.click(gotBtn)
    actions.perform()
  ```
  * 在点击继续阅读按钮之前，最好先判断这个按钮是否存在，如果只有1页的时候，是不会有这个按钮的，判断方法，可以用之前的方法进行判断。<br>
    获取文章的所有内容:
    ```python
    time.sleep(3)
    #获取包含内容的div 
    div_text = browser.find_elements_by_class_name("ie-fix")
    for temp in div_text:
        text = temp.text
        print text
    ```
    
  ## 注意
  有可能会因为百度文库的广告导致将继续阅读按钮遮住，致使点击的时候，点击不到继续阅读按钮，<br>
  所以，你需要找到广告的位置（需要先判断广告是否存在，再做处理，否则可能会报错），然后，<br>
  使用让隐藏按钮可以点击的方法处理广告即可。
