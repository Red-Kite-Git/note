# Vue

## 一、 简介

Vue.js是目前最流行的、国产的前端MVVM框架作者是尤雨溪 ( 华人 )

**优势**

* 简洁：HTML 模板 、 Vue 实例 、 JSON 数据
* 轻量：17kb，性能好
* 渐进式：边学边用
* 设计思想：视图与数据分离，无需操作DOM
* 社区：大量的中文资料和开源案例

## 二、基础

### 1.根组件

### 2.Vue中的生命周期

### 3.模板字符串

### 4.禁用元素

### 5.单击事件

## 三、双向绑定

## 四、入门程序

### 1.计数器

```Vue
<body>
	<div id="app">
		<!-- {{ }} 是差值 -->
		<h1>{{count}}</h1>
	</div>
</body>
<script>
	// 定义名为App的json格式的常量
	const App = {
		// 在Model中创建了count变量，变量值为1	M（Model）VVM
		data() {
			return {
				count: 1,
			}
		},
		// mounted是Vue的生命周期函数 当组件内容渲染到页面上自动执行
		mounted() {
			// setInterval函数每隔 1000ms 执行一次
			setInterval(() => {
				this.count++
			}, 1000)
		}
	};
	// 使用Vue.creatApp(App)方法创建Vue实例，创建时传入了定义好的App常量。.mount("#app")方法将实例挂载到id为app的元素上
	// 返回值vm 为项目的根组件
	const vm = Vue.createApp(App).mount("#app");
</script>
```

### 2.字符串翻转

```Vue
<body>
	<div id="app">

	</div>
</body>
<script>
	const App = {
		data() {
			return {
				message: "HelloWorld"
			}
		},
		// 在methds函数里面定义函数
		methods: {
			fun1() {
				//split方法分割字符串成数组
				//reverse方法翻转数组中的元素
				//join方法拼接数组为字符串
				this.message = this.message.split("").reverse().join("")
			}
		},
		template: `<div>{{message}}</div> <button @Click="fun1()">字符翻转</button>`
	}
	Vue.createApp(App).mount("#app")
</script>
```

### 3.内容展示与隐藏

```Vue
<body>
	<div id="app">
		<!-- v-if指令属性 true展示 false隐藏 -->
		<span v-if="isShow">展示和隐藏的内容</span>
		<button @click="fun1()">展示/隐藏</button>
	</div>
</body>
<script>
	const App = ({
		data() {
			return {
				isShow: true
			}
		},
		methods: {
			fun1() {
				this.isShow = !this.isShow;
			}
		}
	});
	Vue.createApp(App).mount('#app');
</script>
```

### 4.任务列表

```Vue
<body>
	<div id="app">
		<!-- v-model指令属性 将data中inputVal的值绑定输入框的内容 -->
		<input type="text" v-model="inputVal" />
		<button @click="insert()">新增</button>
		<ul>
			<!-- v-for指令属性 遍历集合/数组 -->
			<li v-for="(item,index) in list">
				{{index+1}}.{{item}}
			</li>
		</ul>
	</div>
</body>
<script>
	const App = ({
		data() {
			return {
				inputVal: '请输入任务',
				list: []
			}
		},
		methods: {
			insert() {
				// push方法 向数组添加元素
				this.list.push(this.inputVal);
			}
		}
	});
	Vue.createApp(App).mount('#app');
</script>
```

