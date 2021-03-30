## canvas

- canvas 图像的渲染有别于 HTML 图像的渲染

  canvas 的渲染极快，不会出现代码覆盖后延迟渲染的问题.写 canvas 代码时要有同步思维。

* 在绘制图形之前需要先判断画笔是否存在，并注意 save()和 restore()方法的使用。

* canvas 画布默认高度为 300 \* 150

  定义高宽一定要使用 HTML 的 attribute 的形式来定义，通过 css 的形式定义会缩放画布内的图像。

* 绘制矩形的问题

  边框宽度问题：边框宽度是在偏移量上下分别渲染一半，可能会出现小数边框，出现小数边框时会向上取整，为了避免出现这种问题，可以设置偏移量为带小数的值。

  canvas 的 API 只支持一种图像的直接渲染。即矩形。

## canvas 基本用法

### 1.什么是 canvas(画布)

```
<canvas> 是 HTML5 新增的元素，可用于通过使用JavaScript中的脚本来绘制图形
例如，它可以用于绘制图形，创建动画。<canvas> 最早由Apple引入WebKit
  我们可以使用<canvas>标签来定义一个canvas元素
         使用<canvas>标签时，建议要成对出现，不要使用闭合的形式。
         canvas元素默认具有高宽
    			width：  300px
    			height：150px
```

### 2.替换内容

```
<canvas>很容易定义一些替代内容。由于某些较老的浏览器（尤其是IE9之前的IE浏览器）
	不支持HTML元素"canvas"，
	但在这些浏览器上你应该要给用户展示些替代内容。
	这非常简单：我们只需要在<canvas>标签中提供替换内容就可以。
		--->支持<canvas>的浏览器将会忽略在容器中包含的内容，并且只是正常渲染canvas。
		--->不支持<canvas>的浏览器会显示代替内容
```

### 3.canvas 标签的两个属性

```
   <canvas> 看起来和 <img> 元素很相像，唯一的不同就是它并没有 src 和 alt 属性。
	实际上，<canvas> 标签只有两个属性—— width和height。这些都是可选的。
	当没有设置宽度和高度的时候，canvas会初始化宽度为300像素和高度为150像素。

	画布的高宽
		html属性设置width height时只影响画布本身不影画布内容
		css属性设置width height时不但会影响画布本身的高宽，
					还会使画布中的内容等比例缩放（缩放参照于画布默认的尺寸）
```

### 4.渲染上下文

```
	<canvas> 元素只是创造了一个固定大小的画布，要想在它上面去绘制内容，需要找到它的渲染上下文

	<canvas> 元素有一个叫做 getContext() 的方法，这个方法是用来获得渲染上下文和它的绘画功能。
	getContext()只有一个参数，上下文的格式
		----->获取方式
			  var canvas = document.getElementById('box');
			  var ctx = canvas.getContext('2d');

		----->检查支持性
			  var canvas = document.getElementById('tutorial');
			  if (canvas.getContext){
					var ctx = canvas.getContext('2d');
				}
```

## canvas 绘制矩形

HTML 中的元素 canvas 只支持一种原生的图形绘制：矩形。所有其他的图形的绘制都至少需要生成一条路径

### 1.绘制矩形

```
canvas提供了三种方法绘制矩形：
		1. 绘制一个填充的矩形（填充色默认为黑色）
             x,y参照的是画布的左上角,且x,y两个偏移量不能加单位
			fillRect(x, y, width, height)
		2. 绘制一个矩形的边框（默认边框为:一像素实心黑色）
			strokeRect(x, y, width, height)
		3. 清除指定矩形区域，让清除部分完全透明。
			clearRect(x, y, width, height)

	x与y指定了在canvas画布上所绘制的矩形的左上角（相对于原点）的坐标。
	width和height设置矩形的尺寸。（存在边框的话，边框会在width上占据一个边框的宽度，height同理）
```

### 2.strokeRect 时，边框像素渲染问题

