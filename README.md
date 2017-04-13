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
        - 解构完全成功的状态
        <pre> 
            var [a,b,c] = [1,2,3]
            a //1
            b //2
            c //3
            
            let [a,[[b],[c]]] = [1,[[2],[3]]]
            a //1
            
            let [,,third] = ['foo',bar','baz'];
            thira //baz
            
            let [head,...tail] = [1,2,3,4];
            head //1
            tail //[2,3,4]
            如果解构不成功，变量的值等于undefined；
            let [a,b] = [1,2,3];
        </pre>
        - 解构只能用于数组和对象，其他原始类型的值都可以转换为对象，但undefined 和 null 不行
        ```
            let [foo] = null //报错
            let [foo] = undefined //报错
        ```
        - 数组的解构赋值允许制定默认值
        <pre>
            var [too = true] = [];
            too //true
            
            let [x,y='b'] = ['a',undefined];
            x //a
            y //b
            
            const [x] = [null]
            x //null  
        </pre>
        - todo Set结构也可以使用数组的结构赋值
        ```
        let [a,b,c] = new Set(['a','b','c'])
        a //a
        ```
    2. 对象的解构赋值
        - 对象没有次序，变量必须与属性同名，才能取到正确的值。
        ```
        var {foo,bar} = {foo:'a',bar:'b'}
        foo //a
        bar //b
        let {foo,bar} = {bar:'b',foo:'a'};
        foo //a
        bar //b
        ```
        - 对象的解构赋值的内部机制，是先找到同名属性，然后在赋给对应的变量。真正被赋值的是后者，而不是前者。
        ```
        var {foo:baz} = {foo:'a',fcc:'b'};
        baz //a
        foo // foo is not defined;  真正被赋值的变量式baz 而不是模式foo
        ```
        - 嵌套
        ```
        var node = {
            loc: {
                start: {
                    line:1,
                    column:5
                }
            }
        } 
        var { loc: { start: { line }}} = node;
        line //1
        只有line 是变量， loc 和start 都是模式，不会被赋值
        ```
        - 注意——默认值生效的条件是，对象的属性值严格等于undefined--为避免js 将`{}`理解成代码块，应避免大括号出现在行首
        ```
            var {x,y = 5} = {x:1};
            x //1
            y //5
            var {x = 3} = {x:undefined};
            x //3
            var {x = 3} = {x:unll};
            x //null  
            ——————————————————————————————————————
            var x;
            {x} = {x:1} //syntax error
            //正确写法
            ({x} = {x:1}:
            ——————————
            *let{ log,sin,cos} = Math;
        ```
    3. 用途
        - 变量的交换
        ```
        [x,y]= [y,x];
        ```
        - 从函数返回对个数值
        ```
        //返回一个数组
        function example(){
            return [1,2,3]
        }
        var [a,b,c] = example();
         ——————
         //返回一个对象
         function example(){
            return {
                foo:a,
                fcc:b
            }
         }
         var {foo,fcc} = example()
        ```
        - 函数参数的定义
        ```
        //有序
        function fn([x,y,z]){...}
        fn([1,2,3]);
        //无序
        function fn({x,y,z}){...}
        fun({x:3,y:2,z:1})
        ```
        - 提取JSON数据
        ```
        var jsonData = {
            id:43,
            status:'OK',
            data:{'name':'Meatchell','age':'man'}
        }
        let { id, status, data.obj} = jsonData;
        ```
        - 函数参数的默认值
        ```
        jQuery.ajax = function(url,{
        async = true,
        beforeSend = function(){...},
        cache = true,
        complete = function(...){},
        crossDomain = false,
        // ...more config
        }){
        // ...do stuff
        }
        ```
   