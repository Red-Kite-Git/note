# Vue入门

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

```Vue
<body>
	<div id="app">
		<input type="text" v-model="text"/>
	</div>
</body>
<script>
	//createApp表示创建一个Vue应用 定义的变量vm就是一个根组件
	const vm = Vue.createApp({
		data() {
			return {
				text:"根组件"
			}
		}
	}).mount("#app")
</script>
```
在浏览器控制台上可以查看、修改属性的值 ▼
```cmd
vm.$data.text
vm.$data.text="新的内容"
```



### 2.★Vue中的生命周期

```Vue
<body>
	<div id="app">

	</div>
</body>
<script>
	Vue.createApp({
		beforeCreate() {
			console.log('实例初始化之后执行, 此时实例还未创建,data选项中的数据还未初始化');
		},
		created() {
			console.log('组件实例创建完成,属性(data选项中的数据)已经被绑定(到组件上)。但是不能操作DOM');
		},
		beforeMount() {
			console.log('在组件内容被渲染到页面之前自动执行的函数');
		},
		mounted() {
			console.log('在组件内容被渲染到页面之后自动执行的函数,这个阶段可以执行dom操作');
		},
		beforeUpdate() {
			console.log('data中的数据发送改变之后自动执行的函数');
		},
		updated() {
			console.log('当数据发送变化,同时页面完成更新后,会自动执行的函数');
		},
		beforeUnmount() {
			console.log('vue应用失效时,自动执行的函数');
		},
		unmounted() {
			console.log('vue应用失效时,并且dom完全销毁之后,自动执行');
		}
	}).mount("#app")
</script>
```

### 3.模板字符串

```Vue
<body>
	<div id="app">
		<div>{{message}}</div>
		<div v-html="message"></div>
		<div v-text="message">helloworld</div>
		<!-- v-bind绑定组件的属性 设置为titleValue的值 -->
		<div :title="titleValue"/>
	</div>
</body>
<script>
	Vue.createApp({
		data() {
			return {
				message: '<strong>hello world</strong>',
				titleValue:"hello"
			}
		}
	}).mount("#app");
</script>
```

### 4.禁用元素

```Vue
<body>
	<div id="app">
		<input type="text" :disabled="isDisable" />

		<button @[event]="handleClick()" :[attr]="isDisable">按钮1</button>

	</div>
</body>
<script>
	Vue.createApp({
		data() {
			return {
				isDisable: true,
				event: "click",
				attr: 'disabled'
			}
		},
		methods: {
			handleClick() {
				alert('触发单击');
			}
		}
	}).mount("#app");
</script>
```

### 5.单击事件

```Vue
<body>
	<div id="app">
		<form action="http://www.baidu.com">
			<button @click="handleClick()">成功提交</button>
		</form>
		<!-- @click.prevent阻止提交触发的函数 -->
		<form action="http://www.baidu.com" @click.prevent="handleClick2">
			<button @click="handleClick()">阻止提交</button>
		</form>
	</div>
</body>
<script>
	Vue.createApp({
		methods: {
			handleClick() {
				alert('提交');
			},
			handleClick2() {
				alert('阻止提交');
			}
		}
	}).mount("#app");
</script>
```

### 6.★计算属性和监听器

```Vue
<body>
	<div id="app">
		数量：<input type="number" v-model="number"/>
		单价：<input type="number" v-model="price"/>
		<div>总价：{{ total }}</div>
	</div>
</body>
<script>
	Vue.createApp({
		data() {
			return {
				number: 2,
				price: 15
			}
		},
		// computed用于计算属性,下面的total函数只有当数量或者价格改变以后才会重新计算
		computed: {
			total() {
				return this.number * this.price;
			}
		},
		// watch监听到price属性的值改变  等1s之后打印price属性当前的值和之前的值。
		watch: {
			price(now, before) {
				setTimeout(() => {
					console.log("price当前的值是:" + now);
					console.log("price之前的值是:" + before);
				}, 1000);
			}
		}
	}).mount("#app");
</script>
```

