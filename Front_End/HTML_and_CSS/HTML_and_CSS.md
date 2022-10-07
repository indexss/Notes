# HTML

1. 标签分类

​		<html> </html>

​		双标签

​		<br />		单标签

​		包含关系 并列关系

2. 语义标签

​		标题标签

​		段落标签

3. 标题标签

		<h1> - <h6> head 当作标题使用

4. 换行标签

​		<br />

​		段落之间会有段垂直间距，但br换行没有

5. 文本格式化标签

| 标签                         | 语义   | 说明                 |
| ---------------------------- | ------ | -------------------- |
| <strong></strong>或者<b></b> | 加粗   | 用strong语义更加强烈 |
| <em></em>或者<i></i>         | 倾斜   | em语义更加强烈       |
| <del></del>或者<s></s>       | 删除线 | 推荐使用del          |
| <ins></ins>或者<u></u>       | 下划线 | 更推荐使用           |

6. 无语义标签

​		<div></div> 竖盒子 没有语义 一个标签单独占一行<span></span>横盒子

7. 图像标签和路径

```html
<img src="图像url" /> 单标签
```

| 属性   | 属性值   | 说明                                 |
| ------ | -------- | ------------------------------------ |
| src    | 图片路径 | 图片路径                             |
| alt    | 文本     | 替换文本，图片如果不能显示显示的文字 |
| title  | 文本     | 提示文本，鼠标放在图像上显示的文字   |
| width  | 像素     | 设置图像的宽度 左右宽度              |
| height | 像素     | 设置图像的高度 上下高度              |
| border | 像素     | 设置图像的边框粗细                   |

​		注意：使用键值对连接属性和属性值。 中间用等号连接。

8. 目录

​		目录文件夹和根目录	第一层就是根目录	再进入一层就不是根目录

9. 路径

​		相对路径和绝对路径 同级./ 上一级 ../ 下一级/

10. 超链接标签 ->外部

```html
<a href="链接地址" target="目标窗口弹出方式 _self为默认值 _blank新窗口"></a>
```

11. 内部链接 直接跳网页 若为空链接 改为href="#"

```html
<a href="网页目录"></a>
```

12. 下载链接

​		如果href里面地址是一个文件或者压缩包，会下载这个文件

13. 元素链接

```html
<a><img src="img.jpg"/></a>
```

14. 锚点链接

```html
<a href="#名字">锚点定位</a>
<h3 id="名字"> 定位点不需要打#
```

15. 注释标签

    <!-- -->

16. 特殊字符

| 特殊字符 | 对应代码  |
| -------- | --------- |
|          | &nbsp ;   |
| <        | &lt ;     |
| >        | &gt ;     |
| &        | &amp ;    |
| ¥        | &yen ;    |
| ©️        | &copy ;   |
| ®️        | &reg ;    |
| 度       | &deg ;    |
| 正负     | &plusmn ; |
| ✖️        | &times ;  |
| ➗        | &divide ; |
| 平方     | &sup2 ;   |
| 立方     | &sup3 ;   |

17.  表格

```html
<table>
  <tr>
    <th>表头<th/>
  <tr/>
  <tr> 行
    <td>文字</td> 单元格
  <tr/>
<table/>
表格属性 不常用 因为可以用css描述
```

|    属性     |      属性值       | 描述                                              |
| :---------: | :---------------: | :------------------------------------------------ |
|    align    | left center right | 规定表格相对周围元素的对齐方式                    |
|   border    |     1 or " "      | 规定表格单元是否拥有边框，默认为“”，表示没有边框  |
| cellpadding |      像素值       | 规定单元边沿与其内容之间的空白 格内空白 默认为1px |
| cellspacing |      像素值       | 规定单元格之间的空白 格外空白 默认为2px           |
|    width    | 像素值或者百分比  | 规定表格的宽度                                    |
|   bgcolor   |     背景颜色      | 背景颜色                                          |
|   height    |      像素值       | 规定表格的高度                                    |

表格结构标签

```html
<thead> 表头区域 <tbody>表中区域
```

合并单元格

```html
跨行合并 竖着合并 rowspan="合并单元格的个数"
跨列合并 横着合并 colspan="合并单元格的个数"
合并代码写到目标单元格里 也就是最上，最左的那个格子里，写到属性里。
```

18. 列表标签  更整齐

    1. 无序列表（重要）

       ```html
       <ul>
         <li>内容1</li>
         <li>内容2</li>
       </ul>
       ul里面只能放li
       li里面可以放任何东西
       ```

    2. 有序列表

       ```html
       <ol>
         <li>内容1</li>
         <li>内容2</li>
       </ol>
       ```

    3. 自定义列表

       ```html
       <dl>
         <dt>该项名字</dt>
         <dd>该项下设内容</dd>
       </dl>
       ```

