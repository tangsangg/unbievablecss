### 层叠顺序（stacking level）与堆栈上下文（stacking context）知多少？

```
<div class="container">
    <div class="inline-block">#divA display:inline-block</div>
    <div class="float"> #divB float:left</div>
</div>
```
css
```
.container{
    position:relative;
    background:#ddd;
}
.container > div{
    width:200px;
    height:200px;
}
.float{
    float:left;
    background-color:deeppink;
}
.inline-block{
    display:inline-block;
    background-color:yellowgreen;
    margin-left:-100px;
}

```
![](https://camo.githubusercontent.com/79a1aa1dc3ac671b2fe08ce35ab8b464a599c3b5/687474703a2f2f696d616765732e636e626c6f67732e636f6d2f636e626c6f67735f636f6d2f636f636f31732f3838313631342f6f5f737461636b696e676c6576656c2e706e67)

1. the background and borders of the element forming the stacking context.
2. the child stacking contexts with negative stack levels (most negative first).
3. the in-flow, non-inline-level, non-positioned descendants.
4. the non-positioned floats.
5. the in-flow, inline-level, non-positioned descendants, including inline tables and inline blocks.
6. the child stacking contexts with stack level 0 and the positioned descendants with stack level 0.
7. the child stacking contexts with positive stack levels (least positive first).
###翻译
1. 形成堆叠上下文环境的元素的背景与边框
2. 拥有负 z-index 的子堆叠上下文元素 （负的越高越堆叠层级越低）
3. 正常流式布局，非 inline-block，无 position 定位（static除外）的子元素
4. 无 position 定位（static除外）的 float 浮动元素
5. 正常流式布局， inline-block元素，无 position 定位（static除外）的子元素（包括 display:table 和 display:inline ）
6. 拥有 z-index:0 的子堆叠上下文元素
7. 拥有正 z-index: 的子堆叠上下文元素（正的越低越堆叠层级越低）

###触发堆叠上下文

1. 那么，如何触发一个元素形成 堆叠上下文 ？方法如下，摘自 MDN：
根元素 (HTML),
1. z-index 值不为 "auto"的 绝对/相对定位，
1.  一个 z-index 值不为 "auto"的 flex 项目 (flex item)，即：父元素 display: flex|inline-flex，
1.  opacity 属性值小于 1 的元素（参考 the specification for opacity），
transform 属性值不为 "none"的元素，
1.  mix-blend-mode 属性值不为 "normal"的元素，
1.  filter值不为“none”的元素，
1.  perspective值不为“none”的元素，
1.  solation 属性被设置为 "isolate"的元素，
1. position: fixed
1. 在 will-change 中指定了任意 CSS 属性，即便你没有直接指定这些属性的值
-webkit-overflow-scrolling 属性被设置 "touch"的元素