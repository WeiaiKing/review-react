## reate知识

### react 项目规范

* 文件夹，文件名同一个大小写，多个单词可连接符（-）连接；
* JavaScript变量名称采用小驼峰标识，常量全部使用大写字母，组件采用大驼峰；

* css 采用普通css和styled-component结合来编写
  * 全局采用普通css,局部采用styled-component

* 整个项目不再使用`class`组件，统一使用函数式组件，并且全面拥抱`Hooks`；

* 所有的函数式组件，为了避免不必要的渲染，全部使用memo进行包裹
* 组件内部的基本状态按照如下顺序编写代码：
  * 组件内部`state`管理；
  * `redux`的`hooks`代码；
  * 其他组件`hooks`代码；
  * 其他逻辑代码；
  * 返回JSX代码；

* redux代码规范如下：
  * 每个模块有自己独立reducer,通过combineReducer进行合并
  * 异步请求代码使用re