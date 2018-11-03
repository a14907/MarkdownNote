
<!-- TOC -->
- [1. Vuejs起步](#1-vuejs)
- [2. Vuejs 模板语法](#2-vuejs-)
    - [2.1. 插值](#21-)
        - [2.1.1. 文本](#211-)
        - [2.1.2. Html](#212-html)
        - [2.1.3. 属性](#213-)
        - [2.1.4. 表达式](#214-)
    - [2.2. 指令](#22-)
        - [2.2.1. 参数](#221-)
        - [2.2.2. 修饰符](#222-)
    - [2.3. 用户输入](#23-)
    - [2.4. 过滤器](#24-)
    - [2.5. 缩写](#25-)
        - [2.5.1. v-bind 缩写](#251-v-bind-)
        - [2.5.2. v-on 缩写](#252-v-on-)
- [3. Vuejs 条件与循环](#3-vuejs-)
<!-- /TOC -->

# 1. Vuejs起步
	<div id="vue_det">
		<h1>site : {{site}}</h1>
		<h1>url : {{url}}</h1>
		<h1>{{details()}}</h1>
	</div>
	<script type="text/javascript">
		var vm = new Vue({
			el: '#vue_det',
			data: {
				site: "菜鸟教程",
				url: "www.runoob.com",
				alexa: "10000"
			},
			methods: {
				details: function() {
					return  this.site + " - 学的不仅是技术，更是梦想！";
				}
			}
		})
	</script>

可以看到在 Vue 构造器中有一个el 参数，它是 DOM 元素中的 id。
data 用于定义属性，实例中有三个属性分别为：site、url、alexa。
methods 用于定义的函数，可以通过 return 来返回函数值。
{{ }} 用于输出对象属性和函数返回值。

除了数据属性，Vue 实例还提供了一些有用的实例属性与方法。它们都有前缀 $，以便与用户定义的属性区分开来。例如：

    document.write(vm.$data === data) // true
	document.write("<br>") // true
	document.write(vm.$el === document.getElementById('vue_det')) // true

# 2. Vuejs 模板语法
## 2.1. 插值
### 2.1.1. 文本
数据绑定最常见的形式就是使用 {{...}}（双大括号）的文本插值：

### 2.1.2. Html
使用 v-html 指令用于输出 html 代码：

### 2.1.3. 属性
HTML 属性中的值应使用 v-bind 指令。
以下实例判断 class1 的值，如果为 true 使用 class1 类的样式，否则不使用该类：

	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="utf-8">
	<title>Vue 测试实例 - 菜鸟教程(runoob.com)</title>
	</head>
	<style>
	.class1{
	  background: #444;
	  color: #eee;
	}
	</style>
	<body>
	<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script>

	<div id="app">
	  <label for="r1">修改颜色</label><input type="checkbox" v-model="class1" id="r1">
	  <br><br>
	  <div v-bind:class="{'class1': class1}">
		v-bind:class 指令
	  </div>
	</div>
	
	<script>
	new Vue({
		el: '#app',
	  data:{
  		class1: false
	  }
	});
	</script>
	</body>
	</html>

### 2.1.4. 表达式
Vue.js 都提供了完全的 JavaScript 表达式支持。

## 2.2. 指令
指令是带有 v- 前缀的特殊属性。
指令用于在表达式的值改变时，将某些行为应用到 DOM 上。如下例子：

	<div id="app">
		<p v-if="seen">现在你看到我了</p>
	</div>
    
	<script>
	new Vue({
	  el: '#app',
	  data: {
		seen: true
	  }
	})
	</script>

### 2.2.1. 参数
参数在指令后以冒号指明。例如， v-bind 指令被用来响应地更新 HTML 属性：

	<div id="app">
		<pre><a v-bind:href="url">菜鸟教程</a></pre>
	</div>    
	<script>
	new Vue({
	  el: '#app',
	  data: {
		url: 'http://www.runoob.com'
	  }
	})
	</script>
在这里 href 是参数，告知 v-bind 指令将该元素的 href 属性与表达式 url 的值绑定。
另一个例子是 v-on 指令，它用于监听 DOM 事件：

	<a v-on:click="doSomething">

### 2.2.2. 修饰符
修饰符是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()：

	<form v-on:submit.prevent="onSubmit"></form>

## 2.3. 用户输入
在 input 输入框中我们可以使用 v-model 指令来实现双向数据绑定：

	<div id="app">
		<p>{{ message }}</p>
		<input v-model="message">
	</div>
    
	<script>
	new Vue({
	  el: '#app',
	  data: {
		message: 'Runoob!'
	  }
	})
	</script>
v-model 指令用来在 input、select、text、checkbox、radio 等表单控件元素上创建双向数据绑定，根据表单上的值，自动更新绑定的元素的值。

按钮的事件我们可以使用 v-on 监听事件，并对用户的输入进行响应。

以下实例在用户点击按钮后对字符串进行反转操作：

	<div id="app">
		<p>{{ message }}</p>
		<button v-on:click="reverseMessage">反转字符串</button>
	</div>
    
	<script>
	new Vue({
	  el: '#app',
	  data: {
		message: 'Runoob!'
	  },
	  methods: {
		reverseMessage: function () {
		  this.message = this.message.split('').reverse().join('')
		}
	  }
	})
	</script>

## 2.4. 过滤器
Vue.js 允许你自定义过滤器，被用作一些常见的文本格式化。由"管道符"指示, 格式如下：

	<!-- 在两个大括号中 -->
	{{ message | capitalize }}

	<!-- 在 v-bind 指令中 -->
	<div v-bind:id="rawId | formatId"></div>

过滤器函数接受表达式的值作为第一个参数。
以下实例对输入的字符串第一个字母转为大写：

	<div id="app">
	  {{ message | capitalize }}
	</div>
    
	<script>
	new Vue({
	  el: '#app',
	  data: {
		message: 'runoob'
	  },
	  filters: {
		capitalize: function (value) {
		  if (!value) return ''
		  value = value.toString()
		  return value.charAt(0).toUpperCase() + value.slice(1)
		}
	  }
	})
	</script>

过滤器可以串联：
	
	{{ message | filterA | filterB }}
过滤器是 JavaScript 函数，因此可以接受参数：

	{{ message | filterA('arg1', arg2) }}

这里，message 是第一个参数，字符串 'arg1' 将传给过滤器作为第二个参数， arg2 表达式的值将被求值然后传给过滤器作为第三个参数。

## 2.5. 缩写
Vue.js 为两个最为常用的指令提供了特别的缩写：
### 2.5.1. v-bind 缩写

	<!-- 完整语法 -->
	<a v-bind:href="url"></a>
	<!-- 缩写 -->
	<a :href="url"></a>

### 2.5.2. v-on 缩写

	<!-- 完整语法 -->
	<a v-on:click="doSomething"></a>
	<!-- 缩写 -->
	<a @click="doSomething"></a>

# 3. Vuejs 条件与循环
## 条件判断
### v-if
条件判断使用 v-if 指令：

    <div id="app">
        <p v-if="seen">现在你看到我了</p>
        <template v-if="ok">
          <h1>菜鸟教程</h1>
          <p>学的不仅是技术，更是梦想！</p>
          <p>哈哈哈，打字辛苦啊！！！</p>
        </template>
    </div>
    
    <script>
    new Vue({
      el: '#app',
      data: {
        seen: true,
        ok: true
      }
    })
    </script>

这里， v-if 指令将根据表达式 seen 的值(true 或 false )来决定是否插入 p 元素。
在字符串模板中，如 Handlebars ，我们得像这样写一个条件块：

    <!-- Handlebars 模板 -->
    {{#if ok}}
      <h1>Yes</h1>
    {{/if}}

### v-else
可以用 v-else 指令给 v-if 添加一个 "else" 块：

    <div id="app">
        <div v-if="Math.random() > 0.5">
          Sorry
        </div>
        <div v-else>
          Not sorry
        </div>
    </div>
    
    <script>
    new Vue({
      el: '#app'
    })
    </script>

### v-else-if
v-else-if 在 2.1.0 新增，顾名思义，用作 v-if 的 else-if 块。可以链式的多次使用：

    <div id="app">
        <div v-if="type === 'A'">
          A
        </div>
        <div v-else-if="type === 'B'">
          B
        </div>
        <div v-else-if="type === 'C'">
          C
        </div>
        <div v-else>
          Not A/B/C
        </div>
    </div>
    
    <script>
    new Vue({
      el: '#app',
      data: {
        type: 'C'
      }
    })
    </script>

### v-show
我们也可以使用 v-show 指令来根据条件展示元素：

    <h1 v-show="ok">Hello!</h1>

# 循环语句

循环使用 v-for 指令。

v-for 指令需要以 site in sites 形式的特殊语法， sites 是源数据数组并且 site 是数组元素迭代的别名。

v-for 可以绑定数据到数组来渲染一个列表：

    <div id="app">
      <ol>
        <li v-for="site in sites">
          {{ site.name }}
        </li>
      </ol>
    </div>
 
    <script>
    new Vue({
      el: '#app',
      data: {
        sites: [
          { name: 'Runoob' },
          { name: 'Google' },
          { name: 'Taobao' }
        ]
      }
    })
    </script>

模板中使用 v-for：

    <ul>
      <template v-for="site in sites">
        <li>{{ site.name }}</li>
        <li>--------------</li>
      </template>
    </ul>

## v-for 迭代对象

v-for 可以通过一个对象的属性来迭代数据：

    <div id="app">
      <ul>
        <li v-for="value in object">
        {{ value }}
        </li>
      </ul>
    </div>
 
    <script>
    new Vue({
      el: '#app',
      data: {
        object: {
          name: '菜鸟教程',
          url: 'http://www.runoob.com',
          slogan: '学的不仅是技术，更是梦想！'
        }
      }
    })
    </script>

你也可以提供第二个的参数为键名：

    <div id="app">
      <ul>
        <li v-for="(value, key) in object">
        {{ key }} : {{ value }}
        </li>
      </ul>
    </div>