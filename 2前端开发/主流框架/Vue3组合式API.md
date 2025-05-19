# Vue3

## 一、Vue3特点

* #### 性能提升

  * 打包大小减少41%

  * 初次渲染快51%，更新渲染快133%

  * 内存占用减少54%

* #### 源码升级

  * 使用Proxy 代替Object.defineProperty 实现双向数据绑定

  * 重写虚拟DOM的实现和Tree-Shaking

  * vue3 更好的支持typescript

* #### 新的特性

  * composition API（组合式api）

  * setup配置

  * ref和reactive

  * watch 和watchEffect

  * provide和inject

  * 提供了新的内置组件

  * Fragment

  * Teleport

  * Suspense

* #### 其他改变

  * 新的生命周期钩子

  * 移除keyCode 作为v-on的修饰符

### 2.开发环境搭建

* 创建项目方式（Vue CLI / Vite）
* 项目目录结构解析
* 开发服务器配置

## 二、核心语法

### 1.setup方法

setup函数是Composition API的入口函数，所有的Composition API都在setup函数中写

老写法 ▼

```vue
<template>
  <div></div>
</template>

<script>
export default {
  setup() {
      
  }
};
</script>

<style></style>
```

语法糖 直接在script标签上写setup ▼

```vue
<template>
  <div></div>
</template>

<script setup>
</script>

<style></style>
```

### 2.ref

> 在vue3的setup函数中，不使用this来访问组件数据、实例等
>
> 使用`数据.value`访问数据值

```vue
<template>
    <div ref = "elem">
		<div>普通变量：{{ number }}</div>
  		<div>对象变量：{{ object }}</div>
  		<div>数组变量：{{ array }}</div>
  		<div><button @click="fun">函数</button></div>
    </div>
</template>

<script setup>
// 引入ref函数
import { ref } from "vue";

// ref()函数用于创建一个响应式数据 接收一个参数，返回一个包含value属性的对象
// 定义普通变量
let number = ref(666);

// 定义对象变量
let object = ref({ id: 1, name: "张三" });

// 定义数组变量
let array = ref([666, "string", { id: 1, name: "张三" }, [1, 2, 3]]);

// 定义函数变量
let fun = ref(function () {
  alert("函数调用成功");
});

// 获取DOM元素 （获取ref属性值为elem的元素）
let elem = ref();
    consle.log(elem);
</script>

<style></style>
```

### 3.事件方法与计算属性 

* 事件方法 点击按钮数字+1

```vue
<script setup>
......
// 定义time变量 赋值0
let time = ref(0);
setInterval(() => {
  time.value++;
}, 1000);
......
</script>
```

* 计算属性

```vue
<script setup>
......
// 定义变量 使用导入的computed
let doubleCount = computed(() => {
  return time.value * 2;
});
......
</script>
```

### 4.reactive与ref之间转换

```vue
<script setup>
......
// 使用reactive 封装对象用于复杂的对象信息 ，一般后端返回的数据使用ref
let school = reactive({
  name: "课工场",
  address: "徐州",
});
// 转化为ref （与原先的属性名一致）
// ref改变值，原先的reactive数据也同样变化  类似双向绑定
let { name, address } = toRefs(school);
name.value = "学校2";
address.value = "地址2";
......
</script>
```

> reactive一般用于对象,ref用于基本类型

### 5.watch函数

```vue
<script setup>
......
// 监听指定的响应式数据变化 watch函数
watch(time, (newValue, oldValue) => {
  // time值变化时触发的操作
  console.log("新值", newValue);
  console.log("旧值", oldValue);
});
......
</script>
```

### 6.watchEffect函数

```vue
<script setup>
......
//如果需要停止监听，可以用变量获取返回值，调用返回值函数即可
// let stopWatch =
// 监听所有的响应式数据变化（不接收特定参数）  watchEffect
watchEffect(
  (cb) => {
    // 在创建时会立即执行一次回调函数，之后每当数据发生变化就会再次执行
    console.log("time值变化为：", time.value);

    cb(() => {
      // 在数据更新之前执行
      console.log("数据更新之前执行");
    });

    // watchEffect是在DOM更新之前执行
    if (elem.value) {
      console.log("div元素中的值：" + elem.value.innerHTML);
    }
  },
  // 添加 {flush:"post"} 表示在DOM更新之后执行（默认为pre）
  {
    flush: "post",
  }
);
// 调用返回值函数  停止监听
// stopWatch();
......
</script>
```

> 可以通过设置flush:'post',让watchEffect在数据响应 后,DOM更新后执行
>
> 获得watchEffect函数的返回值,使用返回值可以停止 watchEffect函数的监听行为
>
> 在watchEffect中传入形参,形参名对应的方法会在更新 前触发



## 三、生命周期函数 