```
按理渲染出的边框应该是1px的，
canvas在渲染矩形边框时，边框宽度是平均分在偏移位置的两侧。

    context.strokeRect(10,10,50,50)
        :边框会渲染在10.5 和 9.5之间,浏览器是不会让一个像素只用自己的一半的
            相当于边框会渲染在9到11之间

    context.strokeRect(10.5,10.5,50,50)
        :边框会渲染在10到11之间
```

### 3.添加样式和颜色

```
        设置画布的样式和颜色必须在画矩形之前进行设置，若在画矩形之后设置，则不会生效
fillStyle   :设置图形的填充颜色。填充时才生效，画的边框矩形是不会生效的
strokeStyle :设置图形轮廓的颜色。
    默认情况下，线条和填充颜色都是黑色（CSS 颜色值 #000000）
lineWidth : 这个属性设置当前绘线的粗细。属性值必须为正数。
           这个粗细也是上下均分的，即向上取设置的lineWidth属性值的一半，向下取lineWidth属性值的一半
    描述线段宽度的数字。 0、 负数、 Infinity 和 NaN 会被忽略。
    默认值是1.0。

    注意：若通过css为canvas的画布设置宽高，在画矩形时会拉伸矩形的，所以不要通过css来设置画布的宽高，如果需要设置宽高，则使用canvas标签的width和height属性来设置
```

### 4. lineWidth & 覆盖渲染

```
设置了样式以后，在画矩形时，如果出现重叠，后面绘制的矩形会覆盖前面绘制的矩形设置的样式
```

### 5.lineJoin

```
设定线条与线条间接合处的样式（默认是 miter）
round : 圆角
bevel : 斜角
miter : 直角
```

## 绘制路径

图形的基本元素是路径。路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。

### 步骤

```
1.首先，你需要创建路径起始点。
2.然后你使用画图命令去画出路径
3.之后你把路径封闭。
4.一旦路径生成，你就能通过描边或填充路径区域来渲染图形。
```

### 常用 API 解析

```
beginPath()

    新建一条路径，生成之后，图形绘制命令被指向到路径上准备生成路径。

	生成路径的第一步叫做beginPath()。本质上，路径是由很多子路径构成，这些子路径都是在一个列表中，
	所有的子路径（线、弧形、等等）构成图形。而每次这个方法调用之后，列表清空重置，
	然后我们就可以重新绘制新的图形。会清空路径容器

moveTo(x, y)
	将笔触移动到指定的坐标x以及y上
	当canvas初始化或者beginPath()调用后，你通常会使用moveTo()函数设置起点，会抬起画笔

lineTo(x, y)
	将笔触移动到指定的坐标x以及y上
	绘制一条从当前位置到指定x以及y位置的直线。

closePath()
	闭合路径之后图形绘制命令又重新指向到上下文中。
		闭合路径closePath(),不是必需的。这个方法会通过绘制一条从当前点到开始点的直线来闭合图形。
	如果图形是已经闭合了的，即当前点为开始点，该函数什么也不做
		当你调用fill()函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用closePath()函数。
	但是调用stroke()时不会自动闭合，需要调用closePath()函数或者将画笔重新设置为起始点

stroke()
	通过线条来绘制图形轮廓。
	不会自动调用closePath()

fill()
	通过填充路径的内容区域生成实心的图形。
	自动调用closePath()，会自动合并路径，将画的点自动闭合起来并进行填充
```

### 绘制矩形

```
rect(x, y, width, height)
    绘制一个左上角坐标为（x,y），宽高为width以及height的矩形。
    当该方法执行的时候，moveTo()方法自动设置坐标参数（0,0）。
    也就是说，当前笔触自动重置会默认坐标

单纯通过rect设置值并不会绘制矩形，需要在下面设置通过什么形式绘制，例如画线或者填充
```

### lineCap

```
lineCap 是 Canvas 2D API 指定如何绘制每一条线段末端的属性。
        默认值是 butt。
有3个可能的值，分别是：
	butt  :线段末端以方形结束。
	round :线段末端以圆形结束
	square:线段末端以方形结束，但是增加了一个宽度和线段相同，高度是线段厚度一半的矩形区域
```