### 7.样式绑定语法

```Vue
<body>
	<div id="app">
		<div :class="classString">{{text}}</div>
		<div :class="classObject">{{text}}</div>
		<div :style="styleObject">{{text}}</div>
	</div>
</body>
<script>
	Vue.createApp({
		data() {
			return {
				text: "hello world",
				classString: 'red',
				classObject: {
					green: true,
				},
				styleObject: {
					fontSize: '20px'
				}
			}
		}
	}).mount("#app");
</script>
```

### 8.条件渲染

```Vue
<body>
	<div id="app">
		<div v-if="isShow">isShow为true</div>
		<div v-else>isShow为false</div>
	</div>
</body>
<script>
	Vue.createApp({
		data() {
			return {
				isShow: true
			}
		}
	}).mount("#app")
</script>
```

### 9.列表渲染

```Vue
<body>
	<div id="app">
		<button @click="insertName()">新增名称</button>
		<ul>
			<!-- v-for指令 用一个变量 in list，变量是item -->
			<!-- v-for指令 用两个变量(item,index) in list，第一个变量是item，第二个是index -->
			<li v-for="item in nameList">{{ item }}</li>
		</ul>
	</div>
</body>
<script>
	Vue.createApp({
		data() {
			return {
				nameList: ["admin","zhangSan","liSi"]
			}
		},
		methods:{
			insertName(){
				this.nameList.unshift("redKite")
			}
		}
	}).mount("#app")
</script>
```

### 10.事件修饰符

* prevent

```Vue
<body>
	<div id="app">
		<a href="https://www.baidu.com" @click.prevent="handlClick()">阻止链接跳转</a>
	</div>
</body>
<script>
	Vue.createApp({
		methods: {
			handlClick() {
				alert("链接被点击！但被prevent事件修饰符阻止了跳转！")
			}
		}
	}).mount("#app")
</script>
```

* stop

```Vue
<body>
	<div id="app">
		<div @click="handlClick2()">
			<div @click="handlClick2()">
				<button @click.stop="handlClick()">阻止事件冒泡</button><br/>
				<button @click="handlClick()">不阻止事件冒泡,父级相同事件一起触发</button>
			</div>
		</div>
	</div>
</body>
<script>
	Vue.createApp({
		methods: {
			handlClick() {
				alert("单击事件！")
			},
			handlClick2(){
				alert("父级单击事件！")
			}
		}
	}).mount("#app")
</script>
```

## 三、★双向绑定

### 使用v-model指令双向绑定属性值

```Vue
<body>
	<div id="app">
		<div>
			文本框：
			<input type="text" v-model="inputText" />
		</div><br />
		<div>
			多选框：
			<input type="checkbox" v-model="checkbox" value="1" />选框1(value=1)
			<input type="checkbox" v-model="checkbox" value="2" />选框2(value=2)
			<input type="checkbox" v-model="checkbox" value="3" />选框3(value=3)
		</div><br />
		<div>
			单选按钮：
			<input type="radio" v-model="radio" value="1" />单选1(value=1)
			<input type="radio" v-model="radio" value="2" />单选2(value=2)
		</div><br />
		<div>
			下拉框：
			<select v-model="selectValue">
				<option v-for="object in options" :value="object.id">{{ object.name }}</option>
			</select>
		</div><br />
		<button @click="getValue()">取值</button>
	</div>
</body>
<script>
	Vue.createApp({
		data() {
			return {
				inputText: "默认值",
				checkbox: [1, 2],
				radio: 0,
				options: [{
					id: 0,
					name: "请选择"
				}, {
					id: 1,
					name: "选项1"
				}, {
					id: 2,
					name: "选项2"
				}, {
					id: 3,
					name: "选项3"
				}],
				selectValue: 0
			}
		},
		methods: {
			getValue() {
				alert("取得文本框中的值为：" + this.inputText +
					"\ncheckbox的值为：" + this.checkbox +
					"\nradio的值为：" + this.radio +
					"\noptionValue的值为：" + this.selectValue)
			}
		}
	}).mount("#app")
</script>
```

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

