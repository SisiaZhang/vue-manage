## 后台管理系统

[b站后台管理项目](https://www.bilibili.com/video/BV1QU4y1E7qo?p=1&vd_source=a605ce6be49dfd9087de4af3bedc5b50)

<img src="E:\04learn\06 forth\project_test\22.8.4vue-houtaiadmin\ziyuan\images\8.png" alt="8" style="zoom: 50%;" />

### 1技术栈

#### 1.1 vue-router

在根目录下创建router文件夹（index.js），专门管理项目路由，main.js中引入vue-router。

index.js中，引入vue和vue-router，将vue-router全局引入，进行初始配置；就可以给首页写一个路由；在App.vue中放置路由容器`<router-view></router-view>`；

设置路由跳转，使用编程式导航$router.push()；在Main主页中使用嵌套路由，显示主体区域；

#### 1.2 vuex

store文件夹中引入vue和vuex，创造新的vuex实例；把引入的vuex实例注入到根组件中；这里的vuex使用modules模块，包括tab和user模块；

tab模块中，state包括isCollapse、tabList、currentMenu、menu。mutations包括collapseMenu（将控制侧边栏缩放状态的数据取反）、selectMenu（改变tabList内容，点击菜单栏就多一个tab（且当前点击高亮））、closeTag（点击tag的关闭按钮，数据源state也随之改变，只能通过mutation来改变）。使用$store.state.xxx取数据，$store.commit()调用方法

#### 1.3 axios

axios是基于promise的http库，有几个好处（支持promiseAPI，可以避免回调地狱；可以在请求和响应前进行拦截；支持防御XSRF的攻击）

npm安装axios；全局引入，axios不是插件，想在全局中使用，需要绑定在Vue的prototype属性上

调用class类：将接口请求都写在data.js中

#### 1.4 element-ui

分为全局引入和按需引入，全局引入会下载很多不必要的组件，影响性能，真实项目中应该用按需引入。

#### 1.5 二次封装axios

需要将项目的配置逻辑添加到axios的请求中，通过配置来改变接口请求的地址，同时对所有接口进行监听，在请求前和请求后进行对应的拦截（在请求前添加统一的header，请求后捕获全局的catch，同时对异常的http状态进行处理）

将axios生成一个工具类，在api文件夹下新建axios.js文件；配置文件config中定义相关配置，baseUrl定义开发环境和生产环境

首先对当前环境变量进行判断；再写axios的工具类，class语法；getInsideConfig() 定义axios的相关配置；interceptors(instance)拦截器；request(options) 后续接口请求时，会调用此函数

总结：这只是简单封装，最主要的功能是将配置文件与axios进行结合

#### 1.6 mock

可以拦截ajax请求，并且在回调函数中通过自定义数据模板，或者说在回调函数中直接返回接口的响应数据，用于模拟后台返回的接口。

mock.js下对ajax请求进行拦截，mockServeData文件夹存放所有数据

#### 1.7 echarts

需要为echarts准备一个定义了宽高的DOM；在script中调用echarts的init方法，传如DOM节点；指定图表的配置项和数据；

echarts也有全局引入和按需引入

#### 1.8 其他

- less：npm下载less、以及less解析器less-loader

### 2项目的搭建

#### 2.1项目架构分析

首先在 登陆页面 进行登录，登录之后展示 首页 ，左上角是用户展示区（显示用户名称和权限名称；上次登录时间和登陆地点）；左下角是table列表，展示数据；右上角是订单的统计数据；右下角是echarts图标

顶部是header区域（左侧有一个折叠按钮和面包屑，右侧是用户头像，可以点击登出）

左边是菜单栏，点击菜单跳转到不同的页面，同时header下方有一个tabs，点击tabs可以进行跳转删除等操作，tab与面包屑有联动；菜单栏有一级菜单和二级菜单

用户管理页面中，可以新增用户、对页面的数据进行编辑；用户的数据是分页的列表，可以点击分页进行切换；顶部有搜索框，可以使用姓名做数据层面的过滤

进行登出，系统也做了权限的控制，非管理员账号只能看到首页和商品管理

#### 2.2项目模块搭建

#### 2.3脚手架搭建配置

1. yarn和npm的对比：

   - npm的坑：`npm i`时慢；包的版本号无法保持一致；安装时，所有包同时下载安装，导致包抛出错误时在终端发现不了

   - yarn的优点：速度快（并行安装，npm是按队列执行每个package）；离线模式（有缓存）；安装版本统一；更简洁的输出；多注册来源处理；语义化

   基于上，选择用yarn包管理工具（实际上，就开始的时候用了，后面也没用啊？）

2. vue-cli4脚手架搭建

   - 首先需要nodejs环境（通过安装包安装nodejs环境时，会自带npm环境）
   - 安装cnpm和yarn：来替代npm进行包管理
   - 安装vue-cli脚手架：用cnpm来安装
   - 创建项目：`vue create 项目名称`；选择自己想要的插件（这里选默认的vue2+babel+eslint）,此时项目基本文件和依赖项都生成和下载好了
   - 启动项目：`npm run serve`

#### 2.4组件初始化

#### 2.5路由初始化

#### 2.6vuex初始化

### 3模块分配

#### 3.1登录页

路由中定义登录页的路由；使用element-ui的form表单，对表单进行校验

#### 3.2后台首页

使用element-ui的Container布局；

侧边栏使用element-ui的NavMenu组件。因为每个页面都要用到侧边栏，故将其抽离成一个公共组件，定义在components文件夹下CommonAside.vue；添加数据，后面是用mock模拟的，对有子项目的菜单和无子项目的菜单分别使用计算属性；设置路由跳转，使用编程式导航，并且，在Main主页中使用嵌套路由，显示主体区域；

header部分也抽离成一个公共组件。左侧按钮的样式用element-ui的icon来实现，右侧下拉菜单用element-ui的dropdown。左侧按钮点击实现侧边栏收缩，而侧边栏收缩是通过iscollapse数据控制，用vuex来实现兄弟组件间的通信；

主体区域home页面 使用element-ui的Layout布局，card卡片显示登录信息（这里数据是写死的，要做优化），table组件；图表数据用mock模拟（getData()），这里echarts还封装了公共组件，我没有做

面包屑和tag区域：面包屑的数据用vuex存储；header区域使用element-ui的breadcrumb组件，在computed中将vuex属性注入；tag区域用element-ui的tag标签组件，将tag抽离成公共组件，对tag删除时，vuex中的数据源也要相应改变

#### 3.3用户管理页

顶部form表单区域，使用element-ui的form表单组件，封装CommonForm组件；当表单内容填写完后点击确认按钮，需要将数据上传，这里使用mock模拟拦截数据（createUser、updateUser分别处理数据）；

table表格区域，封装成CommonTable组件，其中使用element-ui的table组件；注意父子组件之间的通信，用props和this.$emit；

#### 3.4分页处理

table下面使用element-ui的pagination组件

#### 3.5用户crud

#### 3.6路由守卫

什么时候校验token是否存在？在路由跳转的时候，可以用导航守卫来监听token是否存在，

#### 3.7权限管理

填写用户名和密码后，点击登录将用户名和密码传给后端，后端与数据库进行匹配，匹配后返回一个登录的凭证（token），前端将这个凭证缓存起来，登陆的时候将token传给后端验证，这样便建立起了登陆权限体系。

store中管理token，将token缓存的时候依赖这个js-cookie库；

使用vue-router的动态添加路由的方式，将之前写死的路由全部替换；permission中用mock模拟了接口数据

### 项目讲解大纲

#### 项目搭建

#### element-ui的使用

#### vue路由的使用

#### 首页的搭建

#### 左侧菜单栏

#### header组件（vuex实现左侧折叠）

#### home组件（axios二次封装、mock模拟数据、Echarts公共组件、面包屑和tag）

#### form组件（table组件）

#### 登陆页面

### 日常笔记

#### 8.4

- 下载依赖项：`npm i element-ui -S`中`-S`相当于`--save-dev`
- `Vue.use( plugin )`：vue安装插件
- element-ui：不要全局引入，会占用体积，多很多无用的组件，应该按需引入
- `npm run build`：将项目打包，会生成dist文件夹，通过查看dist文件夹大小等，可看到element-ui全局引入和按需引入的差别
- 问题：组件的命名（eslint要求multi-word）和路由的命名可以不一样，之间有什么关系吗？
- vscode代码对齐快捷键： 选中文本； Shift + Alt + F 实现代码的对齐
- `height: 100vh`：viewpoint height，视窗高度，1vh=视窗高度的1%
- 自动加空格问题：如`:class="`el-icon-${item.icon}`"`可以用模板字符串解决

#### 8.5

- 路由命名一定要大写吗？
- 调转到嵌套路由：父页面和嵌套的children页面都会显示

#### 8.6

- 为什么调用vuex中模块的state时要用模块名，而调用方法不需要模块名。如`this.$store.state.tab.isCollapse;`和`this.$store.commit("collapseMenu");`
- axios：不是插件，不能`Vue.use(http)`，而是将其绑定在Vue的prototype属性上`Vue.prototype.$http = http;`
- 接口请求：一般是在生命周期的mounted()

#### 8.7

- mock：[官方文档](http://mockjs.com/)
- 问题：不知为什么，跟着把axios二次封装后，请求就发不出去了，卡住了

#### 8.9

- axios二次封装的时候，将拦截器部分复制过来就好了，不知道是我自己写的时候，哪里出错了？
- echarts：[文档](https://echarts.apache.org/handbook/zh/get-started/)
- 获取DOM元素：使用`ref`属性，在js中使用`this.$refs`，如![image-20220809144429594](C:\Users\sissiz\AppData\Roaming\Typora\typora-user-images\image-20220809144429594.png)![image-20220809144333546](C:\Users\sissiz\AppData\Roaming\Typora\typora-user-images\image-20220809144333546.png)。这个是什么原理呢？ES6？
- import引入：需要复习一下，`import * as echarts from 'echarts'`无版本的引入？是什么格式
- git使用：弹幕里提醒边做项目边git，以便不小心就没了。需要复习一下git的使用
- vuex：了解了mapstate的使用，mapstate调用之后返回一个对象，所以可用...解构
- 面包屑：教程里面包屑的使用及定义好像都有点问题，可以自己完善一下

#### 8.10

- cursor属性：定义了鼠标指针放在一个元素边界范围内时所用的光标形状（pointer 光标呈现为指示链接的指针（一只手））
- vuex：调用vuex里的方法时，除了用`this.$store.commit`之外，还可以用`mapMutations`

#### 8.11

- 父子组件数据传递：props传递的数据最好只读，修改的话，如果修改基本数据类型，会报错，修改对象或者数组种的数据，不会报错，但是不建议；可以将props的内容赋值到data之后再修改
- mock拦截：可以指定url地址，也可以使用正则
- vscode快捷方式：vue组件用`<v`
- 问题：一个项目里已经用了vuex了，还需要用父子组件传递这些吗？
- 插槽：table组件中插槽的用法还不是很懂

#### 8.12

- bug：路由设置login那里，不知道怎么搞的页面就是出不来，然后把login和Main换了个位置就出来了，不知道怎么回事
- 使用js-cookie：[文档](https://github.com/js-cookie/js-cookie)
- 使用导航守卫：拦截token；在里面先获取token值得原因是防止页面刷新之后vuex丢失了token值的信息

#### 8.13

- 动态添加路由：使用router的动态路由处理方式，将menu动态添加到路由中
- cookie：cookie只能储存字符串，不能存对象，如果val是对象的话，要用`JSON.stringify(val)`转换一下