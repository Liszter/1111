#### 1. JavaScript运行三部曲

* 过程
  * 语法分析
  * 预编译
  * 解释执行

* 过程详细

  * 页面产生，对应的window对象也就产生了。
  * 加载脚本
  * 脚本加载完毕，分析里面的语法是否正确
  * 开始预编译
    1. 查找--变量声明，先赋值为undefind
    2. 查找--函数声明， 赋值予函数体。

* 例题

  1. 直接声明变量

     ```js
         function bar(){
             return foo;
             foo = 10;
             function foo(){
      
             }
             var foo = 11;
         }
         document.write(bar())
     ```

     * 错误记录 

       1. 2021年3月15日 15:00:05

          

  2. ```js
      function bar(){
             foo = 10;
             function foo(){
     
             }
             var foo = 11;
             return foo;
         }
         document.write(bar())
     ```

     * 错误记录
       1. 2021年3月15日 15:00:31

  3. ```js
     function test(x,x){
         console.log(x); 
         x = 10;
         console.log(arguments); 
         function x(){}
     }
     
     test(5,20);
     ```

  4. 没有弄明白的题

     ```js
     function test(x,x){
     console.log(x); // function x(){}
     x = 10;
     console.log(arguments); // [5,10]
     function x(){}
     }
     
     test(5,20);
     ```

  5. 当有if时

     ```js
     var a = 10;
     if( function c() {} ){		//表达式，函数名被忽略
         function f() {
             return typeof c + a;	// undefined10
         }
     }
     function test(a, b){
      
         console.log(a);	//20
         var a = 20;
         console.log(d);	//undefined 
      
         if(b) {
             function d () {
                 console.log(b() + a);
             }
         }
         console.log(d);	//function d
         d();	//undfined1020
     }
     test(20, f);
     
     ```

     * 错误记录
       * 2021年3月15日 15:16:31



* 总结
  1. 代码块中的函数也得不到提升
  2.  if( true / false ){ /* */ } 中不会得到提升