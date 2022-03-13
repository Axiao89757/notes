# 1. 项目创建

1. 怎么都装不上，最后 **安装了 yarn，然后 npx create-react-app todolist-plus 给装好了**

   教训：

   >1. 必须以**管理员身份运行cmd**
   >2. 可能得安装 python 和 visual studio 2019 （不一定）

# 2. 代码问题

1. propTypes方法无法检测

   > 编辑器上不会报错的，同时不影响页面渲染，但是控制台会报错
   
2. **styled components** 错误使用造成动态创建组件，输入框只能输入一个字符，然后就刷新失去焦点

   > StyledComponents 的定义不能写在render()方法内，也不能写在方法组件内

   