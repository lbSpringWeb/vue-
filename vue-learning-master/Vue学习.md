# Vue学习

## 收集表单数据：

1. 若：<input type="text" />, 则v-model收集的是value值，用户输入的就是value值。（输入框）
2. 若：<input type="radio" />, 则v-model收集的是value值，且要给标签配置value值。（单选，例如：男，女）
3. 若：<input type="checkbox" />
   1. 没有配置input的value属性，那么收集的就是checked（勾选 or 未勾选， 是布尔值）
   2. 配置input的value属性：
      1. v-model的初始值是非数组，那么收集的就是checked（勾选 or 未勾选，是布尔值）
      2. v-model的初始值是数组，那么收集的就是value组成的数组
4. 备注：v-model的三个修饰符：
   1. lazy：失去焦点再收集数据
   2. number：输入字符串转为有效的数字
   3. trim：输入首尾空格过滤

```vue
年龄：<input type="number" v-model.number="age">
```

## 过滤器：

定义：对要显示的数据进行特定格式化后再显示（适用于一些简单的逻辑的处理）。

语法：

1. 注册过滤器：Vue.filter(name,callback) 或 new Vue { filters: { } }
2. 使用过滤器：{{ xxx | 过滤器名 }}  或  v-bind：属性 = "xxx | 过滤器名"

```vue
Vue.filter('name',function(value){
	return XXXX
})
```

备注：

1. 过滤器也可以接受额外参数、多个过滤器也可以串联
2. 并没有改变原本的数据，是产生新的对应的数据

## 内置指令：

v-bind		: 单向绑定解析表达式，可简写为 	:
v-model	 : 双向数据绑定
v-for		   : 遍历数组、对象、字符串
v-on		   : 绑定事件监听，可简写为	@
v-if			 : 条件渲染（动态控制节点是否==存在==）
v-else		: 条件渲染（动态控制节点是否==存在==）
v-show	  : 条件渲染（动态控制节点是否==**展示**==）
v-text		 :  

1. 作用：向其所在的节点中渲染文本的内容
2. 与插值语法的区别：v-text会替换掉节点中的内容，{{xx}}则不会。

v-html		:

1. 作用：向指定节点中渲染包含html结构的内容
2. 与插值语法的区别
   1. v-html会替换掉节点中所有的内容，{{XX}}则不会
   2. v-html可以识别html结构
3. 严重注意：v-html有安全性问题！！！！
   1. 在网站上动态渲染任意HTML是非常危险的，容易导致XSS攻击
   2. 一定要在可信任的内容上使用v-html，永远不要用在用户提交的内容上

v-cloak指令（没有值）

1. 本质是一个特殊的属性，Vue实例创建完毕并接管容器后会删除v-cloak属性
2. 使用css配合v-cloak可以解决网速慢时页面展示出 {{XX}} 的问题

v-once指令（没有值）

1. v-once所在的节点在初次动态渲染后，就视为静态内容了。
2. 以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能。

v-pre指令（没有值）：

1. 跳过其所在节点的编译过程。
2. 可以利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译。

## 自定义指令

1. 自定义语法：

   1. 局部指令：																				

      new Vue ({																				new Vue ({

      ​	directives: { 指令名：配置对象 }				或								directives{指令名：回调函数}

      })																								})

   2. 全局指令：

      Vue.directive(指令名，配置对象)  或	Vue.directive( 指令名，回调函数 )

2. 配置对象中常用的3个回调：

   1. bind：指令与元素成功绑定时调用
   2. inserted：指令所在元素被插入页面时调用
   3. update：指令所在模板结构被重新解析时调用

3. 备注

   1. 指令定义时不加v-，但使用时要加v-
   2. 指令名如果是多个单词，要使用kebab-case命名方式，不要用camelCase命名。

## 生命周期

1. 又名：生命周期回调函数、生命周期函数、生命周期钩子
2. 是什么：Vue在关键时刻帮我们调用的一些特殊名称的函数。
3. 生命周期函数的名字不可更改，但函数的具体内容的程序员根据需求编写的。
4. 生命周期函数中的this指向是 vm 或 组件实例对象

![img](https://img-blog.csdnimg.cn/915b9f85e4264142a3e1935135c10204.png)

生命周期四大阶段：
1.初始化阶段:
beforeCreate：实例刚创建完成,此时还没有data和methods属性
created：vue实例data和method属性已经初始化完成,此时还没有编译模板

2.实例挂载阶段
beforeMount：挂载前 模板编译完成,此时e l 还 没 有 挂 载 , el还没有挂载，data目前可见
mounted：挂载完成后 模板编译完成,$el挂载完成

3.数据更新阶段
beforeUpdate： 数据更新时执行,data数据此时已经是最新的数据,UI界面还是旧的
updated：数据更新完成后,界面和data里的数据此时都是最新的,完成的界面的更新渲染render

4.销毁阶段
销毁前: beforeDestroy： 实例准备销毁,此时data和methods方法都能用
销毁后:destroyed： 实例销毁完成,此时原先创建的实例方法和属性都不可以属性

常用的生命周期钩子：

1. mounted：发送ajax请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】
2. beforeDestroy：清除定时器、解绑自定义事件、取消订阅消息等【收尾工作】。

关于销毁Vue实例

1. 销毁后借助Vue开发者工具看不到任何信息。
2. 销毁后自定义事件会失效，但原生DOM事件依然有效。
3. 一般不会在 beforeDestroy 操作数据，因为即使操作数据，也不会再触发更新流程了。































