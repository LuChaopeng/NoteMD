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

6. 执行到`try/catch`语句中的`catch`块，会在作用域链前端添加一个变量对象。创建一个新的变量对象，这个变量对象会包含要抛出的错误对象的声明。

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

         类块中的this指向原型（如这里的set访问器，this.name_是定义在原型上的，不过name属性在实例上）；

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

         静态类方法非常适合用作实例工厂，如下例。

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

         类定义并不显式地支持在原型或类上添加数据成员（但自己实践可以给类添加静态数据成员，不过说这是反模式，不应当使用），但在类定义外部，可以手动添加。

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