19. 表单标签 **所有表单都需要在表单域中！**

      1. 表单域**<form> **

         把表单内容提交给服务器中

         ```html
         <form action="url" method="提交方式" name="表单域名称"> 
           各种表单元素控件
         </form>
         ```

         | 属性   | 属性值     | 说明                                               |
         | ------ | ---------- | -------------------------------------------------- |
         | action | url地址    | 用于指定接受并处理表单数据的服务器程序的url地址    |
         | method | GET / POST | 用于设置表单数据的提交方式                         |
         | name   | 名称       | 用于指定表单的名称，以区分同一个页面中的多个表单域 |

         

      2. 表单控件

         1. input 输入表单元素

            ```html
            <input type="属性值" />
            ```

            | type属性值 | 描述                                          |
            | ---------- | --------------------------------------------- |
            | button     | 定义课点击按钮(用于通过js启动脚本)            |
            | checkbox   | 定义复选框                                    |
            | file       | 定义输入字段和浏览按钮，供文件上传            |
            | hidden     | 定义隐藏的输入字段                            |
            | image      | 定义图像形式的提交按钮                        |
            | password   | 定义密码字段，该输入字段的字符杯掩码          |
            | radio      | 定义单选按钮                                  |
            | reset      | 定义重置按钮 重置按钮会清楚表单中的所有数据   |
            | submit     | 定义提交按钮 提交按钮会把表单数据发送到服务器 |
            | label      | 文字绑定榜单元素                              |

            input还有其他属性

            | input属性   | 属性值       | 描述                                               |
            | ----------- | ------------ | -------------------------------------------------- |
            | name        | 由用户自定义 | 定义input元素的名称 实现单选                       |
            | value       | 由用户自定义 | 规定input元素的值 加载时有的值 框里有的 按钮的名字 |
            | checked     | checked      | 规定此input元素首次加载时应当被选中                |
            | maxlength   | 正整数       | 规定输入字段中的字符的最大长度                     |
            | placeholder | 由用户自定义 | 灰色的字 在搜索框中                                |

            label标签用法 for对应id

            ```html
            <label for="sex">男</label>
            <input type="radio" name="sex" id="sex" />
            ```

            

         2. select 下拉表单元素

            ```html
            <select>
              <option>选项1</option>
              <option selected = "selected">选项2</option>
              ......
            </select>
            option标签加selected表示默认选中
            ```

            

         3. textarea 文本域元素

            输入内容比较多的时候，可以使用，用于留言板 评论之类的。

            ```html
            <textarea cols="" rows="">
              文本内容
            </textarea>
            ```

20. 查阅文档

    MDN

# CSS

1. 用于美化网页

   html局限性：只关注内容，太丑了

   CSS可以美化 Cascading Style Sheets

   CSS也是一种标记语言 让布局更简单

   ```css
   p{
   	color : red;
     font_size : 12px;
   }
   ```

2. 选择器

     1. 基础选择器

        1. 标签选择器

           ```css
           p{
             color: red;
           }
           ```

           

        2. 类选择器

           ```css
           .className{
             color: red;
           }
           <div class="className">hello world</div>
           ```

           ***一个标签可以指定多类名   class可以被多人使用*** 

           ```css
           <div class="red font"></div>
           写到一个class属性里面 中间用空格分开
           ```

           

        3. id选择器

           ```css
           #idName{
           	color: red;
           }
           <div id="idName">hello world</div>
           ```

           ***id只能被调用一次 如果有人调用过，那么不能再被调用***

           

        		4. 通配符选择器 选择所有的元素

             ```css
             *{
             	color: red;
             }
             ```

             ***一般特殊情况才会使用，比如说清楚所有元素标签的内外边距***

             ```css
             * {
               margin: 0;
               padding: 0;
             }
             ```

3. 字体属性

   1. font-family 文字字体

      在style之前引入link

      然后在style中进行应用

   2. font-size 文字大小

   3. font-weight 文字粗细 normal bold bolder lighter 数字 700为bold 400为正常

   4. font-style 文字样式 normal italic(斜体)

   5. 复合属性 **s**tyle **w**eight **s**ize **f**amily      ***swsf*** 稍微舒服 必须保留最后两项

      ```css
      body{
        font: font-style font-weight font-size/line-hight font-family;
      }
      ```

4. 文本属性

   1. color 文本颜色

   | 表示         | 属性值                          |
   | ------------ | ------------------------------- |
   | 预定义的颜色 | red green blue                  |
   | 十六进制     | #FF0000  #FF6600                |
   | RGB代码      | rgb(255,0,0)    rgb(100%,0%,0%) |

   2. text-align 文本对齐

      left right center. ***想让图片居中对齐 需要让他的上级父标签text-align: center***

   3. text-decoration 文本装饰

      - none 
      - underline
      - overline
      - through

   4. text-indent 文本缩进

​				text-indent: 10px或者2em。 em就是一个字	

​		 5.line-hight
​				行高氛围上间距 文本高度 下间距

5. CSS引入方式

   1. 行内样式表

      ```html
      <p style="color: red; font-size: 12px;"></p>
      ```

      

   2. 内部样式表

      ```html
      <style>
        div{
          color: red;
        }
      </style>
      ```

      

   3. 外部样式表

      ```html
      <head>
      	<link rel="stylesheet" href="文件地址">
      </head>
      ```

      

