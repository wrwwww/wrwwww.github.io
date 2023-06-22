---
title: vue3
date: 2023-05-05 21:30:42
tags:  
---
## createVNode与h函数
createVNode与h函数都是用来创建虚拟节点的.
createVNode();
第一个参数是要创建的节点 如'div'
第二个为动态属性{ class:{active,list},id:'dd'}
第三个参数为节点中的内容,可以往里面继续添加节点
创建出来的虚拟节点使用render()来渲染
```typescript
import {App, createVNode, Directive, render} from "vue";  
import loadingBar from '@/components/MLoading/index.vue'  
import router from "@/router";
// 创建虚拟节点
const node = createVNode(loadingBar)  
// 挂载到body上并渲染
render(node, document.body)  

// 导入路由,将不同的函数挂载在路由的钩子上
router.beforeEach(()=>{  
    node.component?.exposed?.startLoading()  
})  
router.afterEach(()=>{  
    node.component?.exposed?.endLoading()  
  
})
```
elementUI的全局导入,应该也是使用了插件,通过插件来将其中的组件全部注册为全局组件


## vue插件
这是一个ts文件,可以导入vue组件,然后通过app.component("组件名",组件)来全局挂载组件,这样就可以在任意的单文件组件中使用这个组件
```typescript
import {App, Directive} from "vue";  
// vue插件  
// 可以导出一个对像和函数,如果是对象的话需要install函数  
export  default {  
    install(app:App){  
       //可以挂一个全局组件在app上
       //通过 `app.component()` 和 `app.directive()` 注册一到多个全局组件或自定义指令
       // 挂载全局的$translate() 方法
       app.config.globalProperties.$translate = (key) => { }
       
    }  
}
// 导入后在app.use(..)挂载

```
## hook
介绍：通过组合式API可以把复杂或常用的逻辑封装成一个个hook来进行调用，简介优雅，方便维护。
hooks 本质是一个函数，是对 setup 中使用的 composition API的封装，通俗来讲：抽出一部分代码单独放一个js里，在setup里就可直接用hook中的函数，不是父子组件的关系。
hooks 复用率高，使setup中逻辑更清晰
`vueuse`很多hook函数
```typescript
// 使用组合式api的watch函数,在需要使用的直接调用就好
// 其实就相当于utils库,只不过这种库中用到的watch只能在vue中使用,不像其他工具类库,不受框架限制,使用原生api开发
export function useHistory() {  
    const route=useRoute();  
    // history.state对于没有后一个和前一个的记录返回null  
    const backPath = ref<string>(history.state.back);  
    const forwardPath = ref<string>(history.state.forward);  
  
    watch(()=>route.path,()=>{  
        backPath.value = history.state.back;  
        forwardPath.value = history.state.forward;  
    })  
    return{  
        backPath,  
        forwardPath  
    }  
}
```

## 自定义指令
<template>  
<div v-lsls="1">你好,我是风吹麦浪</div>  
</template>  
  ```vue
<template>  
<!-- 调用自定义指令 -->
<div v-lsls="1">你好,我是风吹麦浪</div>  
</template>

```
```typescript
<script setup lang="ts">  
import {Directive,onMounted} from "vue";  
// 定义自定义指令 名字需要v开头
const vLsls:Directive={  
  mounted: (el,binding) => {  
    el.innerText=el.innerText+'hello,我是自定义指令'  
    console.log('这里',el)  
    console.log('这里',binding.value)  
  }  
  
}  
</script>
```
### 自定义指令的生命周期
```typescript
const myDirective = {
  // 在绑定元素的 attribute 前
  // 或事件监听器应用前调用
  created(el, binding, vnode, prevVnode) {
    // 下面会介绍各个参数的细节
  },
  // 在元素被插入到 DOM 前调用
  beforeMount(el, binding, vnode, prevVnode) {},
  // 在绑定元素的父组件
  // 及他自己的所有子节点都挂载完成后调用
  mounted(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件更新前调用
  beforeUpdate(el, binding, vnode, prevVnode) {},
  // 在绑定元素的父组件
  // 及他自己的所有子节点都更新后调用
  updated(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件卸载前调用
  beforeUnmount(el, binding, vnode, prevVnode) {},
  // 绑定元素的父组件卸载后调用
  unmounted(el, binding, vnode, prevVnode) {}
}
```
### 全局自定义指令
```typescript
import {App, Directive} from "vue";
// 自定义指令的名字 和directive对象或者函数,不过函数写法只在`mounted` 和 `updated` 时触发并且行为相同
app.directive('lazy',vLazy)
app.directive('lazy',(el)=>{
	console.log(el)
})
```
## 一个将img转为base64的指令


## 批量注册插件
```typescript

import {App, createVNode, Directive, render} from "vue";  
import loadingBar from '@/components/MLoading/index.vue'  
import router from "@/router";  
 
export  default {  
    install(app:App){  
        // 批量注册组件  
        // vite的glob导入组件
        const modules = import.meta.glob('../*/*.ts',)  
        
        for (const path in modules) {  
            modules[path]().then((mod: any) => {  
                let temp=path.split('/')  
                temp.pop()  
                console.log(temp.pop(),mod)  
                //注册插件
                app.use(mod.default)  
            })  
        }  
        // 批量注册组件同理
        // 扫描所以组件
        // 使用app注册就好了
    }  
}
```

## vue事件修饰符
1. prevent：阻止默认事件 
2. stop：阻止事件冒泡
3. once：事件只触发一次  
4. capture：使用事件的捕获模式
5. self：只有event.target是当前操作的元素时才触发事件
6. passive：事件的默认行为立即执行，无需等待事件回调执行完毕
![[Pasted image 20230414214056.png]]
事件冒泡:点击按钮后会先打印按钮,box2,box1从内到外,从当前节点往上,直到根节点
事件捕获:跟事件冒泡正好相反,是从box1->box2->btn,从根节点出发到目标节点

passive修饰如滚动默认是先执行函数在滚动,添加修饰符之后是先滚动在执行事件函数
### @wheel
滚轮事件