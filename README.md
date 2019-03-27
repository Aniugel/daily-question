# 每日一题[（生活一题）](./life.md)

在这里记录着每天自己遇到的一道印象深刻的前端问题，以及一道生活中随处可见的小问题。

强迫自己形成积累的习惯，鞭挞自己不断前行，共同学习。

### **2019/03/27 - 2019/03/31**

- 请问这句语句 `var args=[].slice.call(arguments,1)` 是什么意思？

先看原函数：

```js
function a() {
  var args = [].slice.call(arguments, 1);
  console.log(args);
}

a("haha", 1, 2, 3, 4, 5); // log出[1, 2, 3, 4, 5]
a("run", "-g", "-b"); // log出['-g', '-b']
```

1. 首先，函数 call() 方法，第一个参数改变函数的 this 指向，后面剩余参数传入原函数 slice 中
2. arguments 是什么？
   > arguments 是函数中的一个类数组的参数集合对象
   > 如： {'0': 'haha', '1': 1, '2': 2}
3. slice 为数组可从已有的数组中返回选定的元素。
   > 此题为从 index = 1 往后
4. 综上，这句语句的作用是——**将函数中的实参值转化成数组**