6. Emmet语法
   1. 快速生成html
      - 标签名+tab
      - diiv*10 生成10个div
      - 父子级 ul>li + tab
      - 兄弟级 div+p + tab
      - .nav +tab直接生成<div class="nav"></div>
      - p.nav + tab也可以直接生成
      - 生成类名有顺序 .demo$*5
      - 生成标签自带文字 div{文字}
   2. 快速生成css
      - tac -> text-align: center
      - ti -> text-indent
      - w-> width
      - h -> height
      - w100 -> w: 100px
      - ti2em
   3. 快速格式化： shift + option + f

7. 复合选择器

   1. 后代选择器(多)

      元素1 元素2 元素3 .....{ 样式声明 }

      所有基础选择器都可以混着用

      .class li{ 样式声明 } 这种也可以

   2. 子元素选择器（少）

      **选择最近的儿子**（嫡长子）

      元素1 > 元素2 { 样式声明 }

   3. 并集选择器 **一般竖着写， 最后一个选择器不应该加逗号**（多）

      可以选择多组标签 选择使用相同样式

      元素1, 元素2{ 样式声明 } 也可以用后代选择器作为子集

   4. 链接伪类选择器（多）

      **用于向某些选择器添加特殊的效果，比如给链接添加特殊效果，或选择第一个，或者第n个元素** 

      :hover(鼠标经过的时候) :first-child

      ***链接伪类选择器为确保生效 要按LVHA的顺序往下写 一般先给a样式 再给ahover一个样式***

      | 选择器    | 用法                                   |
      | --------- | -------------------------------------- |
      | a:link    | 选择所有未被访问的链接                 |
      | a:visited | 选择所有已经被访问的链接               |
      | a:hover   | 选择鼠标指针位于其上的链接（鼠标经过） |
      | a:active  | 选择活动链接（鼠标按下未弹起的链接）   |

   5. focus伪类选择器（少）

      获得选择的表单的样式

      input:focus{

      }

8. 元素显示模式

   1. 块级元素

      典例：h1-h6, p, div, ul, ol, li. **其中div是最典型的**

      特点：1. 独占一行 

      ​			2.可以控制高度 宽度 外边距 内边距

      ​			3.宽度默认是容器（父级宽度）的100%

      ​			4.是一个容器以及盒子，里面可以放入行内活着块级元素

   2. 行内元素

      典例：a, strong, b, em, i, del, s, ins, u, span **其中span是最经典的**

      行内元素也称内联元素

      特点：1. 相邻行内元素在一行上，一行可以显示多个。

      ​			2.高，宽直接设置是无效的

      ​			3.默认宽度就是它本身内容的宽度

      ​			4.行内元素只能容纳文本或者其他行内元素

      注意：链接里面不能再放链接

      ​			特殊情况a里面可以放块级元素但是 给a转换一下块级模式最安全	

      行内块元素：

      ​	在行内元素中有几个特殊的标签 如**img input td**他们同时具有块元素和行内元素的特点

      ​	民间叫法为行内块元素。一行可以放多个 中间有空白 但是可以调尺寸 

      ​	特点：1. 和相邻行内元素 在一行上 但是它们之间会有空白缝隙 一行					可以显示多个 

      ​				2.默认宽度是它内容本身的宽度

      ​				3.高度，行高，外边句以及内边距都可以控制。

   3. 元素显示模式转换

      比如想要增加a的触发范围

      - 行元素转换为块元素 **display: block;**
      - 块级元素改为行元素 **display: inline;**
      - 转换为行内块 **display: inline-block;**

9. 单行文字 垂直居中

   方法: 行高 = 盒子高度

   原理: 行高 = 文字本身高度 + 上间距 + 下间距。上间距 = 下间距

   ​		 盒子高度 = 盒子在垂直方向的长度 

   ​		 当行高 = 盒子高度时，上下空隙正好平分空白地方，文字永远夹在中间。

10. 背景

    1. 背景颜色

       background-color : transparent / color

    2. 背景图片 **优点：便于控制位置。默认背景图片平铺**

       background-image : none / url()

    3. 背景平铺

       background-repeat : repeat / no-repeat / repeat-x / repeat-y

    4. 背景图片位置

       background-position: x y;

       可以用方位名词 也可以用精确单位

       | 参考值   | 说明                                                         |
       | -------- | ------------------------------------------------------------ |
       | length   | 百分数 \| 由浮点数子和单位标识符组成的长度值                 |
       | position | top \| center \| bottom \| left \| center \| right 方位名词 方位名词顺序无关 |

       

    5. 背景图像固定
    
       background-position: x y;
    
    6. 背景附着
    
       background-attachment 可以做 ***视差滚动*** 效果
    
       可取值： scroll | fixed
    
    7. 背景混合属性
    
       background: 颜色 地址 平铺 滚动 位置 (没有顺序要求)
    
    8. 背景色半透明
    
       background: rgba(r, g, b, alpha) ;

​		