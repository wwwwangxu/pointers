1. CSS3 新增伪类
  * p:first-of-type / p:nth-child(2) / :enabled :disabled 表单控件的禁用状态 / :checked 单选框或复选框被选中

2. CSS3 新特性
  * GRBA 和 透明度
  * background-image / background-origin / background-size / background-repeat
  * word-wrap (对长的不可分割的单词换行)
  * text-shadow / box-shadow
  * font-face
  * border-radius
  * 媒体查询

3. 为什么要初始化 CSS 样式？
  * 因为浏览器的兼容性问题，不初始化会导致不同浏览器之间的页面显示存在差异
  * 不同浏览器的标签默认 margin 和 padding 不一样

4. 如何计算被设置为绝对定位的元素的包含块？
  * 离其最近的position值不为static的祖先元素的padding box

5. 在不同浏览器下，`visibility: collapse;` 的区别
  * 在chrome中，使用collapse和使用hidden没有区别
  * firefox, opera 和 IE 中，使用collapse和使用display:none 没有什么区别

6. 设置了 `position: absolute/fixed;`的元素，float属性会失效，display值需要调整
7. 触发 BFC 的方式
  * float / overflow: hidden; / display: flow-root/inline-block/table-cell; / postition: absolute/fixed;

8. 浮动元素不在文档流中，所以文档流的块框表现得就像浮动框不存在
9. 行内元素设置浮动后会变成块状元素
10. 移动端布局使用媒体查询
  *  `<link rel="stylesheet" type="text/css" href="xxx.css" media="only screen and (max-device-width:480px)">`
  * `@media only screen and (max-device-width:480px){/css样式/}`

11. 响应式设计：一个网站能够兼容不同设备；原理：通过媒体查询检测设备屏幕尺寸做出相应处理
12. 伪元素：不存在与 DOM 之中，只存在于页面之中
13. 怎样让 Chrome支持小于12px 的文字？ `p{font-size: 10px; transform: scale(0.8);}`
14. 动画刷新的最短时间间隔：(1 / 60) * 1000 = 16.7ms

