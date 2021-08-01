appium原生定位方法：

```
driver.find_element_by_android_uiautomator('new UiSelector().text("手机号")')
# 包含text文字
driver.find_element_by_android_uiautomator('new UiSelector().textContains("机")')
# 以text什么开始
driver.find_element_by_android_uiautomator('new UiSelector().textStartsWith("手")')
# 正则匹配text
driver.find_element_by_android_uiautomator('new UiSelector().textMatches("^手.*")') 
# className
driver.find_elements_by_android_uiautomator('new UiSelector().className("android.widget.TextView")')
# classNameMatches
driver.find_elements_by_android_uiautomator('new UiSelector().classNameMatches("^android.widget.*")') 
# resource-id、resourceIdMatches    类似我们html id 这个可能重复，
driver.find_element_by_android_uiautomator('new UiSelector().resourceId("com.syqy.wecash:id/et_content")') # description driver.find_element_by_android_uiautomator('new UiSelector().description("S 日历")') # descriptionStartsWith driver.find_element_by_android_uiautomator('new UiSelector().descriptionStartsWith("日历")') # descriptionMatches driver.find_element_by_android_uiautomator('new UiSelector().descriptionMatches(".*历$")') 
#组合定位
self.driver.find_element_by_android_uiautomator('new UiSelector().resourceId("com.xueqiu.android:id/tab_name").text("我的")').click() 
#父子关系定位
self.driver.find_element_by_android_uiautomator('new UiSelector().resourceId("com.xueqiu.android:id/title_container").childSelector(text("股票"))')
#兄弟关系定位
self.driver.find_element_by_android_uiautomator('new UiSelector().resourceId("com.xueqiu.android:id/title_container").fromParent(text("股票"))')
#滚动查找
self.driver.find_element_by_android_uiautomator('new UiScrollable(new UiSelector().scrollable(true).instance(0)).scrollIntoView(new UiSelector().text("查找的元素文本").instance(0));')
```

