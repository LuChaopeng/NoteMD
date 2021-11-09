## 第2章 HTML中的JavaScript

#### 2.1 `<script>`元素

1. 执行顺序：浏览器会按照`<script>`在页面中出现的顺序依次解释它们，前提是它们没有使用 defer （延缓）和 async（异步） 属性。

2. 把JavaScript引用放在`<body>`元素中的页面内容后面，避免先把所有 JavaScript 代码都下载、解析和解释完成。

3. 推迟执行JavaScript

   在<script>元素上设置 defer 属性，相当于告诉浏览器立即下载（但也是要到加载到这个元素），但延迟执行，延迟到整个页面都解析完毕后。

   **有多个延迟脚本defer时执行顺序不能确定（尽管大多按照先后顺序），因此最好只包含一个这样的脚本**

4. 异步执行

   可以使用 async 属性表示脚本不需要等待其他脚本，同时也不阻塞文档渲染，即异步加载。异步脚本不能保证按照它们在页面中出现的次序执行。正因为如此，异步脚本**不应该在加载期间修改 DOM**。

5. 动态加载脚本

   可以通过DOM操作向DOM中添加script元素以加载脚本。

6. XHTML本意代替HTML，现已退出历史舞台。

#### 2.2 行内代码与外部文件

1. 最佳实践

   尽可能将JavaScript代码放在外部文件中。

   **可维护性、缓存、适应未来**

#### 2.3文档模式

1. 标准模式下，浏览器按照规范呈现页面。混杂模式下，页面以一种比较宽松的向后兼容的方式显示。

   `<!DOCTYPE html>`是标准模式下HTML5的声明；

   混杂模式靠省略doctype开启；

   标准模式和准标准模式非常接近。

#### 2.4 `<noscript>`元素

1. 对禁用或不支持（已经没有不支持js的了）JavaScript的浏览器提供替代内容。如果浏览器支持并启用脚本，则`<noscript>`元素中的任何内容都不会被渲染。



## 第3章 语言基础

#### 3.1 `<script>`元素

1. ECMAScript中一切都区分大小写

2. ECMAScript标识符的最佳实践是**使用驼峰大小写形式**。

3. 严格模式

   要对整个脚本启用严格模式，在脚本开头加上这一行：

   **`"use strict";` **

   也可在函数体开头单独指定一个函数严格模式。

4. 语句加分号；一条语句也用代码块；

#### 3.3 变量

1. 不推荐改变变量保存值的类型。

2. var的声明的作用域为包含它的**函数作用域**  **（*这里的函数指ECMAScript函数？定义在`if`语句中的大括号内的变量还在函数作用域内）**。

   去掉var 操作符之后，变量 就变成了全局变量，严格模式下不允许。

   用var定义变量会声明“提升”（hoist），也就是把所有变量声明都拉到函数作用域的顶部。

3. `let`和`var`声明的区别

   1. `let`是块作用域；`var`是函数作用域

   2. `let`声明的变量在作用域中不会被提升；

   3. `let`在*全局作用域中*声明的变量不会成为window对象的属性，`var`声明的会；

   4. for循环中，使用`var`定义的迭代变量会渗透到循环体外部。

      奇特问题：`eg`:

      ```javascript
      for (var i = 0; i < 5; ++i) { 
       setTimeout(() => console.log(i), 0) 
      } 
      // 你可能以为会输出 0、1、2、3、4 
      // 实际上会输出 5、5、5、5、5 
      /*
      因为在退出循环时，迭代变量保存的是导致循环退出的值：5。在之后执行超时逻辑时，所有的 i 都是同一个变量.
      */
      
      for (let i = 0; i < 5; ++i) { 
       setTimeout(() => console.log(i), 0) 
      } 
      // 会输出 0、1、2、3、4
      /*
      而在使用 let 声明迭代变量时，JavaScript 引擎在后台会为每个迭代循环声明一个新的迭代变量。
      */
      ```

4. `const`声明

   与`let`基本相同。

   区别：声明的**同时必须初始化变量**，且之后**不可被修改**。

   所以`const`不能声明迭代变量。

   但是值得注意的是，`const` 声明的限制只适用于它指向的变量的引用。如果 `const` 变量引用的是一个对象，可以修改这个对象内部的属性。

   因此可以在`for-of` 和` for-in` 循环中使用。

5. **最佳实践**

   `const`优先，`let`次之，不使用`var`。

#### 3.4 数据类型

1. ECMAScript不能自定数据类型，所有值使用7种数据类型表示

   **6种简单数据类型（原始类型）**

   Undefined、Null、Boolean、Number、String、Symbol

   **1种复杂数据类型**

   Object

2. `typeof`操作符

   ECMAScript类型松散，通过** `typeof`操作符**知道数据类型，其返回的字符串值有7种：

   - "undefined"  未定义；
   - "boolean"  布尔值；
   - "string"  字符串
   - "number"  数值
   - "object"  对象或null
   - "function"  函数
   - "symbol"  符号

   **注意：**

   `typeof`是一个操作符，不需要参数（可以使用参数）；

   `typeof null`返回"object"，`null`被认为是一个对空对象的引用；

   函数其实是对象，但其有自己的特殊属性，所以用`typeof`区分函数和其他对象。

3. **Undefined类型**

   1. 只有一个值`undefined`，使用var或let声明了但没初始化相当于赋了`undefined`值，默认未经初始化的变量都会取得 `undefined`值。
   2. 声明未初始化以及未声明，都会返回"undefined"，所以建议声明的同时初始化。
   3. `undefined`是一个假(false)值。

4. **Null类型**

   1. 只有一个值`null`，表示一个空对象指针。
   2. 用`null`来初始化将来要保存对象值的变量。
   3. `null`也是一个假值，由`undefined`派生而来。

5. **Boolean类型**

   1. 两个字面值：`true`和`false`
   2. 其他类型通过`Boolean()`函数转换为布尔值。

   ![转布尔值](src\转为布尔值.png)

6. **Number类型**

   1. 二进制字面量以`0b`开头，八进制以`0o`开头，16进制以`0x`开头，科学记数法`eg: 3.25e7` ;

   2. 浮点值内存是整数值的两倍，所以ECMAScript会尽量把值转换为整数，如定义`1.` `1.0` 都会被转换成整数`1`处理；

   3. 由于浮点数精度问题，**永远不要测试特定的浮点值**。

   4. `isFinite()`函数测试数值范围。

   5. 特殊值`NaN`（Not a number）

      1. `0/0` `-0/+0` ，0，0相除返回 `NaN`；`1/0`为`Infinity`，`1/-0`为`-Infinity`，分子非零返回无穷大；
      2. 任何`NaN`操作返回`NaN`；
      3. `NaN`不等于包括`NaN`在内的任何值；
      4. `isNaN()`函数接收任意数据类型判断是否“不是数值”，该函数会先尝试转换为数值；

   6. 数值转换

      `Number()`转型函数，将任何数据类型转为数值；

      `parseInt()`依据指定基数 [ 参数 **radix** 的值]，把字符串 [ 参数 **string** 的值] 解析成整数;

      `parseFloat()`把一个字符串解析成浮点数。

