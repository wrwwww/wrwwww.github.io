# HTML+Css+JavaScript 50天项目练习

## 2022-12-16  day01
### 效果图
![[2022-12-16 day01.gif]]
### 收获
```javascript
// 通过css选择器去选择指定的dom节点
let panels = document.querySelectorAll('.panel')
// 之前都是直接操作className
panels[i] += " active"
panels[i] = "panel"
// 通过classList来添加或者删除class
panels[i].classList.add("active");
panel.classList.remove('active')
```
```css
/* 在active类后对相关的元素进行操作 */
.mian-warp .panel.active h3 {
    opacity: 1;
    transition: opacity 0.3s ease-in 0.4s;
}
/* 元素想要垂直水平居中可以设置父元素为flex
   其中align-items使得子元素垂直居中
   justify-content使得字元素水平居中
   overflow是防止出现滚动条
*/
body {
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
}
/* flex元素在不设置大小的时候，可以直接设置flex来变得比其他同级要大
   于此同时使用vh,或者vw来适应浏览器窗口大小的变化
*/
.mian-warp .panel {
    height: 80vh;
    flex: 0.5;
    margin: 10px;
}
.mian-warp .panel.active {
    flex: 5;
}

```
## 2022-12-17 day02
### 效果图
![[2022-12-17 day02.gif]]
### 收获
```javascript
// 可以直接设置按钮失效
next.disabled = false
```
```css
/**
我是在除第一的后面几个添加两个伪类一个是灰色背景一个是有颜色的
  not选择器，决定不选哪些
**/
li:not(:first-child)::before {   
}
/**
学习到可以通过设置一个大的背景通过遍历节点来控制蓝色进度条的长度
**/
```

 
##  2022-12-18 day03
### 效果图
![[2022-12-18 day03.gif]]
### 收获
```javascript
// js就是普通的点击事件
```
```css
/**
	‘+’兄弟选择器的使用
	实现牵一发而动全身，一个兄弟动跟前的兄弟也动
     .main.active+nav .nav-items li:nth-child(2) {}
	旋转角度以及焦点
	 transform-origin: top left;
	 猫头鹰选择器 body * + *{}
**/
```
##  2022-12-19 day04
### 效果图
![[2022-12-19 day04.gif]]


### 收获
```javascript
 // javascript实现很简单就是点击按钮的时候
 // 简单的将输入框的宽度增加缩小
```
```css
/**
 css中需要注意的就是input和button是行内块，他们的基线不同
 使用vertical-align:middle;来统一基线，达到两个元素齐平
 因为是行内元素要让他两相连可以在html结构中不换行书写
 以及在html加载结束后才开始加载js
 window.onload=function(){}
 ()()立即执行的匿名函数
**/
```

## 2022-12-20 day05
### 效果图
![[2022-12-20 day05.gif]]


### 收获
```javascript
// setInterval的使用
let t=setInterval(blurring,20);
function blurring(){
        idx+=1;
        if(idx>99){
            clearInterval(t);
        }
}
// 以及将99分为30部分
// idx在99中的比率,*30就为等比例

```
```css

```

## 2022-12-21 day06
### 效果图
 
 ![[2022-12-21 day06.gif]]

### 收获
```javascript
// 当前窗口的高度
 window.innerHeight
 // dom元素e距离窗口顶部的距离
 e.getBoundingClientRect().top
```
```css

```


## 2022-12-22 day07
### 效果图
 ![[2022-12-22 day07.gif]]


### 收获
```javascript
 

```
```css

```


## 2022-12-23 day08
### 效果图
 
![[2022-12-23 day08.gif]]

### 收获
```javascript
 
// 利用js将原来的字符串分为字符,并且单独加上一个过渡延迟
labels.forEach(label => {
label.innerHTML = label.innerText
.split('').map((letter, idx) => `<span style="transitiondelay:${idx*50}ms">${letter}</span>`)
        .join('')
```
```css

```

##  2022-12-24 day09
### 效果图
 
![[图片/javascript/2022-12-24 day09.gif]]
### 收获
```javascript
// 播放音乐,选中audio的dom对象
	song.play();


```
```html
<audio class="a1" src="./Sound Board - 50项目50天.mp3"></audio>

```

## 2022-12-25 day10
### 效果图 
![[2022-12-25 day10.gif]]
### 收获
```javascript
// fetch的使用
    async function get_jokes(){
		// 请求url
        let a=await fetch("https://icanhazdadjoke.com/",
        // 请求头
        {
	        headers:{
	            Accept:  "application/json"
	        }
        })
        // 响应
        .then(resp=>resp.json())

        jokes.innerText=await a.joke;

    }
```
```css

```

##  2022-12-26 day11
### 效果图 
![[2022-12-26 day11.gif]]
### 收获
```javascript
// 使用innerHTML插入网页
// 以及key监听事件
```
```css

```
##  2022-12-27 day12
### 效果图 
![[2022-12-27 day12.gif]]
### 收获
```javascript
// 给父元素类名做操作
// toggle有就remove,没有就add
e.parentElement.classList.toggle("active");
```
```css

```

## 2022-12-28 day13
### 效果图 
![[2022-12-28 day13.gif]]

### 收获
```javascript
//定时器,延时器的使用
let track=setInterval(track1,100);
    function track1(){
        buttons.forEach(e=>e.classList.remove("active"));
      buttons[Math.floor(Math.random()*buttons.length)].classList.add("active");
    }
    setTimeout(function(){
        clearInterval(track);
    },2000);
    //以及添加按钮监听事件,在按钮按了之后就清空按钮,重新绘制计算按钮
    
```
 
## 2022-12-29 day14
### 效果图 
![[图片/javascript/2022-12-29 day14.gif]]
### 收获
```css

```

## 2022-12-30 day15
### 效果图 

![[2022-12-30 day15.gif]]
### 收获
```javascript
// +的妙用
// 1.当 + 号的两边都是number类型的时候，此时 + 号 代表数学符号加法
// 2. 当 + 号写在一个数据前面的时候，此时 + 号代表数学符号 正号
// 可以给字符串隐式转换
// 3. 当 + 号的两边，只要有一边是string类型的时候，此时 + 号代表字符串的连接符
```
```css

```
## 2022-12-31 day16
### 效果图 
![[2022-12-31 day16.gif]]
### 收获
```javascript
// 底下水瓶的逻辑
// 点按指定水瓶后前面的水瓶都点亮
// 当水瓶是点亮的最后一个,再次点击就会切换状态
```
```css

```

明天考虑实现一个玩文件里添加指定内容的东西,最好是动态的.





# 模板
## 时间
### 效果图 

### 收获
```javascript

```
```css

```