### 5.★任务列表-定义使用全局组件

```Vue
<body>
	<div id="app">
		<!-- @focus获得焦点事件 -->
		<!-- @blur失去焦点事件 -->
		<input type="text" v-model="inputVal" @focus="cleanInput()" @blur="initInput()" />
		<button @click="pushInput()">新增</button>
		<ul style="list-style-type: none">
			<!-- v-bind指令绑定组件的属性 可以简写 -->
			<li-item v-for="(item,index) in list" v-bind:index="index" :item="item"></li-item>
		</ul>
	</div>
</body>
<script>
	const App = Vue.createApp({
		data() {
			return {
				inputVal: "请输入任务",
				list: []
			}
		},
		methods: {
			cleanInput() {
				this.inputVal = ""
			},
			initInput() {
				if (this.inputVal === "") {
					this.inputVal = "请输入任务"
				}
			},
			pushInput() {
				if (this.inputVal != "") {
					this.list.push(this.inputVal)
				}
			}
		}
	});
	// 定义全局组件（组件名支持驼峰命名法与下划线命名法自动转换 liItem=>li-item）
	App.component("liItem", {
		// 定义属性
		props: ['index', 'item'],
		// 定义模板
		template: `
		<li>
			{{ index+1 }}.{{ item }}
		</li>
		`
	})
	App.mount("#app")
</script>
```

## 五、组件

### 1.全局组件

只要定义了就可以在任何位置使用。建议组 件名称由多个小写单词组成,中间使用横线进行分隔

```Vue
<body>
	<div id="app">
		<!-- 使用全局组件 -->
		<hello />
	</div>
</body>
<script>
	// 创建Vue实例
	const app = Vue.createApp({
		data() {
			return {
				counter: 0
			}
		}
	});
	// 定义全局组件
	app.component("hello", {
		template: `
		<h1>这是全局组件</h1>
		`
	})
	// 挂载
	app.mount('#app');
</script>
```

### 2.局部组件

注册之后才能使用,并且只能在当前注册的组 件中使用。建议组件名称由多个单词组成,每个单词的 首字母大写

```Vue
<body>
	<div id="app">
	</div>
</body>
<script>
	// 定义一个常量
	const package = {
		template: `
		<h1>这是局部组件</h1>
		`
	}
	// 创建Vue实例
	const app = Vue.createApp({
		// 在根组件中注册局部组件(组件名: 组件) 组件名与组件相同可以省略组件名
		components: {
			"hello": package
		},
		// 在根组件中使用局部组件
		template: `
		<hello />
		`
	});
	// 挂载
	app.mount('#app');
</script>
```

### 3.全局型的父子组件

```Vue
<body>
	<div id="app">
		<parent />
	</div>
</body>
<script>
	// 创建Vue应用实例
	const app = Vue.createApp({});
	// 定义全局子组件
	app.component("child", {
		data() {
			return {
				counter: 1,
			};
		},
		template: `
		<button @click="this.counter++">{{ counter }}</button>
		`,
	});
	// 定义全局父组件(父组件中使用子组件)
	app.component("parent", {
		template: `
		<child />
		`
	});
	//挂载
	app.mount("#app");
</script>
```

### 4.局部型的父子组件

```Vue
<body>
	<div id="app">
		<parent />
	</div>
</body>
<script>
	// 定义子组件
	const childPackage = {
		data() {
			return {
				counter: 1
			}
		},
		template: `
		<button @click="this.counter++">{{ counter }}</button>
		`
	}
	// 定义父组件
	const parentPackage = {
		//在父组件中注册子组件
		components: {
			"childPackage": childPackage
		},
		template: `
		<childPackage />
		`
	}
	// 定义根组件
	const vm = Vue.createApp({
		components: {
			parentPackage
		},
		template: `
		<parentPackage />
		`
	})
	// 挂载根组件
	vm.mount("#app")
</script>
```

