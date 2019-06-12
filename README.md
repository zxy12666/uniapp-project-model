# wechatTradeCode
一款基于uni-app编写的app登录模板，优雅的解决了登录拦截问题，也可作为项目基础模板使用。
支持H5，小程序，安卓，ios端。支持的功能：登录拦截方案，路由二次封装，图片统一管理方案，api统一管理方案，icon通用方案，页面loading组件，过滤器的解决方案，可配置启动时登录，也可配置非启动环境下强制登录或非强制登录。


## 运行方式
将项目拖入[HbuilderX](http://www.dcloud.io/hbuilderx.html),直接运行即可

## 项目访问说明
* 浏览器中访问请在url中带openId=123参数，比如：[http://localhost:8080/converge-official-page/#/?openId=123](http://localhost:8080/converge-official-page/#/?openId=123)，登录时手机号随意填写，登录正确验证码为000000，项目接口都是mock数据。
* 在线mock数据：[http://easy-mock.liuup.com](http://easy-mock.liuup.com)，非常好用，非常方便，推荐同学们也可以去使用。
* H5版在线预览地址：[http://liuup.com/converge-official-page/#/?openId=123](http://liuup.com/converge-official-page/#/?openId=123)。


## 特点
* 兼容H5、微信小程序和安卓、ios
* 适用于强制登录和非强制登录应用场景
* 使用vuex管理登录状态


## 开发规范
* 1.组件名以m做为前缀。
* 2.在vue原型挂载对象，以$为前缀。
* 3.scss预编译
* 4.es6、Promise、async和await。

## 项目架构方案

* 静态资源解决方案（图片、字体）；
静态资源需要支持服务器地址或着本地地址配置，在config目录中新增assets.config.js文件，创建json对象，达到统一管理的目的，挂载到vue原型中$assets，所有页面通过this.$assets.imgKey访问图片url。css背景图片应尽量使用行内样式设置。

* api集中管理；
为简化逻辑代码量整洁的原则，像调用函数一样调用api，做到代码分离，在apis目录创建http.js封装request请求方法，在新建index.js中引入http.js，统一创建api函数，并且封装接口返回数据类型校验的方法，挂载到vue原型中，页面通过await this.$apis.apiName()调用，数据校验通过await this.$apis.utils.isArray(data)方法，注意：尽量使用es7的 async 和 await 的特性实现同步。

* uni-app实现登陆拦截解决方案；
由于框架没有全局路由守卫函数，那么要实现登陆拦截需要在common目录中创建Router.js封装路由api，在config目录中创建权限路由表，两者配合以达到路由跳转权限控制目的。

* icon图标跨端通用解决方案。
统一使用字体图标库，跨端兼容好，也是官方推荐的，注意：字体文件太大建议做分割处理，避免加载过慢的情况。

* pages目录分包管理
由于微信小程序的限制，上传发布机制总包大小不能大于2m，所以项目若超出该限制，要在page.json中做分包处理，分包处理的配置与pages目录保持一致。

* AppEnterControll.js 应用的全局类，继承vue的所有方法。
场景：app入口控制，用户登陆。非页面自有方法。
中心思想：类似小程序app.js

* joy-page组件说明
中心思想：组件代表一个page，可随意扩展，已扩展页面loading，网络异常重载操作。
场景：每个页面按需求引入。页面内容通过slot渲染，loading通过props控制。
