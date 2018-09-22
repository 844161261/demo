## 背景和边框
***
***
## 多重边框
### 盒模型提供的border只有一层，如果需要实现多层边框的效果，我们往往通过使用额外的DOM元素来实现。
![image](img/multi_border.jpg)
### 实现要点： box-shadow: h-shadow v-shadow 模糊距离 阴影尺寸 color inset; 
    
html
```
<div></div>
```
css
```
width: 100px;
height: 60px;
margin: 25px; // box-shadow本身不占位，需要让位
background: yellowgreen;
box-shadow: 0 0 0 10px #655,
		0 0 0 15px deeppink,
		0 0 0 20px blue; // 可以设置多值
```  
***
## 灵活的背景定位
### 我们常用的背景定位方式要么指定左上角的偏移量，要么干脆与其他三个角靠齐，如果想要实现距离右下角有一定距离的偏移量，同时满足适应性要求，可以这么做：
![image](img/bg_position.png)
### 实现要点： background: right 20px bottom 10px; 偏移量前指定关键值；

html
```
<div></div>
```
css
```
background: url(http://csssecrets.io/images/code-pirate.svg)
	            no-repeat bottom right #58a;
	background-position: right 20px bottom 10px;// 关键字加偏移量

	/* Styling */
	max-width: 10em;
	min-height: 5em;
	padding: 10px;
	color: white;
	font: 100%/1 sans-serif;
```
***
## 条纹背景
### 条纹图案无处不在，通常我们在处理的过程中，会创建一个单独的位图，然后去调整修改它，或者使用svg，但这样还是一个独立的文件。以下是几种简单的条纹效果  
### 1、水平/垂直条纹 
![image](img/bar_repeat.png)  
### 实现要点: background: linear-gradient(color1 point, color2 point); 当两个色标的值相等时候，渐变效果可以忽略不计（当后者的色标小于前者时候，后者色标等于前者色标最大值）
html
```
<div></div>
```
css
```
background: linear-gradient(#fb3 33.3%, yellowgreen 0, yellowgreen 66% ,#58a 0);
background-size: 100% 30px;
``` 
### 2、斜向条纹
![image](img/WX20180630-200324@2x.png)  

CSS
```
background: repeating-linear-gradient(45deg, #fb3, #fb3 15px,#58a 0, #58a 30px);
```
***
## 连续的图像边框
CSS的border-img属性出来的效果并不能满足绝对大部分场景，针对对图片内容有要求的时候，就显得差强人意。可以用双背景图的方法去模拟  
![image](img/WX20180630-201348@2x.png)
CSS
```
border: 1em solid transparent;
// 第一个白色渐变是做内层背景，最后的斜杆是background缩写。
// 第二个背景图才是当做是border-img
// padding-box是避免出现内层把外层背景图覆盖了。
background: linear-gradient(white, white) padding-box,
			url(http://csssecrets.io/images/stone-art.jpg) border-box  0 / cover;

/* Styling & enable resize */
width: 21em;
padding: 1em;
overflow: hidden;
resize: both;
font: 100%/1.6 Baskerville, Palatino, serif;
```
***
## 图像边框和条纹的综合引用

### 条纹效果的应用一 信封
同样利用背景图渐变的特性，我们可以制作出信封效果。
![image](img/WX20180630-192937@2x.png)
html

```
<div>Lorem ipsum dolor, sit amet consectetur adipisicing elit. Obcaecati esse autem ad explicabo nobis eum, aut quis facilis, dolorem laudantium alias voluptas voluptate blanditiis. Quis, eaque fuga? Hic, inventore fugit?
</div>
```
css
```
padding: 1em;
border: 1em solid transparent;
// background-size: 6em 6em 是根据想要的渐变宽度勾股定理四舍五入计算出的
background: linear-gradient(white, white) padding-box, repeating-linear-gradient(-45deg, red 0, red 12.5%, transparent 0, transparent 25%, #58a 0, #58a 37.5%, transparent 0, transparent 50%) 0 / 6em 6em;
max-width: 20em;
font: 100%/1.6 Baskerville, Palatino, serif;
```
***
### 条纹效果的应用二 信封效果的衍生：蚂蚁行军
将渐变宽度缩小为1px,再给background-position添加一个动画 
![image](img/WX20180630-202444@2x.png)
CSS
```
@keyframes ants { to { background-position: 100% 100% } }
div {
	padding: 1em;
	border: 1px solid transparent;
	background: linear-gradient(white, white) padding-box,
	            repeating-linear-gradient(-45deg, black 0, black 25%, transparent 0, transparent 50%) 0 / .6em .6em;
	animation: ants 12s linear infinite;
	max-width: 20em;
	font: 100%/1.6 Baskerville, Palatino, serif;
}
```
***
## 形状
***
***
### 平行四边形
![image](img/WX20180702-224232@2x.png)
1、普通做法： skewX属性。  缺点是导致多一层html结构
html  
```
// 多嵌套了一层是因为skewX会使得内容也倾斜了。内层把他反向过来
<a href="#yolo" class="button"><div>Click me</div></a>
<button class="button"><div>Click me</div></button>
```
css
```
.button { transform: skewX(45deg); }
.button > div { transform: skewX(-45deg); } // 反向。

.button {
	display: inline-block;
	padding: .5em 1em;
	border: 0; margin: .5em;
	background: #58a;
	color: white;
	text-transform: uppercase;
	text-decoration: none;
	font: bold 200%/1 sans-serif;
}
```  
2、伪元素的方案  
html  
```
<a href="#yolo" class="button"></a>
```
css
```
.button {
	position: relative;
	display: inline-block;
	padding: .5em 1em;
	border: 0; margin: .5em;
	background: transparent;
	color: white;
	text-transform: uppercase;
	text-decoration: none;
	font: bold 200%/1 sans-serif;
}

.button::before {
	content: ''; /* To generate the box */
	position: absolute;
	top: 0; right: 0; bottom: 0; left: 0;
	z-index: -1;
	background: #58a;
	transform: skew(45deg);
}
```
***
### 菱形
基本思路：通过父容器的边框去拆切内层图片。  

