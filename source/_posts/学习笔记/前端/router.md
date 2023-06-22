---
title: router
date: 2023-05-05 21:30:42
tags:  
---
## 导入创建路由
通过createRouter()方法
```Typescript
import s from '../views/s.vue'

// vite的动态匹配,这个将匹配符合规则的模块
// 匹配到的文件默认是懒加载的，通过动态导入实现，并会在构建时分离为独立的 chunk
// 通过 modules['./view/ss.vue']() 来加载模块
const modules = import.meta.glob('./views/*.vue')


const router = createRouter({
	// 路由的模式
	// hash路由模式和history路由模式。
	// hash模式浏览器有#号,history没有
  history: createWebHashHistory(),
  // 路由
  routes: [{path,
  // 三种导入方式,推荐第三种
  component:s.vue,
  
  component:()=>import('../*.vue*'),
  
  component:()=>modules['./view/ss.vue'](),
    
  ,name,children,redirect}],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})
// 导出router
export default router
```

在main.ts种挂载
```typescript
// @是./src的别名 vite提供的别名,在这个项目中都可以使用@
// 导入router
import router from '@/router'
// 挂载
createApp(App).use(router).mount('#app')
```

 路由的跳转
 ```vue
<!-- 类似a标签,点击后会路由跳转,不过不刷新浏览器-->
<!--  -->
<!-- 其中to可以是跳转的路由path,也可以是对象 -->


 <router-link tag="div" to="/">跳转a</router-link>
 <router-link tag="div" :to="{name:'login'}">跳转a</router-link>
<!-- a标签跳转,有弊端,会刷新浏览器-->
 <a href='/login'>ss</a>
 <!-- 路由匹配到的组件将渲染在这里 -->
 <router-view></router-view>
<!-- 编程的方式跳转-->
```
```typescript
import { useRouter } from 'vue-router'
const router = useRouter()
router.push('/login')
router.push({name:'login'})
router.push({path:'/login'})
// 替换原有组件但是不会留下history
router.replace('/main')
// history 的使用
// 前进一条history
router.go(1)
// 后退一条history
router.back()

```
路由间参数的传递
```typescript
import useRouter from 'vue-router'
router=useRouter()

//通过query传参
// 参数会在地址栏
router.push({
	path:'/login',
	query:item
})

// 接收参数
import useRoute from 'vue-router'
const route=useRoute()
route.query.[参数]

// 通过Params传参
router.push({
	path:'/login',
	params:item
})
// 通过params接收
route.params.[参数]

// 动态传参
// 路由
 {   
   //动态路由参数    
  path:"/login/:id",
  name:"login",
	component:()=> import('../components/login.vue')
   }
   // 在跳转后params携带的参数id会被记录
```
区别:
1. query 传参配置的是 path，而 params 传参配置的是name，在 params中配置 path 无效
2. query 在路由配置不需要设置参数，而 params 必须设置
3. query 传递的参数会显示在地址栏中
4. params传参刷新会无效，但是 query 会保存传递过来的值，刷新不变 ;
## 同一个路由多个组件
路由组件中使用components
视图的默认名称也是 `default`
```typescript
components: {
        default: () => import('../components/layout/menu.vue'),
        header: () => import('../components/layout/header.vue'),
        content: () => import('../components/layout/content.vue'),
}
// router-view 通过name对应组件
 <router-view></router-view> 
 <router-view name="header"></router-view> 
 <router-view name="content"></router-view>

```
## 重定向
路由的配置中使用`redirect`, 下面的路由地址栏中显示`/` ,内容为`/user`
```typescript
   {
    
    path:'/',
     component:()=> import('../components/root.vue'),
     // 字符串
     redirect:'/user1',
     // 对象
     redirect:{
       path:'/user'
    }
    // 函数 可以传递参数
      redirect:(to)=>{
       return {
       path:'/user'
       query:to.query
    }
    }
    
}
```
## 别名alias
给路由添加别名,可以实现不同的名称访问相同的组件
```typescript
const router=[{
path:'/'
alias:['/root','/root2'],
component:()=>import('../components/root.vue')
}]
```