## save

    save() 是 Canvas 2D API 通过将当前状态放入栈中，保存 canvas 全部状态的方法

```
保存到栈中的绘制状态有下面部分组成：
	当前的变换矩阵。
	当前的剪切区域。
	当前的虚线列表.
	以下属性当前的值：
	             strokeStyle,
				 fillStyle,
				 lineWidth,
				 lineCap,
				 lineJoin...
    栈：先进后出
```

### restore

    restore() 是 Canvas 2D API 通过在绘图状态栈中弹出顶端的状态，将 canvas 恢复到最近的保存状态的方法。
    如果没有保存状态，此方法不做任何改变。

## 绘制曲线

### 绘制圆形

```
arc(x, y, radius, startAngle, endAngle, anticlockwise)
		画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，
		startAngle,endAngle是弧度值
		按照anticlockwise给定的方向（默认为顺时针）来生成。anticlockwise默认为false
		ture：逆时针
		false:顺时针
```

### arcTo

```
arcTo(x1, y1, x2, y2, radius)
 是 Canvas 2D API 根据控制点和半径绘制圆弧路径，使用当前的描点(前一个moveTo或lineTo等函数的止点)。根据当前描点与给定的控制点1连接的直线，和控制点1与控制点2连接的直线，作为使用指定半径的圆的切线，画出两条切线之间的弧线路径。
    根据给定的控制点和半径画一段圆弧,radius 为半径长度

	一般要结合moveTo(x,y)来使用
	  x,y :起始点
	  x1,y1:控制点
      x2,y2:结束点

	不一定会经过起始点和结束点，只是画一段圆弧，然后慢慢靠到起始点、控制点、结束点三个点组成的角上。

	参考文档：https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/arcTo
```

### 二次贝塞尔

```
quadraticCurveTo(cp1x, cp1y, x, y)
    是 Canvas 2D API 新增二次贝塞尔曲线路径的方法。它需要2个点。 第一个点是控制点，第二个点是终点。 起始点是当前路径最新的点，当创建二次贝赛尔曲线之前，可以使用 moveTo() 方法进行改变。

	绘制二次贝塞尔曲线，cp1x,cp1y为一个控制点，x,y为结束点。
	起始点为moveto时指定的点
	起始点和终点是圆弧必须经过的两个点，圆弧的半径没办法指定

	参考链接：https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/quadraticCurveTo
```

### 三次贝塞尔

```
bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)
    是 Canvas 2D API 绘制三次贝赛尔曲线路径的方法。 该方法需要三个点。 第一、第二个点是控制点，第三个点是结束点。起始点是当前路径的最后一个点，绘制贝赛尔曲线前，可以通过调用 moveTo() 进行修改。

	绘制三次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x,y为结束点。
	起始点为moveto时指定的点

	参考链接：https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D/bezierCurveTo
```

## canvas 中的变换

### translate 平移变换

```
translate(x, y)
	我们先介绍 translate 方法，它用来移动 canvas的原点到一个不同的位置。
	translate 方法接受两个参数。x 是左右偏移量，y 是上下偏移量，
	    在canvas中translate是累加的
```

### rotate(angle)旋转

```
这个方法只接受一个参数：旋转的角度(angle)，它是顺时针方向的，以弧度为单位的值。
旋转的中心点始终是 canvas 的原点，如果要改变它，我们需要用到 translate 方法
在canvas中rotate是累加的

需要注意的是：canvas中的同步思维，先平移再旋转和先旋转再平移效果是不一样的

```

### scale(x, y)缩放

```
scale(x, y)
scale 方法接受两个参数。x,y 分别是横轴和纵轴的缩放因子，它们都必须是正值。
值比 1.0 小表示缩小，比 1.0 大则表示放大，值为 1.0 时什么效果都没有。
缩放一般我们用它来增减图形在 canvas 中的像素数目，对形状，位图进行缩小或者放大。

在canvas中scale是累乘的
```
