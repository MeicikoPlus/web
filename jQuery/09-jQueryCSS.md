# 操作CSS

jQuery 拥有若干进行 CSS 操作的方法：

- addClass() - 向被选元素添加一个或多个类
- removeClass() - 从被选元素删除一个或多个类
- toggleClass() - 对被选元素进行添加/删除类的切换操作
- css() - 设置或返回样式属性



## 设置CSS属性

### 返回css属性

```js
语法：css("propertyname")
```

```js
实例：$("p").css("background-color");
```

### 设置多个css

```js
语法：css({"propertyname":"value","propertyname":"value",...});
```

```js
实例：$("p").css({"background-color":"yellow","font-size":"200%"});
```

