---
title: css学习分享杂记
categories: css
tags: css sass 规范 flex 布局
---
### css与sass
#### scss我常用的两种转码方式
1. [koala是一个前端预处理器语言图形编译工具，支持Less、Sass、Compass、CoffeeScript，帮助web开发者更高效地使用它们进行开发。跨平台运行，完美兼容windows、linux、mac。](http://koala-app.com/index-zh.html)  
2. gulp  

	    gulp.task('sass', function () {
	      return gulp.src('./sass/*.scss')
	        .pipe(sass.sync().on('error', sass.logError))
	        .pipe(gulp.dest('./css'));
	    });
	    gulp.task('sassWatch', function () {
	        gulp.watch('./sass/*.scss', ['sass']);
	    });

#### scss一些常见用法
直接看`xiaolu\dev\m.xiaoluluanzhuang.com\html\crm\sass\common.scss`  
应该是`@import "reset"`

#### 提升css选择器性能
> 提问：假设页面的ul很多，`#nav ul li #id {...}`与`ul li #id {...}`这两种选择器哪种效率高，即哪种能最快的找出我们指定的元素

为了保证渲染速度，尽量少使用层级关系，sass嵌套在三层以内。  

#### 属性书写顺序：
同一 rule set 下的属性在书写时，应按功能进行分组，并以 Formatting Model（布局方式、位置） > Box Model（尺寸） > Typographic（文本相关） > Visual（视觉效果） 的顺序书写，以提高代码的可读性。

1. Positioning 相关属性包括：`position / top / right / bottom / left / float / display / overflow `等
2. Box model  相关属性包括：`border / margin / padding / width / height `等
3. Typographic  相关属性包括：`font / line-height / text-align / word-wrap `等
4. Visual  相关属性包括：`background / color / transition / list-style `等

另外，如果包含 `content` 属性，应放在最前面。

#### 书写规范

- 属性定义必须另起一行，不要以行为单位
- 在可以使用缩写的情况下，尽量使用属性缩写。
- 当数值为 0 - 1 之间的小数时，省略整数部分的 0。
- url() 函数中的路径不加引号。
- 长度为 0 时须省略单位。 (也只有长度单位可省)
- ...

[前端开发规范手册](http://zhibimo.com/read/Ashu/front-end-style-guide/css/general.html)

#### 浏览器默认属性重置  


	* {
	    -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
	}
	a {
	    text-decoration: none;
	}

input修改border-color的栗子

#### 浏览器兼容  

	div {
	    transform:rotate(7deg);
	    -ms-transform:rotate(7deg); 	/* IE 9 */
	    -moz-transform:rotate(7deg); 	/* Firefox */
	    -webkit-transform:rotate(7deg); /* Safari 和 Chrome */
	    -o-transform:rotate(7deg); 	/* Opera */
	}


推荐gulp插件autoFx  

	gulp.task('autoFx', function () {
	    gulp.src(['./css/*.css'])
	        .pipe(autoprefixer({
	            browsers: ['last 2 versions', 'Android >= 4.0', 'ie >= 7'],
	            cascade: true, //是否美化属性值 默认：true 像这样：
	            //-webkit-transform: rotate(45deg);
	            //        transform: rotate(45deg);
	            remove:true //是否去掉不必要的前缀 默认：true
	        }))
	        .pipe(gulp.dest('./dist'));
	});

---
### flex，百分比和浮动的选择

#### flex  
有两跟轴线：主轴(main axis)和交叉轴(cross axis)  
##### 属性

	flex-direction
	flex-wrap
	flex-flow
	justify-content
	align-items
	align-content

- flex-direction设置主轴方向。  
- flex-wrap设置是否所有项目排在一条线上  
- flex-flow是上述两个属性的简写形式  
- justify-content定义项目在主轴上的对齐方式  
- align-items定义项目在交叉轴上的对齐方式  
- align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。 

##### 项目属性

	order
	flex-grow
	flex-shrink
	flex-basis
	flex
	align-self

- order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。  
- flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。  
- flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。  
- flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。  
- flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。  
- align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。  
详见start-follow.html  
---
### rem与vm
rem，em  
vw，vh，vmin，vmax  

---
### html标签的合理使用


	<header>
	    header
	</header>
	<main>
	    <article>
	        <h1>title</h1>
	        <p>content</p>
	    </article>
	    <aside>
	        aside
	    </aside>
	</main>
	<footer>
	    footer
	</footer>


#### 语义嵌套约束
- li 用于 ul 或 ol 下；
- dd, dt 用于 dl 下；
- thead, tbody, tfoot, tr, td 用于 table 下；
#### 严格嵌套约束
- inline-Level 元素，仅可以包含文本或其它 inline-Level 元素;
- a里不可以嵌套交互式元素a、button、select等;
- p里不可以嵌套块级元素div、h1~h6、p、ul/ol/li、dl/dt/dd、form等。

---

### 布局
#### 常见布局
两列布局见demo  

双飞翼布局  
> 左翅sub有200px,右翅extra..220px.. 身体main自适应未知
> 
> 1.html代码中，main要放最前边，sub  extra
> 
> 2.将main  sub  extra 都float:left
> 
> 3.将main占满 width:100%
> 
> 4.此时main占满了，所以要把sub拉到最左边，使用margin-left:-100%  同理 extra使用margin-left:-220px
> 
> 5.main内容被覆盖了吧，除了使用外围的padding，还可以考虑使用margin。
> 
> 给main增加一个内层div-- main-inner, 然后margin:0 220px 0 200px
> 
> 6.main正确展示  

我们公司最常见的一种布局