```vue
<script setup>
......
// 生命周期函数：组件挂载前执行
onBeforeMount(() => {
  console.log("组件挂载前执行的操作");
  console.log(elem.value);
});

// 生命周期函数：组件挂载后执行
onMounted(() => {
  console.log("组件挂载后执行的操作");
  console.log(elem.value);
});
......
</script>
```

## 四、跨组件通信方案 

### 1.provide与inject

使用provide与inject实现父组件与子组件之间参数传递

```vue
<template>
  <div>
    <child-component />
  </div>
</template>

<script setup>
import { provide, ref } from "vue";
// 引用子组件
import ChildComponent from "@/components/ChildComponent.vue";
// 使用ref创建一个响应式数据count值为100
let count = ref(100);
// 使用provide实现向子组件进行参数传递
provide("count", count);
</script>

<style></style>
```

```vue
<template>
  <div>
    父组件传递的值：{{ count }}
    <button @click="fun1()">子组件修改值</button>
  </div>
</template>

<script setup>
import { inject } from "vue";
// 获取传递的值
let count = inject("count");

let fun1 = () => {
  count.value = 999999;
};
</script>

<style></style>
```

子组件获得父组件传递的参数值以后默认是可以修改传 入的参数值。这种操作实际并不是太好,我们一般不允 许在子组件中自己修改传入的值。我们可以在父组件中 定义修改的函数,在子组件中调用函数去修改传入的参数

```vue
<template>
  <div>
    <child-component />
  </div>
</template>

<script setup>
import { provide, readonly, ref } from "vue";
// 引用子组件
import ChildComponent from "@/components/ChildComponent.vue";

let readonlyCount = ref(100);
// 使用provide实现向子组件进行参数传递
// 使用readonly实现只读
provide("readonlyCount", readonly(readonlyCount));

// 提供修改只读值的函数
let changeCount = () => {
  readonlyCount.value = 88888;
};
// 使用provide实现向子组件传递函数
provide("changeCount", changeCount);
</script>

<style></style>
```

```vue
<template>
  <div>
    父组件传递的只读值：{{ readonlyCount }}
    <button @click="fun2()">子组件无法直接修改只读值</button>
    <button @click="changeCount">子组件调用父组件的函数修改只读值</button>
  </div>
</template>

<script setup>
import { inject } from "vue";

// 获取传递的值
let readonlyCount = inject("readonlyCount");
// 获取传递的函数
let changeCount = inject("changeCount");

let fun1 = () => {
  count.value = 999999;
};
let fun2 = () => {
  readonlyCount.value = 999999;
};
</script>

<style></style>
```

### 2.defineProps与defineEmits

利用defineProps与defineEmits进行组件通信 在组合式API中,完成父子组件通信操作 

```vue
<template>
  <div>
    <!-- 父组件传递参数 -->
    <child2-component
      count="666"
      message="Hello"
      @custom-click="getChildParams"
    />
  </div>
</template>

<script setup>
import Child2Component from "@/components/Child2Component.vue";

let getChildParams = (data) => {
  alert(data);
};
</script>

<style></style>
<template>
  <div>
    <!-- 父组件传递参数 -->
    <child2-component count="666" message="Hello" />
  </div>
</template>

<script setup>
import Child2Component from "@/components/Child2Component.vue";
</script>

<style></style>
```

```vue
<template>
  <div>父组件中传递的属性：{{ props.count }}{{ props.message }}</div>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";

// 定义props 子组件中使用defineProps来接收父组件传递过来的数据
let props = defineProps({
  count: {
    type: Number,
    default: 0,
  },
  message: {
    type: String,
    default: "",
  },
});

// 使用defineEmits自定义事件
let emit = defineEmits(["custom-click"]);
// 父组件触发custom-click事件 就会传递数据
emit("custom-click", "子组件传递的数据");
</script>

<style></style>
```



## 五、复用组件功能之use函数

use函数实际上指的是我们定义的一些名字是以use开头 的函数。 通常我们可以将一些需要复用的代码封装到use函数中, 在需要使用时导入使用。

1 在compotable文件夹下创建use.js文件

```js
import { computed, ref } from "vue";

// 通用的use函数 （函数名以use开头 一般是封装的通用函数）
function useCounter() {
  let count = ref(0);
  let doubleCount = computed(() => {
    return count.value * 2;
  });
  return {
    count,
    doubleCount,
  };
}

export { useCounter };
```

 2 在组件中导入use函数

```vue
<template></template>

<script setup>
// 导入复用函数
import { useCounter } from "@/compotable/use.js";
// 调用复用函数，并将函数返回值（返回值：{count,doubleCount}）解构 赋值给count和doubleCount
let { count, doubleCount } = useCounter();
// 使用数据
count.value = 100;
console.log(count.value);
console.log(doubleCount.value);
</script>

<style></style>
```

