# 会动的简历总结


## 简介

该简历是利用基础知识制作，主要涉及的知识点：

1. `pre`标签的使用
   `per`标签会在页面中呈现你写的内容，包括空格与换行。
   `pre`内的标签会被当成`html`解析，利用此给代码高亮。
2. js 多行字符串使用" \` \` "符号
3. 字符串`substring`方法的使用
4. 滚动条自动下拉倒底部：`xxx.scrollTop = xxx.scrollHeight`
5. 异步与回调
6. 两个 js 库：primjs，高亮代码；showdownjs，将 markdown 转化成 html。

## 遇到的问题

- 回调函数作用域问题
  未解决
- 执行函数写在开头遇到的 substring 未定义报错
  变量提升的是声明并不是赋值！

  ```js
  a.substring(0, 2)

  var a = '123456789'

  等价于

  var a

  a.substring(0, 2)

  a = '123456'
  ```