### 5.父向子传参数

```Vue
<body>
	<div id="app">
		<package :param1="value1" :param2="value2" />
	</div>
</body>
<script>
	// 定义Vue应用实例
	const app = Vue.createApp({
		data() {
			return {
				value1: "hello",
				value2: 100
			}
		}
	})
	// 定义全局子
    组件
	app.component("package", {
		// 设置组件的属性
		props: [
			"param1", "param2"
		],
		template: `
		<div>{{ param1 }}</div>
		<div>{{ param2 }}</div>
		`
	})
	//挂载
	app.mount("#app")
</script>
```

### 5.单向数据流（父向子传参数）

子组件能使用父组件传递过来的值,但不能修改父组件 传递过来的值

```Vue
<body>
	<div id="app">
	</div>
</body>
<script>
	const app = Vue.createApp({
		data() {
			return {
				numValue: 1
			}
		},
		template: `
		<test :num=numValue />
		`
	});
	app.component("test", {
		props: ["num"],
		// 子组件只能使用不能修改num的值
		template: `
		<button @click=this.num++>{{ num }}</button>
		`
	})
	app.mount("#app")
</script>
```

可以使用一个变量接收父组件传递的值, 在子组件中去 修改该变量的值

```Vue
<body>
	<div id="app">
	</div>
</body>
<script>
	const app = Vue.createApp({
		data() {
			return {
				numValue: 1
			}
		},
		template: `
		<test :num=numValue />
		`
	});
	app.component("test", {
		props:["num"],
		data() {
			return {
				// 可以将num的赋值给子组件data中的childNum,在子组件中操作childNum
				childNum: this.num
			}
		},
		template: `
		<button @click=this.childNum++>{{ childNum }}</button>
		`
	})
	app.mount("#app")
</script>
```

### 6.子向父传参

```Vue
<body>
	<div id="app">
	</div>
</body>
<script>
	const app = Vue.createApp({
		template: `
		<p1 />
		`
	});
	// 全局父组件
	app.component("p1", {
		data() {
			return {
				// 接收子组件传递的变量
				reviceMsg: ""
			}
		},
		methods: {
			getChildMsg(msg) {
				this.reviceMsg=msg
			}
		},
		template: `
		<input type="text" v-model="reviceMsg" />
		<c1 @get="getChildMsg"/>
		`
	});
	// 全局子组件
	app.component("c1", {
		data() {
			return {
				msg: "子向父传的参数"
			}
		},
		methods: {
			fun1() {
				// this.$emit注册事件，父组件通过注册的get事件可以获得子组件传递的值
				this.$emit("get", this.msg);
			}
		},
		template: `
		<button @click="fun1()">子组件的按钮</button>
		`
	})
	app.mount("#app")
</script>
```

### 7.子组件/事件不接收传参值

子组件不定义属性，传的参数会直接挂载到子组件的容器属性上

```Vue
<body>
	<div id="app">
	</div>
</body>
<script>
	const app = Vue.createApp({
		data() {
			return {
				title1Value: "abc",
				title2Value: "def"
			}
		},
		template: `
		<my-head :title1=title1Value :title2=title2Value />
		`
	});
	app.component("MyHead", {
		// 子组件不定义属性，会将传入的参数挂在template中的标签属性上
		// props: ["title1", "title2"],
		template: `
		<div :title="$attrs.title1"><h1>标题</h1></div>
		`,
		// inheritAttrs属性默认为true false可以阻止默认行为（不定义属性会将传入的参数挂在template中的标签属性上）
		// 在inheritAttrs: false时，也可以手动在容器标签上绑定属性并使用 "$attrs.传参名" 将传参挂在属性上
		inheritAttrs: false
	})
	app.mount("#app")
</script>
```

## 六、仿Element-Plus 按钮 插件
