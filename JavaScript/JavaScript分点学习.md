## 对象、继承和类

1. JavaScript不是完全面向对象的语言，而是**基于对象（object-based）**的语言。

2. ES5类（构造函数）：属性定义在构造函数内，方法定义在原型下；静态属性和方法使用类名定义和调用。

   继承需要组合继承（构造函数继承属性，原型继承方法）

   ES5：

   分别位于三个位置上：实例上，原型上，类上

   ```js
   function Class(instanceAttribute){
       this.instanceAttribute = instanceAttribute
       this.instanceMethod = function (){
           console.log("实例方法")
       }
   }
   Class.prototype.protoAttribute = "原型属性"
   Class.prototype.protoMethod = function (){
       console.log("原型方法")
   }
   Class.staticAttribute = "静态属性"
   Class.staticMethod = function (){
       console.log("静态方法")
   }
   let instance1 = new Class("instance1的实例属性")
   console.log(instance1)
   ```

   

3. call,apply,bind

   1、call,apply和bind的区别

   它们在功能上是没有区别的，都是改变this的指向，它们的区别主要是在于方法的实现形式和参数传递上的不同。call和apply方法都是在调用之后立即执行的。而bind调用之后是返回原函数，需要再调用一次才行，

   2、①：函数.call(对象,arg1,arg2....)

   ②：函数.apply(对象，[arg1,arg2,...])

   ③：var ss=函数.bind(对象,arg1,arg2,....)

   3、总结一下call，apply，bind方法：

   a：第一个参数都是指定函数内部中this的指向（函数执行时所在的作用域），然后根据指定的作用域，调用该函数。

   b：都可以在函数调用时传递参数。call，bind方法需要直接传入，而apply方法需要以数组的形式传入。

   c：call，apply方法是在调用之后立即执行函数，而bind方法没有立即执行，需要将函数再执行一遍。有点闭包的味道。

   d：改变this对象的指向问题不仅有call，apply，bind方法，也可以使用that变量来固定this的指向。

   <details><summary>来源说明</summary>作者：中公教育IT培训
       链接：https://www.zhihu.com/question/40892203/answer/1100672560
       来源：知乎
       著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。</details>

4. 
