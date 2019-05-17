# interview-question
### ES6模块化如何使用？开发环境如何打包？
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
