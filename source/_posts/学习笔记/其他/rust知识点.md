---
title: rust知识点
date: 2023-05-05 21:30:42
tags:  
---
调用c语言的rand数的模块

```rust
extern "C" {
    fn rand() -> i32;
}

```

Rust中的使用双指针的时候要考虑usize类型小于0的边界情况

#[allow(dead_code)]取消代码警告

```rust
//结构体sheep
//trait animal
//为sheep实现animal的trait
//sheep就可以用animal的功能
//trait相当于抽象类

```

#### 包管理

将其它rs文件用pub mod animal 导入

然后用animal 的功能需要 use导入

正则表达式

```rust
```

#### map

```rust
//声明
let map : HashMap<u32, u32> = HashMap::new();
//通过insert(k,v);插入数据
//遍历map
for (k,v) in map{
    todo!
}
//map不按顺序排列,需要顺序排列可以使用BtreeMap
//可以使用[]来取值,但不安全面对没有的值会崩溃程序,推荐使用get来取值

```



##### 同时用HashMap和BTreeMap 统计数组数字数量

```rust
 let mut map:HashMap<i32, i32>=HashMap::new();
    let mut btree:BTreeMap<i32, i32>=BTreeMap::new();
    let nums=vec![1,1,2,3,2,2,4,5,6,5,9];
    // 用map统计nums中的数字的数量
    for num in nums{
        let number=map.entry(num).or_insert(0);
        let number2=btree.entry(num).or_insert(0);
        *number+=1;
        *number2+=1;
    }
```

###### 两种树的统计结果

可以看出btreemap是有序的而hashmap无序

```
k=2     v=3
k=1     v=2
k=5     v=2
k=6     v=1
k=9     v=1
k=3     v=1
k=4     v=1
-----------
k=1     v=2
k=2     v=3
k=3     v=1
k=4     v=1
k=5     v=2
k=6     v=1
k=9     v=1
```

### 函数式编程

```rust
//迭代1~10通过过滤器选择符合条件x%2==0的数,通过collect收集起来
let s=(0..=10).filter(|x|x%2==0).collect::<Vec<i32>>();
//map对迭代器中的数据进行操作
s.iter().map(|x|x*2).collect::<Vec<i32>>();
//take从迭代器中取出指定长度的数据

//join将迭代器元素组合成单个字符串,并且添加一个字符串

//sorted_by(|a,b|(a.1).cmp(b.1))使用指定的排序器进行排序
```

#### 闭包

```rust
struct xx{
    pub a:i32
}
impl xx {
    fn new(num:i32)->Self{
        Self{
            a:num
        }
    }
    fn cmp<B,F>(x:B,y:B,mut f: F)->bool
    where
        F: FnMut(B,B) -> bool,
    {
        f(x,y)
    }
}
fn main(){
	let asa=xx::new(3);
	let asa2=xx::new(5);
   	// 简单的比较器
    print!("{}", xx::cmp(asa,asa2,|x,y|x.a<y.a));
}
输出结果:true

```




































