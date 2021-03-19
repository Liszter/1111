* this试题

  * 阿里面试题

  ```js
  var name = 222
  var a = {
      name: 111,
      say: function () {
          console.log(this.name)
      }
  }
  
  var fun = a.say()
  
  fun() // fun.call(window) 222
  a.say() // a.say.call(a) // 111
  
  var b = {
      name: 333,
      say: function (fun) {
          fun() // window.name
      }
  }
  
  b.say(a.say)  // 222
  b.say = a.say
  b.say() // 333
  ```

  

* Q1  在函数中直接使用

  ```js
  function get (val) {
  	console.log(val)
  }
  
  get(1)
  
  get.call(window, '1')
  ```

* Q2 函数作为对象的方法被调用（谁调用我， 我就指向谁）

  ```js
  var person = {
      name: '张三',
      run: function(val) {
          console.log(`${this.name} ------ ${val}`)
      }
  }
  
  person.run(22)
  
  person.run.call(person, 22)
  
  
  ```

  

* 例题 

  * Q1-- 有关对象中的this

    ```js
    var obj = {   
                     a: 10,
                     b: this.a + 10, 
        			//这里的this指向window(全局)，a为undefined  ==>  undefined + 20 = NaN
                     fn: function () {
                         return this.a;
                       }
                 }
                console.log(obj.b);        //NaN
                console.log(
                    obj.fn(),         //10
                    obj.fn            //fn
                );
    ```

    

  * Q2 -- 

    ```js
    
                var a = 20; 
                var obj = {
                    a: 10,
                    getA: function () {
                        return this.a;
                      }
                }
                console.log(obj.getA());    //10   obj.getA.call(window)
                var test = obj.getA;        // test = return this.a
                console.log(test());        //20   独立调用test
    
    ```

  * Q3  

    ```js
                 var a = 5;
                 function fn1(){
                     var a = 6;
                     console.log(a);        //6
                     console.log(this.a);   //5
                 }
    
                 function fn2(fn) {
                     var a = 7;
                     fn();
                } 
    
                 var obj = {
                     a: 8,
                     getA: fn1
                 }  
                fn2(obj.getA);
    
    ```

  * Q4

    ```js
        function fn( ) {
                   'use strict';
                    var a = 1;
                    var obj = {
                        a: 10,
                        c: this.a + 20        //严格模式下，a指向undefined嘛，undefined.a报错
                    }
                    return obj.c;
                  }
                 console.log(fn());       //输出报错==》 a undefined
    ```

  * Q5

    ```js
     // 声明一个构造函数 
                 function Person(name, age) {
                    this.name = name;
                    this.age = age;
                    console.log(this);      //与下面的this是一样的，都是Person
                }   
                // Person();           //this 指向window
                Person.prototype.getName = function () {
                    console.log(this);      //与上面的this是一样，都是Person
                }; 
                var p1 = new Person("test", 18);
                p1.getName();
    
    ```

  * Q6   匿名函数的this

    ```js
     var obj = {
                   foo: "test",
                   fn: function(){
                       var mine = this;
                       console.log(this.foo);    
                       console.log(mine.foo);     
                       
                       (function(){
                          console.log(this.foo);   
                          console.log(mine.foo);    
                       })();  
                   } 
                };
                obj.fn();
    
    ```

  * Q7 分辨 obj中的this和 function的this

    ```js
    let a = {
        name: '1',
        dbname：this, // 这里this为 window
        get: function() {
            console.log(this) //  这个this 是 a
        }
    }
    ```

     

  * Q8 

    ```js
       function foo(){
           console.log(this.a);
        }
                var a = 2;
                var o = {
                    a:3, 
                    foo: foo
                };
                var p = { a:4 };
                o.foo(); // 3
                (p.foo = o.foo)(); // 2
                
    		   p.foo = o.foo;
                p.foo(); // 4？
    ```

  * Q9

    ```js
        function foo() {
                    console.log(this.a);
                }
                var obj1 = {
                    a: 3,
                    foo: foo
                };    
                var obj2 = {
                    a: 5,
                    foo: foo
                };
                obj1.foo();     //3
                obj2.foo();     //5
                
                obj1.foo.call(obj2);    //5
                obj2.foo.call(obj1);    //3
    
    ```

  * Q10 

    ```js
              function test(arg) {
                    this.x = arg;
                    return this;
                } 
    
                /**
                   var x = test(5);  -->  window.x = window.test(5);
                
                */
    
                var x = test(5);     //此时 x = window, y = undefined
                var y = test(6);     //此时 x = 6,  y = window , 后面申请的x会覆盖掉第一次在this.x 生成的window.x
                console.log(x.x);     //undefined,   实际上是6.x  是undefined
                console.log(y.x);     //6     实际上是window.x 也就是6
    
    ```

  * Q11

    ```js
       var obj = {
                    data: [1,2,3,4,5],
                    data2: [1,2,3,4,5],
                    fn: function () {
                       console.log("--test--");
                       console.log(this);   //{data: Array(5), data2: Array(5), fn: ƒ, fn2: ƒ}
                       return this.data.map(function (item) {
                             console.log(this);     //  -->  window
                             return item * 2;
                        }); 
                    },
                    fn2: function () {
                       console.log("---test2---");
                       console.log(this);   //{data: Array(5), data2: Array(5), fn: ƒ, fn2: ƒ}
                       return this.data2.map(item=>{
                           console.log(this);   //  --> obj {data: Array(5), data2: Array(5), fn: ƒ, fn2: ƒ}
                           return item * 2; 
                       });
                    }
                 };  
                 obj.fn();
                 obj.fn2();
    
    ```

  * 





* 箭头函数中的this
  * 箭头函数中的this是在定义函数的时候绑定的，而不是在执行函数的时候绑定的。
  * 箭头函数中，this指向的固定化，因为箭头函数中根本就没有自己的this，导致其内部的this就是外层代码块的this。 正因为它没有this，所以也就不能用作构造函数。





* Q1 ： 

  ```
   
  ```

  

