# interview-question
### 1.ES6模块化如何使用？开发环境如何打包？
> **模块化的基本语法**
  1.每一个模块只加载一次，每一个JS只执行一次，如果下次再去加载同目录下同文件，直接从内存中读取。一个模块就是一个单例，或者说一个模块就是一个对象
  2.每一个模块内声明的变量都是局部变量，不会污染全局作用域
  3.模块内部的函数或者变量可以通过export导出
  4.一个模块可以导入到别的模块

> **export语法**
  -如果一个js模块文件只有一个功能，那么就可以使用export default导出

> **import语法**
  -如果以export default导出，则对应的import直接接上文件名
  -如果导出的模块里面封装多个功能，则需要在import {...}大括号里面添加你需要导入的内容

***开发环境打包之前需要先进行一些开发环境的配置***
**开发环境的配置---使用babel**
*why？*
> es6中新增了箭头函数、块级作用域和symbol、promise等新语法和数据类型，但是现在的浏览器并不能很好的支持，所以就需要一个转译器，babel就是做这个工作的
安装babel---[点击查看文档](https://www.jianshu.com/p/ceb121445a99)

**注意，还需要安装webpack，因为babel-loader是根据webpack标准写的转码器**
安装webapck[参考文档](https://www.jianshu.com/p/ceb121445a99)   
[关于以上问题还可以点击参考该文档](https://www.javascriptcn.com/read-46936.html)

### 2.浏览器是原生支持base 64编码和解码的
> 如果有时候我们需要把一段字符文本进行前端的简单加密。比如加密为base64，作为前端我们该如何实现呢？*难道真要傻乎乎地引入外部文件(诸如[base64.js](https://github.com/dankogai/js-base64))*，答案显然是no。  
  因为实际上，从IE10+浏览器开始，所以的浏览器都原生提供了base64的编码和解码方法，方法名就是**atob**和**btoa**  

**base64 编码**
``` window.btoa && btoa('robin lei')```
**base64 解码**
```window.atob && atob('cm9iaW4gbGVp');```

**注意，在对中文进行base64转换时会报错**
> *解决方法*
  > <i>就是先将中文encode再将其decode</i>
  ```
    let tmp = btoa(window.encodeURIComponent('雷仔')) // JUU5JTlCJUI3JUU0JUJCJTk0
    window.decodeURIComponent(atob(tmp)) // 雷仔
  ```

  > *还可以这样做*  
    >
      ```
        let tmp = btoa(unescape(encodeURIComponent('雷仔'))) // 6Zu35LuU
        decodeURIComponent(escape(atob(tmp))) // 雷仔
      ```

**那么对于低于IE10的浏览器怎么办呢？莫非要放弃这一块市场蛋糕？答案肯定是no way**
> 我们可以针对这些浏览器专门在引入一段ployfill脚本或者直接引入一个polyfill.js文件
  [点击这里查看polyfill JS脚本](https://github.com/davidchambers/Base64.js/blob/master/base64.js)   
  [点击这里直接下载polyfill JS脚本文件](https://www.zhangxinxu.com/study/201808/base64-polyfill.js)   
  *在实际的项目中该怎么使用呢？*
  > 直接借助ie的条件注释实现无缝衔接
  ```
    <!-- [if IE]>
      <script src='./polyfill.js'></script>
    <![endif]-->
  ```

***其实，利用FileReader对象，我们可以把任意的文件转换为Base64 Data-URl***
>
  ```
    new FileReader().onload = function(e) {
      // e.target.result 就是文件的Base64 Data-URI
    }
  ````

### 3.class与普通构造函数有什么区别？
- class在语法上更加贴合面向对象的写法(class可以说是构造函数的语法糖)
- class在实现继承上更加易读也更加容易理解
- 本质上还是语法糖，还是使用prototype
- class的本质还是函数

### 4.再谈undefined和null的区别
> - 一般情况下，对于大多数计算机编程语言来说，通常只有一个表示'无'的值，但是对于JavaScript来说有点特殊，居然有两个表示'无'的值，undefined和null，why？
  - 众所周知，undefined == nul ==> true
  - 将一个变量赋值为undefined和null，这两种写法几乎等价
  **原因分析**
  - 在《Speaking JavaScript》中写到，在1995年js诞生时，和Java一样，只有一个null表示"无"
  - 根据C语言传统，null被设计为可以自动转为0
  - JavaScript的设计者Brendan Eich觉得这样做还不够
    > - null像在Java里面一样被当做一个对象，但是JavaScript的数据类型分为原始和复杂两种，设计者认为，表示'无'的值最好不是对象
      - 其次，JavaScript的最初版本并没有包括错误处理机制，发生数据类型不匹配时，通常是自动类型转换或者默默认为失败，Brendan Eich觉得，如果null自动转为0，很不容易发现错误
      - 综上，Brendan Eich又设计了一个undefined
  **最初设计**
  > JavaScript的最初版本是这样区分的，null是一个表示'无'的对象，转为数值时为0，undefined是一个表示'无'的原始值，转为数值时为NaN
  **目前用法**
  > null表示'没有对象'，也就是说该处不应该有值
    *用法：(1)作为函数的参数，表示该函数的参数不是对象;(2)作为对象原型链的终点，Object.getPrototypeOf(Object.prototype) // null*
    undefined表示'缺少值'，也就是说此处应该有一个值，但是还没有定义，常见下面几种情况：
    - 变量声明但是没有赋值
    - 调用函数时，应该提供的参数没有提供，此时该参数等于undefined
    - 对象没有赋值的属性，默认该属性值为undefined
    - 函数没有返回值时，默认返回undefined