<img src = 'img/WX20180702-224807@2x.png' width='200' />  

html  
```
<div class="diamond">
	<img src="http://csssecrets.io/images/adamcatlace.jpg" />
</div>
```
css
```
.diamond {
	width: 250px;
	height: 250px;
	transform: rotate(45deg);
	overflow: hidden;
	margin: 100px;
}

.diamond img {
	max-width: 142%; // 这里的图片宽度要足够大。否则会是八边形
	transform: rotate(-45deg) scale(1.42);
	z-index: -1;
	position: relative;
}
```
<img src = 'img/WX20180702-224900@2x.png' width='200' />  
max-width = 100% 时候的效果，因为图片没有被拆切完全。

***
### 切角效果
<img src = 'img/WX20180702-230548@2x.png' width='200' />   

  通过渐变实现  
html	
```
<div>Hey, focus! You’re supposed to be looking at my corners, not reading my text. The text is just placeholder!</div>
```
css
```
div {
	background: #58a;
	background: linear-gradient(135deg, transparent 15px, #58a 0) top left,
	            linear-gradient(-135deg, transparent 15px, #58a 0) top right,
	            linear-gradient(-45deg, transparent 15px, #58a 0) bottom right,
	            linear-gradient(45deg, transparent 15px, #58a 0) bottom left;
	background-size: 50% 50%;
	background-repeat: no-repeat;
	
	padding: 1em 1.2em;
	max-width: 12em;
	color: white;
	font: 150%/1.6 Baskerville, Palatino, serif;
}
```
***
### 弧形切角
<img src = 'img/WX20180702-231134@2x.png' width='200' />   

同理，使用径向渐变实现弧形切角  
css
```
div {
	background: #58a;
	background:	radial-gradient(circle at top left, transparent 15px, #58a 0) top left,
	            radial-gradient(circle at top right, transparent 15px, #58a 0) top right,
	            radial-gradient(circle at bottom right, transparent 15px, #58a 0) bottom right,
	            radial-gradient(circle at bottom left, transparent 15px, #58a 0) bottom left;
	background-size: 50% 50%; // 这个是有效的核心。。。
	background-repeat: no-repeat;
	
	padding: 1em 1.2em;
	max-width: 12em;
	color: white;
	font: 130%/1.6 Baskerville, Palatino, serif;
}
```
***

### 梯形


## 字体排印
***
### 连字符换行
<img src = 'img/WX20180703-225120@2x.png' width='200' />   

CSS
```
text-align: justify;
hyphens: auto;
```
hyphens兼容性不是很好
***
### 插入换行  
这是一个非常常见的结构形式，非常值得参考，html使用dl标签，语义化。  

<img src = 'img/WX20180703-225700@2x.png' width='400' />   

html
```
<dl>
	<dt>Name:</dt>
	<dd>Lea Verou</dd>
	
	<dt>Email:</dt>
	<dd>lea@verou.me</dd>
	<dd>leaverou@mit.edu</dd>
	
	<dt>Location:</dt>
	<dd>Earth</dd>
</dl>
```
CSS
```
dt, dd {
	display: inline;
	margin: 0;
}

dd {
	font-weight: 600;
}

dd + dt::before { // 在最后一个dd上生效
	content: "\A"; // \A是换行的unicode
	white-space: pre; // 换行符在html会显示成空格，这里使用pre来保留换行
}

dd + dd::before {
	content: ', '; 
	font-weight: normal;
	margin-left: -.25em;
}

body {
	font: 150%/1.6 Baskerville, Palatino, serif;
}
```
***
### 自定义下划线  

<img src = 'img/WX20180703-230727@2x.png' width='400' />  

css自带的text-decoration:underline属性出来的效果不能适用大部分场景，而且下划线与文字底部之间的间距不够灵活。那有什么办法呢。。。。。没错。。又是渐变。
先看效果  
<img src = 'img/WX20180703-230853@2x.png' width='400' />  

html
```
<p>“The only way to <a>get rid of a temp­ta­tion</a> is to <a>yield</a> to it.”</p>
<p>“The only way to <a>get rid of a temp­ta­tion</a> is to <a>yield</a> to it.”</p>
```
css
```
a {
	background: linear-gradient(gray, gray) no-repeat; // 实线
	background-size: 100% 1px;
	background-position: 0 1.02em;
	text-shadow: .05em 0 white, -.05em 0 white; // 这里是为了用出一个遮罩，防止下划线挡住文字
}

p:nth-child(2) a {
	background: linear-gradient(90deg, gray 66%, transparent 0) repeat-x; // 虚线效果
	background-size: .2em 2px;
	background-position: 0 1em;
}
```
***
## 结构与布局
***
***
## 过渡与动画
***
***
## 视觉效果
***







