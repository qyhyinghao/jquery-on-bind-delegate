# jquery-on-bind-delegate

## jQuery绑定事件的方法

### bind()
```
$("div p").bind("click",function(){
   console.log("bind");
   console.log(this);
});
```
- bind方法存在两个问题

1. 这里用了隐式迭代的方法，如果匹配到的元素特别多的时候，比如如果我在div里放了50个p元素，就得执行绑定50次。对于大量元素来说，影响到了性能。
但是如果是id选择器，因为id唯一，用bind()方法就很快捷了。
2. 对于尚未存在的元素，无法绑定。点击页面上的按钮，将动态添加一个p元素，点击这个p元素，会发现没有动作响应。
### degelate()

```
$("div").delegate("p","click",function(e){
    console.log('delegate');      
    console.log(this);   
    console.log(e.target);
    console.log(e.currentTarget);
    console.log(e);
});
```
- 这样就解决了用bind()方法的上面两个问题，不用再一个个地去为p元素绑定事件，也可以为动态添加进来的p元素绑定。甚至，如果你将事件绑定到document上，都不用等document准备好就可执行绑定。
- 事件代理的底层原理
不是直接为p元素绑定事件，而是为其父元素（或祖先元素也可）绑定事件，当在div内任意元素上点击时，事件会一层层从event target向上冒泡，直至到达你为其绑定事件的元素，如此例中的div元素。冒泡的过程中，如果事件的currentTarget与选择器匹配时，就会执行代码。

- 事件代理的缺点：如果事件目标在DOM树中很深的位置，这样一层层冒泡上来查找与选择器匹配的元素，又影响到性能了。

### on()方法

```
$("div").on("click",'p',function(e){
    console.log('on');       
    console.log(this);      
    console.log(e.target);
    console.log(e.currentTarget);
    console.log(e);
});
```
- on()其实是将以前的绑定事件方法作了统一，查看jQuery无压缩的源码（我这里看的版本是1.11.3），可以发现无论bind()还是delegate()其实都是通过on()方法实现的，只是参数不同罢了。
- on 和delegate的区别
	1. on 和 deletate的参数顺序不同
	2. on 的第二个参数可以省略
- 官方建议最好用on方法绑定事件

### 移除事件
- 对应于bind()、delegate()和on()绑定方法，其移除事件的方法分别为：

	```
	$( "div p" ).unbind( "click", handler );
	$( "div" ).undelegate( "p", "click", handler );
	$( "div" ).off( "click", "p", handler );
	```
### 总结
1. 选择器匹配到的元素比较多时，不要用bind()迭代绑定
2. 用id选择器时，可以用bind()
3. 需要给动态添加的元素绑定时，用delegate()或者on()
4. 用delegate()和on()方法，dom树不要太深
5. 尽量使用on()
