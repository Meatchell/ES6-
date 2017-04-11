#es6入门笔记
- let命令和cunst命令
    1. 在一个代码块内，不允许重复声明。
    2. const命令也存在“死区”，conts命令只是指向变量所在的地址，所以将一个对象声明为常量是要非常小心。
    ```
    const obj = {};
    obj.name = 'Meatchell';
    console.log(obj.name) //Meatchell
    obj = {} //无效 报错
    todo：对象指向的是一个不可变地址，但对象本身是可变的，如果想将对象冻结可以使用`Object.freeze()`方法只在当前
    代码块起作用，如果要跨模块，需要处理。
    ```
    3. 在代码块内使用let命令声明变量之前，该变量都是不可用的。在语法上称为“暂时性死区”（temporal dead zone 简称 “TDZ”）。
    ```
    隐蔽死区
function bar(x=y, y=2){
  return [x, y];
}
bar(); // 报错
    ```
    
- 全局对象属性
    1. ES6 中规定var 和function命令声明的全局变量，属性全局对象（浏览器环境是window，nodejs 则是global）的属性。let、const、class 声明的全局变量，不属于全局对象的属性.
    ```
        var a = 1;
        //如果在node环境可以global.a 浏览器环境 window.a 访问，通用方法 this.a
        let b = 2;
        window.b //undefined        
    ```
    
- 变量的解构赋值
    1. 数组的结构赋值
    ``` 
        var [a,b,c] = [1,2,3]
        a //1
        b //2
        c //3
        
        let [a,\[\[b],[c]]] = [1,[[2],[3]]]
        a //1
    ```

    