7. **String类型**

   1. 字符串可以使用双引号（"）、单引号（'）或反引号（`）标示，几种表示没有区别。
   2. 通过length属性获取字符串长度。 eg:`text.length`；
   3. ECMAScript的字符串的值一旦创建就不能变了，要修改必须先销毁原始的字符串。
   4. 通过`toString()`方法转为字符串。eg:`obj.toString()`；
   5. null和undefined没有`toString`方法，使用`String()`转型函数。eg:`String(value)`；
   6. 字符串插值 `${}`可以直接写在字符串里，表达式中可以调用函数和方法。
   7. 模板字面量使用反引号 (\` \`)引用，是允许嵌入表达式的字符串字面量。你可以使用多行字符串和字符串插值功能。
   8. *模板字面量标签函数；
   9. *使用默认的 String.raw 标签函数获取原始的模板字面量内容。

8. **Symbol类型**

   1. 符号是原始值，且符号实例是唯一、不可变的，它的用户是确保对象属性使用唯一标识符。
   2. 使用`Symbol()`函数初始化一个符号。eg：`let sym = Symbol();`
   3. *使用`Symbol.for()`在全局符号注册表中创建并重用符号。
   4. 凡是可以使用字符串或数值作为属性的地方，都可以使用符号作为属性。
   5. \* **常用内置符号**以Symbol工厂函数字符串属性的形式存在。

9. **Object类型**

   1. 通过创建Object类型的实例来创建自己的对象，再给对象添加属性和方法：`let o = new Object()`
   2. Object是派生其他类的基类，，所有属性和方法在派生对象同时存在。
      - `constructor`：用于创建当前对象的函数；
      - `hasOwnProperty(propertyName)`：判断当前实例是否存在给定的属性。
      - `isPrototypeOf(object)`：判断当前对象是否是另一个对象的原型
      - `propertyIsEnumerable(propertyName)`：用于判断给定的属性是否可以使用
      - `toLocalString()`返回对象的字符串表示。（本地化执环境）
      - `toString()`返回对象的字符串表示。
      - `valueOf()`返回对象对应的字符串、数值或布尔表示。

#### 3.5 操作符

大部略过

1. 指数操作符 ** 	

   `Math.pow(3,2);与 3**2 一样`

2. 等于操作符`==`和全等操作符`===`的区别在于前者会先进行强制类型转换再确定操作数是否相等。后者不转换操作数。

3. 逗号操作符在同一条语句执行多个操作

   eg:`let num1 = 1,num2 = 2,num3 = 3`;

#### 3.6 语句

1. `for-in` 

   严格的迭代语句，用于枚举对象中的非符号键属性

   `for (property in expression) statement		`eg:

   ```javascript
   for(const propName in window){
   document.write(propName);
   }//显示了BOM对象window的全部属性
   ```

   ECMAScript对象的属性是无序的，因此`for-in`不保证返回对象属性的顺序。

2. `for-of`

   严格的迭代语句，遍历可迭代对象的元素，

   `for(property of expression) statement`

   ```javascript
   for(const el of [2,4,6,8]){
   document.write(el);
   }
   ```

   按照可迭代对象的next()方法顺序迭代元素，不支持迭代的变量将抛出错误。

3. 标签语句 `label:statement`

4. `switch`语句在比较每个条件值时会使用全等操作符，不会强制转换数据类型。

#### 3.7 函数

1. 基本用法

   ```js
   function functionName(arg0, arg1,...,argN) { 
    statements 
   }
   ```

2. 最佳实践：函数要么返回值，要么不返回值。只在某个条件下返回值的函数会带来麻烦，尤其是调试时。



## 第4章 变量、作用域与内存

#### 4.1 原始值与引用值

1. 原始值就是最简单的数据，Undefined、Null、Boolean、Number、String 和 Symbol。保存原始值的变量**按值**访问；

   引用值则是由多个值构成的对象。JavaScript不能直接访问内存，实际上操作的是对该对象的引用而非实际的对象本身。为此，保存引用值的变量是**按引用**访问的。

2. 原始值大小固定，因此保存在栈内存上。

   引用值是对象，存储在堆内存上。

3. 复制值

   原始值复制直接复制内存里的原始值；

   引用值赋给另一个变量，复制的是一个指针，和原变量指向同一个对象。

4. 传递参数

   *明确*：**ECMAScript中所有函数的参数都是按值传递的**

   如果是原始值，那么就跟原始值变量的复制一样，如果是引用值，那么就跟引用值变量的复制一样。

   ***在按引用传递参数时，值在内存中的位置会被保存在一个局部变量，这意味着对本地变量的修改会反映到函数外部。（这在 ECMAScript 中是不可能的。）**  需在后续实践中深入理解

5. 确定类型

   `typeof`确定原始值的类型好用，但对于引用值null或对象，使用`instanceof`判断对象是否为给定引用类型的实例。（按照原型链判定）

#### 4.2 执行上下文与作用域

1. 每个上下文关联一个**变量对象**，上下文中的所有变量和函数都存在这个对象上。上下文在其代码执行完毕后被销毁。

2. **全局上下文**是最外层的上下文，在浏览器中是window对象。`var`定义的全局变量和函数都会成为window对象的属性和方法，`let`和`const`的顶级声明不会定义在全局上下文中，但在作用域链解析效果上是一样的。

3. 每个函数调用都有自己的上下文。当代码执行流进入函数时，函数的上下文被推到一个上下文栈上。在函数执行完之后，上下文栈会弹出该函数上下文。

4. 上下文中的代码在执行的时候，会创建变量对象的一个**作用域链**，代码正在执行的上下文的变量对象始终位于作用域链的最前端。

5. 函数参数被认为是当前上下文中的变量

6. 执行到`try/catch`语句中的`catch`块，会在作用域链 前端添加一个变量对象。创建一个新的变量对象，这个变量对象会包含要抛出的错误对象的声明。

7.  ** `const`**声明的变量不能再被赋其他引用值，但对象键值不受影响。如果想整个对象不能被修改，使用`Object.freeze()`，再赋值会静默失败。

   `const obj = Object.freeze({});`

   应尽多的使用`const`声明。

8. 查找标识符的顺序沿着作用域链，最终可达全局上下文的变量对象。

#### 4.3 垃圾回收

1. **标记清理**

   1. 标记内存中所有变量
   2. 去除上下文中的变量以及被上下文变量引用的变量的标记。
   3. 现有标记的就是待删除的，进行一次**内存清理**，销毁带标记的值并回收内存。

2. 引用计数

3. **内存管理**

   - 出于安全考虑，分配给浏览器的内存一般比较少，应保持较小的内存占用量。

     优化内存：**解除引用**：将不再必要的数据设置为null

   - 通过const和let声明提升性能；

   - 避免意外声明全局变量导致内存泄漏；

## 第6章 集合引用类型

#### 6.1 Object

1. 显式地创建对象：
   - new Object()
   - 对象字面量--常用
2. 所有现代浏览器都支持在对象的最后一个属性后加`,` ，尽管我不喜欢加。
3. 存取对象的属性
   - 点语法（首选）：`person.name`
   - 中括号（可通过变量访问属性or属性名包含特殊字符）：`person[name]`

#### 6.2 Array 



## 第8章 对象、类和面向对象编程

#### 8.1 理解对象

1. 对象的属性分为两种：

   **数据属性**和**访问器属性**

2. 内部特性用来描述属性的特征，用中括号括起来，如[[Enummerable]]

3. **数据属性**

   4个特性

   `[[Configurable]]` `[[Enumerable]]` `[[Writable]]` `[[Value]]` 

   - `[[Configurable]]`：是否可以delete删除；是否可以修改特性；是否可以转为访问器属性/数据属性；（直接定义在对象上的属性默认为`true`）

   - `[[Enumerable]]`：是否可以`for-in`遍历；（直接定义在对象上的属性默认为`true`）
   - `[[Writable]]`：属性值是否可以被修改；（直接定义在对象上的属性默认为`true`）
   - `[[Value]]`：属性值（默认为undefined  **惊现undefined的由来*）

   *** 注意**：如果调用了`Object.defineProperty()`了但不指定特性，默认都为`false` (对于访问器属性来说也是如此）

4. **访问器属性**

   4个特性

   `[[Configurable]]` `[[Enumerable]]` `[[Get]]` `[[Set]]` 

   - `[[Configurable]]`：同上

   - `[[Enumerable]]`：同上
   - `[[Get]]`：获取函数 (默认为undefined)
   - `[[Set]]`：设置函数（默认为undefined）

   访问器属性不能直接定义，只能通过`Object.defineProperty()`定义。

5. 示例

   ```js
   let book = {
       year_:2017,		//伪私有成员
       edition:1		//公共成员
   }
   Object.defineProperty(book,'year',{	//访问器属性year
       get(){
           return this.year_
       },
       set(newValue){
           this.year_ = newValue
       }
   })
   ```

6. 定义多个属性

   ```js
   let book = {}
   Object.defineProperties(book,{
       year_: {
           value: 2021
       },
       year:{
           get(){
               return this.year_
           },
           set(v) {
               this.year_ = v
           }
       }
   })
   ```

   其实跟`defineProperty`一样，不设置特性默认全为false，也可以手动在每个属性后面的描述对象里设置。

7. 获取属性的特性

   `Object.getOwnPropertyDescriptor(book,'year_')`获取book对象的year_属性的特性

   `Object.getOwnPropertyDescriptors(book)`获取book对象所有属性的特性

8. 合并对象

   ```js
   Object Object.assign(target,src,src1,src2)
   ```

   将源对象src, src1, src2 合并到目标对象 target 上，同时返回目标对象

   - 复制的是**可枚举属性**和**自有属性**；<span style="color:green">这里需要后续理解</span>
   - 执行浅复制，多个源对象有相同属性，使用最后一个值；
   - 访问器属性的值会作为静态值给目标对象； 即无法转移get()和set()；
   - 复制时出错，会直接中止而无法回滚
   
9. 相等判定

   与 `===` 相比，考虑了一些边界情况，如正确的NaN相等判定。

   `Object.is(NaN,NaN)` -> `true`

10. 增强的对象语法【**请默认使用**】

    - 属性值简写

      ```js
      let name = 'Matt'
      let person = {	//原来的写法
      	name:name
      }
      let person = {	//简写得到一样的结果
      	name
      }
      ```

    - 可计算属性

      使用可计算属性在对象字面量中直接动态命名属性，用中括号包围的对象属性键将在运行时作为JavaScript表达式求值

      ```js
      const nameKey = 'name'
      let person = {
      	[nameKey]:'Matt'
      }
      console.log(person) //{name:'Matt'}
      ```

    - 简写方法名

      ```js
      //旧方式：方法名、冒号、匿名函数表达式
      let person = {
      	sayName: function(name){
      		console.log(`My name is ${name}`)
      	}
      }
      //新方式
      let person = {
      	sayName(){
      		console.log(`My name is ${name}`)
      	}
      }
      //同样适用于获取函数和设置函数
      let person = {
      	name_:'',
      	get name(){
      		return name_
      	},
      	set name(name){
      		this.name_ = name
      	}
      }
      //可兼容可计算属性键
      const methodKey = 'sayName'
      let person = {
      	[methodKey](name){
      		console.log(name)
      	}
      }
      ```

11. 对象解构

    ```js
    let person = {
        name:'Matt',
        age:27
    }
    let {name:personName, age:personAge} = person
    console.log(personName,personAge) //Matt 27
    //若名称一样，可以使用简写
    let {name,age} = person
    //若引用的属性可能不存在，可以使用默认值
    let {name,job='Engineer'} = person
    
    //如果要给事先声明好的变量赋值，表达式必须包含在一对括号中
    let personName, personAge
    let person = {
        name:'Matt',
        age:27
    }
    ({name: personName, age: personAge} = person)
    ```

12. 对象解构拓展

    - 嵌套解构
    - 部分解构
    - 参数上下文匹配

#### 8.2 创建对象

​	创建对象的方式：1. new Object()； 2. 对象字面量；缺点是创建具有同样接口的多个对象需要重复编写很多代码

1. 概述

   ES5.1构造函数加原型继承；ES6支持了类和继承，是封装了之前规范的语法糖。

2. **工厂模式**

   可以创建多个类似对象，但没解决对象标识问题（新创建的对象是什么类型）
   
3. **构造函数模式**

   1. 可以创建**特定类型**的对象，通过构造函数创建对象：

   ```js
   function Person(name, age, job){
       this.name = name
       this.age = age
       this.job = job
       this.sayName = function (){
           console.log(this.name)
       }
   }
   let person = new Person('Matt',27,'Doctor')
   ```

   2. 使用`new`操作符时会进行：
      1. 在内存中创建一个新对象
      2. 对象内部的`[[Prototype]]`特性被赋值为构造函数的`prototype`属性
      3. 构造函数内部的`this`被赋值为这个新对象
      4. 执行函数内部的代码（给新对象添加属性）
      5. 返回创建的对象

   3. 按照惯例，构造函数名**首字母大写**，其他函数首字母小写

   4. 自定义构造函数可以确保实例被标识成特定类型（使用 `instanceof` 操作符确定对象类型）
   5. 若不想传参，构造函数后面的括号可加可不加

4. 构造函数也是函数

   与普通函数的唯一区别是调用方式不同，**任何函数只要用`new`操作符调用就是构造函数，不使用`new`操作符就是普通函数。

   调用一个函数而未明确设置this的情况下，this始终指向Global对象

5. 构造函数存在的问题

   构造函数定义的方法会在每个实例上都创建一遍，通过原型模式解决

6. **原型模式**

   每个函数都会创建一个`prototype`属性（不是[[prototype]]特性），这个属性是一个对象，就是通过调用构造函数创建的对象的原型。

   将属性和方法直接添加到函数的 `prototype`属性（`Person.prototype`）上，就将由所有实例共享。

7. **原型**

   先上两个原型关系图

   ![原型关系图](src\原型关系图.jpg)

   ![](src\构造函数_原型对象_实例关系图.png)

   1. 只要创建一个函数，就会有一个 `prototype`属性（指向原型对象），这个原型对象默认获得一个 `constructor`的属性，指回与之关联的构造函数。Person.prototype.constructor指向Person。
   
   2. 每次调用构造函数创建一个新实例，这个实例内部的 `[[prototype]]`指针就会被赋值为构造函数的原型对象。可以通过浏览器暴露的`__proto__`属性访问对象的原型。
   
   3. isPrototypeOf()会在传入参数的 `[[prototype]]`指向调用它的对象时返回true
   
   4. Object类有一个**`Object.getPrototypeOf()`**的方法，返回函数内部特性`[[prototype]]`的值。是ES5中得到对象的原型对象的**标准方法**，具有同样作用的**`__proto__`**属性只是浏览器实现的**非标准方法**
   
   5. 使用Object.setPrototypeOf()往实例的私有特性`[[prototype]]`里写入新值，可以重写一个对象的原型继承关系
   
      ```js
      //重写对象person的原型为biped，这个操作的影响极为深远：首先，不管属性有什么差别，都是直接替换掉的；更重要的是，所有继承此原型的都会被最后一次重写影响，因此也严重影响性能
      Object.setPrototypeOf(person,biped) 
      ```
   
      为避免性能影响，应使用`Object.create()`来创建一个**新**对象，同时为其指定原型（即这个方法的第一个参数）。
   
8. 原型层级

   1. 属性搜索

      通过对象访问属性时，会按照属性名称开始搜索。开始于对象实例本身，若没有则沿着原型对象往上找。

   2. 可以通过实例读取原型对象上的值但不能通过实例重写这些值，如果在对象创建与原型上同名的属性，就会**遮蔽**原型上对应的属性，把对象上的这个属性值设为**null**也不能恢复联系，除非使用`delete`操作符删除实例属性。

   3. **hasOwnProperty()**方法用于确定某个属性在**实例上**还是**原型对象**上。

      在属性存在于调用它的对象实例上时返回`true`

      ```js
      person.hasOwnProperty('name')//若name在person实例上，返回true，若只在原型上，返回false
      ```

   4. `in`操作符

      会在通过对象访问指定属性时返回true，无论是在实例上还是原型上。

      那么问题来了，如何确定某个属性是否存在于原型上？

      同时使用 `hasOwnProperty()`和`in`：

      ```js
      function hasPrototypeProperty(object, name){
      	return !object.hasOwnProperty('name') && (name in object)
      }	//不在实例上又能通过in访问到
      ```

   5. for-in循环

      可以通过对象访问并可以被枚举的属性都会返回(不管是在实例还是原型链上【也受到屏蔽的限制】)

   6. `Array[String] Object.keys(obj)`

      接收一个对象作为参数，返回所有可枚举属性名称的字符串数组（不走原型链）

   7. `Array[String] Object.getOwnPropertyNames(obj)`

      与6区别是能列出不可枚举的属性。

      6、7在适当的时候可以替代for-in，比如不需要管原型的时候。

   8. 属性枚举的顺序

      for-in 和 Object.keys()的枚举顺序是不确定的，取决于JavaScript引擎

      `Object.getOwnPropertyNames()、Object.getOwnPropertySymbols()、Object.assign()`的顺序是确定性的，顺序：

      1. [升序]枚举数值键
      2. [插入顺序]枚举字符串和符号键（在对象字面量中定义的键以逗号分隔的顺序为准）

9. 对象迭代

   略过

10. 其他原型语法

    ```js
    //定义原型属性和方法的封装方式，不必每次Person.prototype了，也存在问题（先不深究这里）
    function Person(){}
    Person.prototype = {
    	name:'Nicho',
    	age:29,
    	sayName(){}
    }
    ```

11. 原型的动态性

    从原型上搜索值是动态的，任何时候对原型所做的修改都会在实例上反映出来（即使实例在修改原型前已经存在）前面8.2-7-5也涉及到了。

    **原因**：

    实例和原型之间就是简单的指针，而不是保存的副本；

    如果重写原型切断和构造函数的联系，也不会改变**已创建**实例与**最初**的原型之间的联系，引用的仍然是最初的原型。

    **实例只有指向原型的指针，没有指向构造函数的指针**

12. 原生对象原型

    原生引用类型的构造函数（Object、Array、String）也在原型上定义了实例方法，而且也可以修改这些方法，但不推荐修改原生对象原型，推荐创建自定义的类继承原生类型。

13. 原型的问题

    所有属性在实例间共享：

    函数，很合适，就是要共享的；

    属性，不合适，原始值属性尚且可以用遮蔽解决问题，引用值不行。

#### 8.3 继承（理解为主，面试时再去抠细节记忆）

1. 继承的方式：接口继承、实现继承。ECMAScript函数没有签名，无法实现接口继承，主要通过原型链实现**实现继承**。

2. 原型链继承

   另一个类型的实例作为原型

   ```js
   SubType.prototype = new SuperType()
   ```

   问题：

   - 原型中包含引用值的时候；

   - 子类型实例化时不能给父类构造函数传参；

3. 盗用构造函数继承

   在子类构造函数中调用父类构造函数

   ```js
   function SubType(){
   	SuperType.call(this)
   }
   ```

   优点：

   - 可以在子类构造函数中向父类构造函数传参

   问题：

   - 必须在构造函数中定义方法
   - 子类也不能访问父类原型上定义的方法

4. 组合继承（使用最多的继承模式）

   综合原型链和盗用构造函数，使用原型链继承原型上的属性和方法，通过盗用构造函数继承实例属性。

   ```js
   function Parent (name) {
       this.name = name;
       this.colors = ['red', 'blue', 'green'];
   }
   Parent.prototype.getName = function () {
       console.log(this.name)
   }
   function Child (name, age) {
       Parent.call(this, name);
       this.age = age;
   }
   Child.prototype = new Parent();
   Child.prototype.constructor = Child;
   var child1 = new Child('kevin', '18');
   child1.colors.push('black');
   console.log(child1.name); // kevin
   console.log(child1.age); // 18
   console.log(child1.colors); // ["red", "blue", "green", "black"]
   var child2 = new Child('daisy', '20');
   console.log(child2.name); // daisy
   console.log(child2.age); // 20
   console.log(child2.colors); // ["red", "blue", "green"]
   
   ```

5. 原型式继承、寄生式继承、寄生式组合继承

#### 8.4 类

1. 类定义

   ```js
   类声明和类表达式
   class Person{}
   const Animal = class{}
   ```

   与函数表达式（回顾：函数声明可以被提升，表达式不可以，所以叫声明提升）类似，类表达式在被求值前也不能引用；

   但与函数表达式不同的是，函数声明可以提升，类定义（类声明）不可以；

2. 作用域

   函数受函数作用域限制，类受**块作用域**限制；

3. 类的构成

   可以包含函数构造方法、实例方法、获取函数、设置函数、静态类方法，但都不是必需的，空的类定义也有效。

   与构造函数一样，类首字母名称大写。

   类表达式名称可选，但不能在表达式作用域外访问。`let Person = class PersonName{}`

4. 类构造函数

   `constructor`在类定义块内部创建类的构造函数，告诉解释器使用new创建实例时，应调用这个函数。不定义将为空函数。

   1. 使用`new`操作符实例化Person的操作等于使用`new`调用其构造函数，只不过`new`和类用在一起时，JavaScript解释器知道应该用`constructor`函数进行实例化，`new`调用类的构造函数：

      - 在内存中创建一个新对象
      - 在这个新对象内部的`[[Prototype]]`指针被赋值为构造函数的 `prototype`属性；
      - 构造函数内部的`this`被赋值为这个新对象（this指向新对象）
      - 执行构造函数内的代码（给新对象添加属性）
      - 如果构造函数返回非空对象，则返回该对象，否则返回刚创建的对象。

      参见 #8.2-3-2

   2. 类实例化时传入的参数会用做构造函数的参数。若不需要参数，类名后的括号可选。

      参见 #8.2-3-5

   3. 类构造函数与构造函数的主要区别：

      调用类构造函数必须使用new操作符，而普通构造函数如果不使用new调用，就会以全局的this（Global，通常是window）作为内部对象

      类构造函数没有什么特殊之处，实例化之后会成为普通的实例方法，可以在实例上引用它（但由于是类构造函数，还是要使用new调用）

   4. ECMAScript类就是一种特殊函数

      在类的上下文中，类本身在使用new调用时就被当成构造函数，而此时类中定义的constructor不会被当成构造函数。

      ```js
      class Person {}
      let p1 = new Person()
      console.log(p1.constructor === Person)//true
      console.log(p1 instanceof Person)//true
      console.log(p1 instanceof Person.constructor)//false
      let p2 = new Person.constructor()
      console.log(p2.constructor === Person)//false
      console.log(p2 instanceof Person)//false
      console.log(p2 instanceof Person.constructor)//true
      ```

      类可以像其他对象或函数引用一样在任何地方定义（比如在数组中），也可以把类作为参数传递；

   5. 立即实例化类

      ```js
      let p = new Class Foo{	//表达式定义，所以类名Foo可选。p就是这个实例
      	constructor(x){
      		console.log(x)
      	}
      }('bar')	//立即实例化，会执行构造函数，打印出参数'bar'
      ```

   6. 类上的实例成员、原型成员、类成员

      1. 实例成员

         来源：new调用类标识符时执行类构造函数`constructor`，为实例（this）添加“自有”属性，不在原型上共享。

         ```js
         class Person {
             constructor() {
                 this.name = 'jack'
                 this.sayName = ()=>{
                     console.log(this.name)
                 }
             }
         }
         ```

      2. 原型方法和访问器

         来源：在类块中定义的方法作为原型方法

         constructor的this指向实例；

         类块中的this指向原型（其实是永远指向调用方法的对象，如果实例调用，那就是指向实例）（如这里的set访问器，this.name_是定义在原型上的，不过name属性在实例上）；

         静态方法的this指向类自身；

         ```js
         class Person {
             constructor(){} //会存在于不同的实例上
             //类块定义的会定义在类的原型上
             sayHello(){
                 console.log('Hello')
             }
             //等同于对象属性，可以用字符串、符号、或计算值作为键
             [symbolKey](){
                 console.log('invoked symbolKey')
             }
             ['computed'+'Key'](){
                 console.log('invoked computedKey')
             }
             //支持获取和设置访问器，跟普通对象一样
             set name(newName) {
                 this.name_ = newName
             }
             get name(){
                 return this.name_
             }
             //不能在类块中给原型添加原始值或对象
             //name:'Jake'   Uncaught SyntaxError:Unexpected token
         }
         ```

      3. 静态类方法

         静态成员使用`static`作为前缀，`this`引用自身，每个类只能被创建一次。

         静态类方法非常适合用作实例工厂，如下例。通常用于为一个应用程序创建工具函数。

         ```js
         class Person {
             constructor(age){
                 this.age = age
             } //会存在于不同的实例上
             static locate(){
                 console.log('class',this)
                 return new Person(13)
             }
         }
         let p = Person.locate() //class,class Person{}
         console.log(p.age) //13
         ```

      4. 非函数原型和类成员

         类定义并不显式地支持在原型或类上添加数据成员（但自己实践可以给类添加静态数据成员【之后在MDN上也看到这样的例子，不过后面也说“静态的或原型的数据属性必须定义在类定义的外面”】，不过说这是反模式，不应当使用），但在类定义外部，可以手动添加。

         ```js
         class Person{
             sayName(){
                 console.log(`${Person.greeting}${this.name}`)}}
         Person.greeting = 'My name is '
         Person.prototype.name = 'jake'
         let p = new Person()
         p.sayName() //My name is Jake
         ```

      5. 迭代器和生成器方法

         **\*【暂时略过】\***

   7. 继承

      1. `extends`单继承

         使用`extends`关键字，可以继承任何拥有[[construct]]和原型的对象，意味着可以继承类或者普通的构造函数

         ```js
         class Vehicle {}
         class Bus extends  Vehicle{}
         let b = new Bus()
         console.log(b instanceof Bus) //true
         console.log(b instanceof Vehicle) //true
         
         function Person(){}
         class Engineer extends Person{}
         let e = new Engineer()
         console.log(e instanceof Engineer) //true
         console.log(e instanceof Person) //true
         ```

      2. `super()`

         派生类通过 `super`关键字引用它们的原型，仅限于在类构造函数、实例方法、静态方法内部使用。

         - 在类构造函数中使用`super`可以调用父类构造函数；

           ```js
           class Bus extends Vehicle {
           	constructor(){
           		super() //相当于super.constructor
           	}
           }
           ```

         - 在静态方法中使用`super`调用继承的类上定义的静态方法

           ```js
           class Bus extends Vehicle {
           	static identify(){
           		spuer.identify()
           	}
           }
           ```

      3. `[[HomeObject]]`

         类构造函数和静态方法有特性`[[HomeObject]]`，指向定义该方法的对象，`super`定义为`[[HomeObject]]`的原型

      4. 使用`super`注意

         - 只能在派生类构造函数、实例方法、静态方法中使用
         - 不能单独使用`super`，要么调构造函数，要么静态方法
         - 调用`super()`会调用父类构造函数，并将返回的实例赋值给`this`
         - `super()`如同调用构造函数，可以给父类构造函数传参，`super(param)`
         - 如果没有定义类构造函数，会在实例化派生类时调用`super()`并传入所有参数
         - 在类构造函数中，不能在`super()`之前调用this
         - 如果派生类中显示定义了构造函数，则必须在其中调用`super()`，要么返回一个对象

      5. 抽象基类

         `new.target`保存通过`new`调用的类或函数，通过`new.target`检测以阻止实例化抽象基类，实现抽象类。

         也可以通过检查指定派生类必须定义的方法。（尽管跟java比起来相当奇怪，但也算实现了）

         ```js
         class Vehicle {
             constructor() {
                 if(new.target === Vehicle){
                     throw new Error('Vehicle cannot be directly instantiated')
                 }
                 if(!this.foo){
                     throw new Error('Inheriting class must define foo()')
                 }
             }
         }
         ```

      6. 类混入

         模拟多继承（略）

## 第10章 函数

​	函数是Function类型的实例，所以是一个对象，函数名为对象指针。

​	四种定义函数的方式

```javascript
function sum(num1, num2){
 return num1+num2;
}//函数声明的方式
```

```js
let sum = function(num1, num2){
 return num1+num2;
}; //函数表达式
```

```js
let sum = (num1, num2)=>{
 return num1+num2;
}; //箭头函数
```

```js
let sum = new Function("num1","num2","return num1+num2"); //Function构造函数，不推荐
```

#### 10.1 箭头函数

1. 可以使用函数表达式的地方都可以使用箭头函数；

2. 只有一个参数的时候，不需要括号；

3. 箭头函数不能使用`arguments` `super` `new.target`，不能用作构造函数，也没有`prototype`属性；

4. 函数体部分有省略大括号的情况，但注意：

   - 只能有一条语句；

   - 隐式返回这条语句的值；（相当于写了个return）

#### 10.2 函数名

1. 使用不带括号的函数名会访问函数指针，而不会执行函数。
2. 函数名就是指向函数的指针，一个函数可以有多个名称。

#### 10.3 理解参数

1. ECMAScript函数的参数只是为了方便才写出来的，不是必须；
2. 命名参数和arguments在内存是分开的，不过会保持同步，修改`arguments[0]`会修改对应命名参数`num1`的值，注意，在严格模式下不会影响，`num1`的值不变。
3. 箭头函数不能使用`arguments`关键字访问参数，只能通过命名参数访问；

#### 10.4 没有重载

1. ECMAScript函数没有签名，故而没有重载

#### 10.5 默认参数值

1. 显式定义默认参数

```js
function makeKing(name = 'Henry') { 
 return `King ${name} VIII`;
} 
```

箭头函数也可以用

```js
let makeKing = (name = 'Henry') => `King ${name}`;
```

2. 参数也存在于自己的作用域中，它们不能引用函数体的作用域：

#### 10.6 参数扩展与收集

1. 扩展参数

   ```js
   console.log(getSum(...values, ...[5,6,7]));
   ```

2. 收集参数

   没明白

#### 10.7 函数声明与函数表达式

1. JavaScript 引擎在任何代码执行之前，会先读取函数声明，并在执行上下文中生成函数定义，进行了**函数声明提升**。而函数表达式必须等到代码执行到它那一行，才会在执行上下文中生成函数定义。

   其他时候没有区别。

#### 10.9 函数内部

1. **arguments对象**

   一个类数组对象，包含调用函数时传入的所有参数。

2. **this对象**

   在标准函数中，this 引用的是把函数当成方法调用的上下文对象；

   在箭头函数中，this引用的是定义箭头函数的上下文。

3. **caller属性**

   引用的是调用当前函数的函数（函数的代码），如果是在全局作用域中调用的则为 null。

4. **new.target属性**

   如果函数是正常调用的，则 new.target 的值是 undefined；如果是使用 new 关键字调用的，则 new.target 将引用被调用的

   构造函数。

#### 10.10 函数属性与方法

1. 每个函数都有两个属性：

   `length` ：命名参数的个数；

   `prototype` ：保存引用类型所有实例方法，`toString()` `valueof()`都保存在`prototype`上，进而由所有实例共享

2. 还有两个方法：

   `apply()`: 会设置调用函数时函数体内 this 对象的值。apply()方法接收两个参数：函数内 this 的值和一个参数数组。第二个参数可以是 Array 的实例，但也可以是 arguments 对象。

   ```js
   let obj={
       name:"Cicy"
   }
   function f(){
       return this.name;
   }
   console.log(f.apply(obj));
   ```

   **作用**：调用函数；改变this值；

   接收的参数数组可以是 Array 的实例，也可以是 arguments 对象。

   `call()`向函数传参时，必须将参数一个一个地列出来，其他没区别。

   （还有一个`bind()`方法）；

#### 10.12 递归

1. 通常一个函数通过名称调用自己；

#### 10.13 尾调用优化

#### 10.14 闭包

1. **闭包**指的是引用了另一个函数作用域变量的函数，通常在嵌套函数中实现的。
2. 实现闭包的本质很简单，就是在一个函数内部定义的函数会把其包含函数的活动对象添加到自己的作用域链中。

3. *闭包中的this对象

#### 10.15 立即调用的函数表达式

1. 立即调用的**匿名函数**又被称作**立即调用的函数表达式**

   ```js
   (function(){
   }) ();//用来模拟块级作用域，ES6之后不需要这样
   ```

   另一个用途为锁定参数值；

#### 10.16 私有变量

1. JavaScript没有私有成员的概念（对象的属性都是公有），有**私有变量**的概念。任何定义在函数或块中的变量，都可以认为是私有的，因为外部无法访问。

   私有变量包括**函数参数、局部变量，以及函数内部定义的其他函数**。

2. **特权方法**是能够访问函数私有变量（及私有函数）的公有方法。

3. *模块模式

## 第11章 期约与异步函数

#### 11.1  异步编程

1. 异步操作是为了优化因计算量大而时间长的操作，但只要不想为了等待某个异步操作而阻塞线程执行，就可以使用异步操作。

2. 异步操作经常是必要的，强制进程等待一个长时间的操作通常是不可行的。

   eg：

   *代码要访问一些高延迟的资源，比如等待远程服务器的响应；*

   *监听事件，事件发生时触发执行某一函数；*

   *延迟一定的时间后执行某一函数；*

3. 异步代码不容易推断，异步操作由系统生成一个入队的中断，但入队的时机对JavaScript运行时是一个黑盒，无法预知（尽管可以保证发生在当前线程同步代码之后）。以往有这样几个模式来实现异步编程：

   - 定义回调函数来表明异步操作完成

     1. 异步返回值

        把`setTimeout()`的返回值传到需要的地方：

        ```js
        function double(value,callback){
            setTimeout(()=>{callback(value*2)},1000)
        }
        double(2,x=>{console.log(x)})
        ```

     2. 失败处理

        成功回调和失败回调

        ```js
        function double(value,success,failture){
        	XXXX
        }
        ```

     3. 嵌套异步回调

        如果异步返回值又依赖于另一个异步返回值，就要用嵌套回调，随着代码复杂，回调策略不具有可拓展性。

#### 11.2 期约

1. 期约是对尚不存在结果的一个替身。ES6新增引用类型`Promise`，通过`new`实例化，创建期约时需要传入执行器函数作为参数。

2. 期约状态机

   期约是一个有状态的对象，可能处于以下三种状态之一：

   - 待定（pending）
   - 兑现（fulfilled）
   - 拒绝（rejected）

   待定是一个期约最初的状态，可以落定（settled）为fulfilled或者rejected，落定后期约的状态就不再改变。

   期约**将异步行为封装起来，隔离外部的同步代码**，它的状态是私有的，不能直接通过JavaScript检测到。

3. 期约的作用

   - 提供期约的完成状态；
   - 提供期约改变时异步操作生成的值；

   期约改变时，若为兑现，有一个value，若为拒绝，有一个reason，都是可选的不可修改的引用，默认undefined

4. 通过执行器控制期约状态

   期约的状态是私有的，通过执行器函数控制期约状态，执行器的两项职责：

   - 初始化期约的异步行为；
   - 控制状态的最终转换；（通过 `resolve()`和`reject()`实现）

5. Promise静态方法：`Promise.resolve()`

   调用`Promise.resolve()`方法，可以实例化一个解决的期约，它的值（`value`）为传入的第一个参数。

   `console.log(Promise.resolve(0))`的打印出0，如果传入的参数本身是一个期约，它就相当于一个空包装，可以认为是一个幂等方法，利用这个幂等性可以保留传入期约的状态。

6. Promise静态方法：`Promise.reject()`

   实例化一个拒绝的期约并抛出一个异步错误（只能通过拒绝处理程序捕获），它的理由（`reason`）就是传入的第一个参数。

   不是幂等的，如果传入一个期约，这个期约将成为它的拒绝理由（`reason`）。

7. Promise静态方法：`Promise.all()`

   组合期约

   接收一个可迭代对象，返回一个新期约，这个期约会在包含的每个期约全部解决之后再解决，如果有一个拒绝，则合成的期约也会拒绝并将第一个拒绝的`reason`作为拒绝理由（不过并不影响包含的其他期约的正常拒绝操作）。

   ```js
   Promise.all([
       Promise.resolve(1),
       Promise.resolve(),
       Promise.resolve('val')
   ])
   .then((values)=>{
       console.log(values)
   })
   // 删除[ 1, undefined, 'val' ]
   ```

8. Promise静态方法：`Promise.race()`

   接收一个可迭代对象，返回一个新期约，这个期约是包含期约中最先落定的期约的镜像。

9. Promise**实例**方法：`Promise.prototype.then()`

   为期约添加处理程序，最多接收两个参数（处理函数onResolved函数和onRejected函数，会忽略非函数参数），分别在进入“兑现”和“拒绝”状态时执行，返回一个新的期约实例（无论`fulfilled`还是`rejected`均用`Promise.resolve()`包装），这个期约实例的`value`来自处理函数的返回值，默认为`undefined`，如果在处理程序中抛出了异常，返回值就是一个拒绝的期约（大概像这个样子`Promise.resolve(Promise.rejected())`）。

   **`then()`返回期约指的是把`then()`的处理函数返回的值包装成一个期约**

   ```js
   let p = new Promise(((resolve, reject) =>{
       console.log('first')
       resolve()
   }))
   let p2 = p.then(()=>{
       console.log('second')})
   //这个例子里，p.then()返回一个用Promise.resolve()包装的期约，但then()的处理函数并未返回任何值，所以p2为 Promise {undefined}，若
   let p3 = p.then(()=>{
       return 'p3'})
   //则p3为 Promise {'p3'}
   ```

   **所以如果要处理连续的Promise链，`then()`的处理函数应主动返回一个Promise给`then()`进行包装和返回，就应该是像下面p1.then()里写的那样：**

   ```js
   let p1 =new Promise(((resolve, reject) => {
       console.log('p1')
       setTimeout(resolve,1000)
   }))
       p1.then(()=>{
        //return Promise给p.then()
        return new Promise((resolve, reject) => {
           console.log('p2')
           setTimeout(resolve,1000)
       })
   }).then(()=>{
           console.log('p3')
       })
   ```

   **还有很多这样的写法，让我一度很迷惑：**

   ```js
   /*可以注意到没有return关键字，这是箭头函数的原因:
   箭头函数函数体部分有省略大括号的情况，但注意：
   - 只能有一条语句；
   - 隐式返回这条语句的值；（相当于写了个return）*/
   p1.then(()=>
        new Promise((resolve, reject) => {
            console.log('p2')
            setTimeout(resolve, 1000)
        })
   )
   ```

   

10. Promise**实例**方法：`Promise.prototype.catch()`

   相当于`Promise.prototype.then(null,onRejected)`

11. Promise**实例**方法：`Promise.prototype.finally()`

    [书里写的奇奇怪怪，不易理解，这里摘抄了MDNS上的描述]

    **`finally()`**方法返回一个[`Promise`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)。在promise结束时，无论结果是fulfilled或者是rejected，都会执行指定的回调函数。这为在`Promise`是否成功完成后都需要执行的代码提供了一种方式。

    这避免了同样的语句需要在[`then()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)和[`catch()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)中各写一次的情况。

12. 非重入期约方法【写到这的时候，因为书中难以理解的描述，转而研究了一波相关内容写了后面两个文章，现在再回过头继续看书】

    期约落定时，对应的处理程序（指then()、catch())仅仅会被排期（指扔进作业队列），即使添加处理程序在落定之前也是一样。

    这是JavaScript的非重入特性 ，适用于then()、catch()、finally()的处理程序

13. 传递解决值（resolve的value）和拒绝理由（reject的reason）

    ```js
    let p1 = Promise.resolve('foo')
    p1.then(value => {console.log(value)}) //foo
    let p2 = Promise.reject('bar')
    p2.catch(reason => {console.log(reason)}) //bar
    ```

14. 拒绝期约与拒绝错误处理

    期约可以以任何理由拒绝，但最好统一使用错误对象。

    ```js
    let p = new Promise (
    	(resolve,reject)=>{reject(Error'foo')})
    ```

15. 将期约组合起来

    期约连锁：期约一个接一个地拼接；

    期约合成：多个期约组合为一个 期约；

    1.每个期约实例的方法（`then()` `catch()` `finally()`）都会返回一个新的期约对象。

    ```js
    let p = new Promise(((resolve, reject) =>{
        console.log('first')
        resolve()
    }))
    p.then(()=>{
        console.log('second')})
    .then(()=>{
        console.log('third')
    })
    .then(()=>{
        console.log('fourth')
    })
    ```

    但这里例子里面的任务都是同步的，直接执行同步任务也可以，真正的用处是执行一串异步任务时。

    ```js
    new Promise((resolve, reject) =>{
        console.log('first')
        setTimeout(resolve,1000)
        })
        .then(()=>{
        return new Promise((resolve,reject)=>{
            console.log('second')
            setTimeout(resolve,1000)
            })
        })
        .then(()=>{
            return new Promise((resolve, reject) => {
                console.log('third')
                setTimeout(resolve,1000)
            })
        })
        .then(()=>{
            console.log('fourth')
        })
    ```

16. 更多

    串行期约合成

    期约拓展：取消期约

    期约拓展：期约进度通知

#### 11.3 异步函数

1. 使用Promise，在处理期约返回值时，必须把对应的代码写到then里的期约处理程序里去，非常不便。

2. `async /æˈsɪŋk/`

   `async`关键字用于声明异步函数，示例： 

   ```js
   async function foo(){}
   let bar = async function(){}
   let baz = async ()=>{}
   class Qux{
       async qux(){}
   }
   ```

   异步函数默认的返回值为`undefined`，使用`return`显式返回值。返回值会被`Promise.resolve()`包装成一个期约对象。

3. 在异步函数中抛出错误也是会返回拒绝的期约。

4. `await`的执行顺序为从右到左，不阻塞`await`表达式，但阻塞后面的代码

   await之后如果不是promise，await会阻塞后面的代码，会先执行async外面的同步代码，等外面的同步代码执行完成在执行async中的代码。

   如果它等到的是一个 promise 对象，await 也会暂停async后面的代码，先执行async外面的同步代码，等着 Promise 对象 fulfilled，然后把 resolve 的参数作为 await 表达式的运算结果。
   ————————————————
   版权声明：本文为CSDN博主「暖暖--」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
   原文链接：https://blog.csdn.net/qq_41681425/article/details/85775077

5. `await`必须在异步函数中使用

6. 实现sleep（）

   ```js
   async function sleep(delay){
   	return new Promise((resolve)=>{setTimeout(resolve,delay)})
   }
   ```

7. 平行执行（暂略）

8. 串行执行期约（暂略）

#### 尝试解释Promise的用处

1. 非重入期约方法

   奇奇怪怪的描述，看着烦。

   联系事件循环机制直接总结一波【下面的内容翻来覆去改了好几次，我打算推倒重来，综合看到的资料，试图讲清楚】：

   

   （图例主要讲队列和执行上下文，未区分微宏任务）

   ![JS运行时事件处理](src\JS运行时任务处理.svg)

   

   - 事件循环中，每一次事件循环从宏任务队列队首（FIFO）中取出一个任务执行，当这个任务执行完成后（任务退出，执行上下文为空），立即依次执行微任务队列中的微任务直至微任务队列为空（即使中途有微任务加入）；然后进入下一次事件循环。

   - 显然，setTimeout()是同步的，它的回调函数是异步的，并且是宏任务；

   - 异步会放到同步代码的后面执行；（其实都是放到任务队列依次取，关键是搞明白什么时候会放进任务队列，放进了哪个任务队列）

   - Promise()是微任务；//之前的写法（不准确）：Promise()是同步的，并且是微任务；

   - Promise()落定后的then()是微任务；//之前的写法:Promise()落定后的then()是异步的，但then()是微任务；

     请听题：控制台输出啥？

     ```js
     console.log('开始')
     setTimeout(() => {
         console.log('定时器0秒后的回调')
     }, 0)
     Promise.resolve()
         .then(() => {
             console.log('期约回调1')
         })
         .then(() => {
             console.log('期约回调2')
         })
     console.log('结束')
     ```

     答案非常清楚了叭

     ```js
     开始
     结束
     期约回调1
     期约回调2
     定时器0秒后的回调
     ```

     好的教育方式让人醍醐灌顶，辣鸡的书只会让人原地懵逼爆炸===

   

   重新讲：

   先梳理下Promise到底干嘛用的

   ```js
   const p = new Promise(
       (resolve,reject)=>{setTimeout(()=>{console.log(1);resolve();},1000)})
   .then(()=>{console.log(2)})
   ```

   首先Promise解决的痛点是：我们不知道异步操作什么执行结束，而Promise可以通知我们。

   从0开始理解：比如我们想要延迟一段时间执行一段代码，于是就把这段代码写到一个定时器里。

   ```js
   setTimeout(()=>{
       console.log('我想在1秒后执行这行代码')
   },1000)
   ```

   但我们还想要等这段代码执行完时执行别的代码，可我们不知道这段代码什么时候执行完，那怎么办？凭直觉讲，那就直接把别的代码写到那段代码后面就行了呗，像这样：

   ```js
   setTimeout(()=>{
       console.log('我想在1秒后执行这行代码')
       console.log('我想在前面的执行完之后执行')
   },1000)
   ```

   看似轻松的解决了问题，但如果别的代码还是一个异步操作呢

   ```js
   setTimeout(()=>{
       console.log('我想在1秒后执行这行代码')
       setTimeout(()=>{
           console.log('我想在前面的执行完后2秒执行')
       },2000)
   },1000)
   ```

   显而易见，会一层套一层，之前我写socket编程，就这样各种嵌套回调，代码变复杂，也更不好维护了。同时，每一个异步函数的执行上下文（作用域）就被限制在这个setTimout的回调里面了，多个回调之间就难以建立联系。

   那Promise是怎么通知我们异步操作结束了的呢？来实践一下。

   先把我们的异步操作放到Promise对象里面去：

   ```js
   let p = new Promise(
       (resolve, reject)=>{
           setTimeout(()=>{console.log('我想在1秒后执行这行代码')},1000)
       }
   )
   ```

   在new这个Promise时，构建器函数（就是`(resolve,reject)=>{}`）也会立即开始执行，我们的异步操作就在Promise中执行了。不过看起来好像和之前的方式没什么区别呀，就是放到了一个对象里面嘛，不着急，看构造器函数的参数：`resolve`和`reject`函数（由JavaScript引擎定义好的），这就是Promise的用处所在，这两个函数就是一个“通知”，我们在上述代码上做出修改：

   ```js
   let p = new Promise(
       (resolve, reject)=>{
           setTimeout(()=>{
               console.log('我想在1秒后执行这行代码')
               resolve()
           },1000)
       }
   )
   ```

   这里在第5行增加了一个`resolve()`，一旦执行了`resolve()`，就相当于发出了一个通知说“这个异步操作完成啦”，这个通知写在哪里是由你来定的，不过当然是要在异步操作执行结束后进行通知啦（这里不展开讲`reject()`了，同理）。那么，虽然通知发出去了，别的代码（`console.log('我想在前面的执行完之后执行')`）怎么知道上一个异步操作执行结束了呢？这就需要接收到那个通知，`then()`就是负责接收通知的，一旦`resolve()`了，就会进入`then()`里面，把别的代码写到`then()`里面就可以啦！（关于`then`的参数以及与`catch`的异同不展开了）

   ```js
   let p = new Promise(
       (resolve, reject)=>{
           setTimeout(()=>{
               console.log('我想在1秒后执行这行代码')
               resolve()
           },1000)
       }
   ).then(
       ()=>{console.log('我想在前面的执行完之后执行')}
   )
   ```

   而回调嵌套回调的代码也解开了嵌套，变成了：

   ```js
   let p = new Promise(
       (resolve, reject)=>{
           setTimeout(()=>{
               console.log('我想在1秒后执行这行代码')
               resolve()
           },1000)
       }
   ).then(
       ()=>{
           setTimeout(()=>{
               console.log('我想在前面的执行完后2秒执行')
           },2000)
       }
   )
   ```

#### 尝试讲解JavaScript事件循环中的微任务和宏任务

JavaScript在设计上是单线程非阻塞的，仅有的一个主线程通过事件循环机制（Event Loop）执行全部任务。需要留意以下的几点：

- 任务分为宏任务（Macrotask）和微任务（Microtask），分别位于宏任务队列，或称**任务队列（Task Queue）**和微任务队列，或称**作业队列（Job Queue）**，遵循先进先出的原则（FIFO）；（为免混淆，本文不使用加粗部分的表述，但它其实是更准确的说法）
- 执行上下文是一个栈，正在执行的任务位于执行栈（FILO）中；
- 每一次事件循环率先取出宏任务队列队首的那个宏任务进入执行栈，执行完毕后执行全部微任务，一次事件循环结束；
- 微任务的优先级更高，指的是执行完**一个宏任务**后执行**全部微任务（清空微任务队列）**，而非先做微任务再做宏任务；
- 宏任务的例子有这些：script（整体的代码）、`setTimeout`、 `setInterval`、 `setImmediate`、 I/O 任务等等；
- 微任务的例子有这些：`Promise.then()`、 `processes.nextTick` 等等，注意，Promise的执行器函数会在new Promise()时一起执行，执行器函数不是微任务，而是属于当前宏任务下的一个普通**同步函数**；



接下来就是喜闻乐见的判断代码输出顺序环节，通过这个简单的例子彻底解惑，**放弃尝试的同学可以直接看控制台输出，毕竟不是为了 考倒自己，而是学到东西**

```js
console.log('宏任务0开始') 
let p = new Promise((resolve, reject)=>{
    console.log('Promise执行器，包含在宏任务0内')
    setTimeout(()=>{
        console.log('Promise执行器里的异步函数，宏任务1')
        resolve()
        },0)
    })
p.then(() => {
       console.log('在p的resolve()后执行，微任务0')
   })
   .then(() => {
       console.log('在p.then的resolve()后执行，微任务1')
    })
setTimeout(() => {
    console.log('定时器0秒后的回调，宏任务2')
}, 0)
console.log('宏任务0结束')
```

 是游刃有余还是一头雾水呢？如果你尝试直接复制代码输出结果（图中红线代表一次宏任务的执行，绿线代表清空微任务队列，即执行微任务队列中的全部微任务）：

![](src\事件循环与任务优先级2.png)

控制台上输出的顺序竟然如此规律

1. 执行第一个宏任务
2. 清空微任务队列（此时队列空）
3. 执行第二个宏任务
4. 清空微任务队列
5. 执行第三个宏任务
6. 清空微任务队列（此时队列空）

看到这里，可能已经会豁然开朗了，我们结合这段代码，再把它的执行过程好好捋一捋（引用格式表示**当前任务队列**和**执行栈**状态）：

> 宏任务：script（整体代码）；	微任务：空；	执行栈：空；

1. 第一次事件循环开始，取宏任务队列队首的任务“script(整体代码)”，放入执行栈开始执行；

   - 第1行：控制台输出：*“宏任务0开始”*

   - 第2行：new Promise()时里面的执行器立即执行，①控制台输出：*“Promise执行器，包含在宏任务0内”*；②`setTimeout`开始计时器，其回调将在0秒后进入宏任务队列（注意**延迟结束后进入队列排队，不是立即执行，也不是进入队列再延迟**），由于这里的延迟是0秒，所以此时：

     > 宏任务：setTimeout()； 	微任务：空；	执行栈：script->Promise执行器；

2. 执行栈继续往后执行，`p.then()`是一个微任务，但它特殊在要等到`p`落定为`fulfilled`(这里是执行resolve()时)或`rejected`之后才会进入微任务队列。与`setTimeout()`的相同，都是异步的；与`setTimeout()`不同的是，`setTimeout()`等的是计时器，`then()`等的是Promise落定状态；以及`setTimeout()`进入宏任务队列，`then()`进入微任务队列。

   所以两个`then()`之后，此时的任务队列并没有发生变化。

3. 执行栈继续往后执行，遇到了第二个`setTimeout()`，同样，其回调将等待0秒后进入宏任务排队，由于是0秒，所以立即进入宏任务队列排队了。最后一行代码进入执行栈，控制台输出：*“宏任务0结束”*，此时：

   > 宏任务：setTimeout() 宏任务1 | setTimeout()宏任务2；	微任务：空；	执行栈：script->console.log()

4. 宏任务0执行结束了，去执行微任务队列中的全部微任务。诶，没有微任务呀，舒服了。至此，第一次事件循环结束。

   > 宏任务：setTimeout() 宏任务1 | setTimeout()宏任务2；	微任务：空；	执行栈：空

5. 第二次事件循环开始，取宏任务队列队首的任务“setTimeout() 宏任务1”，放入执行栈开始执行；控制台打印：*“Promise执行器里的异步函数，宏任务1”*，并执行`resolve()`。

   > 宏任务：setTimeout()宏任务2；	微任务：空；	执行栈：setTimeout的回调：console.log();  resolve();

6. `resolve()`时，Promise落定，在一边等待的`then()`终于可以入队了，所以`p.then()`进入微任务队列，注意这个时候，`p.then().then()`没有进入队列哦，还在等待`p.then()`的返回值。

   > 宏任务：setTimeout()宏任务2；	微任务：p.then()；	执行栈：空；

7. 宏任务1执行结束了，去执行微任务队列中的全部微任务。诶，这次有微任务了，将`p.then()`取出放入执行栈开始执行，控制台打印：*“在p的resolve()后执行，微任务0”*。

   > 宏任务：setTimeout()宏任务2；	微任务：空；	执行栈：p.then()：console.log()；

8. 最精彩的来了，我们知道，默认情况下，`p.then()`会返回一个value为`undefined`的已`resolve`的Promise给下一个`then()`，这就使得`p.then()`执行后，`p.then().then()`可以立即进入微任务队列。此时：

   > 宏任务：setTimeout()宏任务2；	微任务：p.then().then()；	执行栈：空；

   诶呦，这中途加进来的可怎么处理呀，记住：微任务的执行原则是：**依次执行微任务队列中的微任务直至微任务队列为空（即使中途有微任务加入）**，所以这个中途加进来的微任务也需要执行，控制台打印：*“在p.then的resolve()后执行，微任务1”*。这时，微任务队列清空了，第二次事件循环结束。

   > 宏任务：setTimeout()宏任务2；	微任务：空；	执行栈：空；

9. 第三次事件循环开始，取宏任务队列队首的任务“setTimeout() 宏任务2”，放入执行栈开始执行；控制台打印：*“定时器0秒后的回调，宏任务2”*，执行完后出栈。此时：

   > 宏任务：空；	微任务：空；	执行栈：空；

10. **亦可赛艇，历经三次事件循环，全部执行完了！**看似非常复杂，但这么理一理，还是可以很清楚的。由这个例子，触类旁通，再怎么变也不怕了。



最后奉上这段代码的注释纯享版：

```js
console.log('宏任务0开始')  //本次宏任务开始
let p = new Promise((resolve, reject)=>{
    //Promise执行器里的同步函数，new Promise时立即执行（包含在本次宏任务内，不会跳出当前执行栈）
    console.log('Promise执行器，包含在宏任务0内')
    setTimeout(()=>{
        //Promise执行器里的异步函数，是一个宏任务（Macrotask），跳出当前执行栈，0ms后进入任务队列（Task Queue）成为宏任务1，这个0ms的延迟就决定了相比宏任务2进入任务队列的先后，如果保持宏任务2的0ms延迟不变，将宏任务1的延迟改为1000ms，宏任务2将排在前面率先执行
        console.log('Promise执行器里的异步函数，宏任务1')
        resolve()
        },0)
    })
p.then(() => {
      //在p的resolve()后执行，是一个微任务（Microtask），跳出当前执行栈，进入作业队列（Job Queue）
      console.log('在p的resolve()后执行，微任务0')
  })
  .then(() => {
      //在p.then()的resolve()后执行，是一个微任务（Microtask），跳出当前执行栈，进入作业队列（Job Queue），排在上一个微任务后面
      console.log('在p.then的resolve()后执行，微任务1')
    })
setTimeout(() => {
    //定时器0秒后的回调，是一个宏任务，在0ms的延迟后进入任务队列，排在上一个宏任务1后面
    console.log('定时器0秒后的回调，宏任务2')
}, 0)
console.log('宏任务0结束')
```

 ![](src\事件循环与任务优先级.png)

## 第12章 BOM

#### 12.1 window对象

1. BOM的核心是window对象，表示浏览器的实例。

   两重身份：ECMAScript中的Global对象；浏览器窗口的JavaScript接口。

2. 网页中定义的所有对象、变量和函数都以 window 作为其 Global 对象，都可以访问其上定义的 parseInt()等全局方法。

3. window对象被复用为 ECMAScript 的 Global 对象，所以通过 var 声明的所有全局变量和函数都会变成 window 对象的属性和方法。如果使用 let 或 const 替代 var，则不会把变量添加给全局对象。

4. 窗口关系

   window.parent、window.top 和 window.self

   top对象指向最外层窗口，即浏览器窗口本身。

   parent对象指向当前窗口的父窗口。

   self和window是同一个对象
   
   
