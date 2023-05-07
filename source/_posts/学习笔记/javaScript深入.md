变量,函数会被添加到全局环境当中
```JavaScript
    var yy="sss"
    function foo(){
        console.log("goo");
    }
    console.log(window);
```
![[Pasted image 20230423131206.png]]
![[Pasted image 20230423131314.png]]

声明的函数会在内存创建一个对象,包含父作用域的作用域链(Scopes),
只要用var声明一个变量,这个变量在这个作用域开始的时候就被bind在当前作用域对象中,只不过没到声明的部分,他值为underfine
```JavaScript
    function foo(){
        // 在函数编译的过程中,sss就被绑定在当前活动对象的属性上,没初始化的时候是underfine
        console.log(sss);//underfine
        // 被赋值初始化,就不是underfine了
        var sss="sdfd";
        console.log(sss);//sdfd
    }
    foo()

```
同上面这个一样k1在函数作用域中被绑定在作用域对象上默认为underfine
```javascript
    var k1=110

    function foo(){

        console.log(k1);// underfine

        var k1=120;

        console.log(k1);// 120

    }
```
这里在函数作用域对象上寻找k1没有找到,就到当前对象的作用域链上父作用域寻找没找到就报错
```javascript
    var k1=110

    function foo(){

        console.log(k1);// 120

    }
```
虽然bar在foo中被调用,当时bar的父作用域是全局对象,他找到的是k1=110,而不是调用他的函数中的k1=888
```JavaScript
    var k1=110

    function foo(){

        var k1=888

        console.log(k1);// 888

        bar()

    }

  

    function bar(){

        console.log(k1);// 110

    }

    foo()
```