# Vue-Element-Node.js

`Vue-Element+Node.js`开发企业通用管理后台系统

```
// vue.config.js

proxy: {
	[process.env.VUE_APP_BASEl_API]: {
		target: `http://127.0.0.1:${port}/mock`,
		changeOrigin: true,
		pathRewrite: {
			['^' + process.env.VUE_APP_BASE_API]: ''
		}
	}
},
```

```
// +字符串转数值 !!转布尔值

const state = {
	sidebar: {
		opened: Cookies.get('sidebarStatus') ? !!+Cookies.get('sidebarStatus') : true, withoutAnimation: false
	},
	device: 'desktop',
	size: Cookies.get('size') || 'medium'
}
```

```
const TokenKey = 'Admin-Token'
export function getToken() {
	return Cookies.get(TokenKey)
}
export function setToken() {
	return Cookies.set(TokenKey)
}
export function removeToken() {
	return Cookies.remove(TokenKey)
}
```

```
created() {
	this.$on('my_events', this.handleEvent)
	this.$on('my_events', this.handleEvents2)
	
	// this.$on(['my_events', 'my_events2'], this.handleEvents)
	// console.log(this._events)
}
methods: {
	handleEvents(e) {
		
	},
	handleEvent2(e) {
		
	},
	boost() {
		this.$emit('my_events', 'my ssss')
	}
}
```

```
router.beforeEach(async(to, from, next) => {
	NProgress.start()
	// start progress bar
	
	// set page title
	document.title = getPageTitle(to.meta.title)
	
	// determine whether the user has logged in
	const hasToken = getToken()
	
	if (hasToken) {
		if (to.path === '/login') {
			// if is logged in, redirect to the home page
			next({ path: '/' })
			NProgress.done()
		} else {
			const hasRoles = store.getters.roles && store.getters.roles.length > 0
			if (hasRoles) {
				next()
			} else {
				try {
					// get user info
					const { roles } = await store.dispath('user/getInfo')
					
					const accessRoutes = await store.dispath('permission/generateRoutes',)
					
					router.addRoutes(accessRoutes)
					
					next({ ...to, replace: true })
				} catch (error) {
					await store.dispatch('user/resetToken')
					Message.error(error || 'Has Error')
					next(`/login?redirect=${to.path}`)
					NProgress.done()
				}
			} 
		}
	} else {
		// has no token
		if (whiteList.indexOf(to.path) !== -1) {
			next()
		} else {
			next(`/login?redirect=${to.path}`)
			NProgress.done()
		}
	}
})

router.afterEach(() => {
	// finish progress bar
	NProgress.done()
})

permission.js
``` 

```
{
	path: '/',
	component: Layout,
	redirect: '/dashboard',
	children: [
		{
			path: 'dashboard',
			component: () => import('@/view/dashboard/index'),
			name: 'Dashboard',
			meta: { title: 'Dashboard', icon: 'dashboard', affix: true }
		}
	]
}
```

前端

- 登录：登录请求；登录鉴权；路由重定向
- 添加图书：图书上传，图书新增，用户引导
- 图书列表：列表组件，翻页组件，查询组件

服务端

- 用户API: 登录，jwt鉴权，用户信息
- 图书API：图书上传，图书解析，目录解析，图书增删改查
- 异常：全局异常，自定义异常

> 功能

- 用户名密码校验
- token生成，校验和路由过滤
- 前端token校验和重定向

> 电子书上传

- 文件上传
- 静态资源服务器

> 电子书解析

- epub原理
- zip解压
- xml解析

> 电子书的增删改

- mysql数据库应用
- 前后端异常处理

`npm,webpack,node.js`

`Vue+Element-UI+vue-element-admin`

`Node+Express+MySQL`

> 前端：

`Vue+Element-UI+vuex+vue-router`

`vue-element-admin+Vue-CLI 3.0+webpack`

> 后端

jwt登录认证 epub电子书解析 mysql

multer文件上传 xml2js（xml文件解析） adm-zip（实现自定义电子书解析过程）

express

node

> 过程

vue进阶->ElementUI入门->Vuex+Vue-router进阶->前端框架搭建->

服务端框架搭建->登录->电子书上传和解析功能开发->

电子书列表开发->项目发布

> Vue进阶

- 事件绑定 `$emit`和`$on`
- 指令`directive`
- 插件`Vue.use`
- 组件化`Vue.component`
- 组件通信`provide`和`inject`
- 过滤器`filter`
- 监听器`watch`
- `class`和`style`绑定的高级用法
- 2.6新特性`Vue.observable`和插槽`slot`

> Element-UI

- vue-cli 3 element插件
- element 按需加载
- 表单开发
- 列表开发
- el-form 源码解读

> vuex和vue-router进阶

- vuex实现原理解析
- vue-router 实现原理解析
- vue-router 路由守卫
- vue-router 路由元信息
- vue-router 路由API

> vue-element-admin框架解读

- 登录逻辑
- 网络请求
- 页面框架(Layout)
- 动态生成路由
- 图标使用
- 面包屑导航

> Node

Node框架-常用库-本地应用开发-网络应用开发-操作数据库

> Express

路由，中间件，异常处理

> 开始

- Nginx服务器
- MySQL数据库
- 安装Node和Vue

1. 登录
2. 文件上传
3. EPUB电子书解析
4. 新增/编辑电子书
5. 电子书列表
6. 删除电子书

> 技术

7. jwt认证
8. EPUB解析电子书
9. XML解析
10. zip解压
11. MULTER文件上传
12. MySQL数据库的操作

> 服务

- CentOS 服务器
- 域名服务
- https服务
- git仓库

> Element-UI饿了么

```
vue create element-ui-test

npm i element-ui -S

npm run serve

import ElementUI from 'element-ui'
Vue.use(ElementUI)

import 'element-ui/lib/theme-chalk/index.css'
```

```
import Vue from 'vue'
import App from './App.vue'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'

Vue.use(ElementUI)
Vue.config.productionTip = false

new Vue({
	render: h => h(App),
}).$mount('#app')
```

> 按需加载

对项目进行打包：

```
npm run build
```

没有按需加载会 vendors 库高达 789kb

安装：`babel-plugin-component`

```
npm install babel-plugin-component -D
```

安装完生成`babel.config.js`进行修改:

```
{
	"presets": [["es2015", { "modules": false }]],
	"plugins": [
		[
			"component",
			{
				"libraryName": "element-ui",
				"styleLibraryName": "theme-chalk"
			}
		]
	]
}
```
按需引入
```
import { Button, Message } from 'element-ui'

Vue.component(Button.name, Button)
Vue.prototype.$message = Message
```

style引用可以去掉，重新构建

> 插件引用

```
vue add element
```

```
vue-cli-plugin-element

Fully import
Import on demand
```

```
// 增加-main.js
import './plugins/element.js'

npm run serve
```

> 表单组件

- el-form容器，通过model绑定数据
- el-form-item容器，通过label绑定标签
- 表单组件通过v-model绑定model中的数据

> 表单校验

```
data() {
	const userValidator = (rule, value, callback) => {
		if (value.length > 3) {
			callback()
		} else {
			callback(new Error('用户名长度必须大于3'))
		}
	}
}
```

```
<el-form inline :model="data" :rules="rules" ref="form">

this.$refs.form.validate((isValid, errors) => {
	console.log(isValid, errors)
})
```

> 表单校验高级用法

- 动态改变校验规则

rules只包含一个校验规则

```
{
	rules: {
		user: [
			{ required: true, trigger: 'change', message: '用户名必须录入' },
		]
	}
}
```

动态添加rules

```
addRule() {
	const userValidator = ( rule, value, callback ) => {
		if (value.length > 3) {
			this.inputError = ''
			this.inputValidateStatus = ''
			callback()
		} else {
			callback(new Error('用户名长度必须大于3'))
		}
	}
	const newRule = [
		...this.rules.user,
		{ validator: userValidator, trigger: 'change' }
	]
	this.rules = Object.assign({}, this.rules, { user: newRule })
}
```

新对象赋值的方法：

```
this.rules = Object.assign({}, this.rules, { user: newRule })
```

> 手动控制检验状态

`validate-status`: 验证状态，枚举值，共四种：

- success: 验证成功
- error: 验证失败
- validating: 验证种
- 空：未验证
- error: 自定义错误提示

设置el-form-item属性

```
<el-form-item
 label="用户名"
 prop="user"
 :error="error"
 :validate-status="status"
>
</el-form-item>
```

> 表单常见属性解析

- `label-position`:标签位置，枚举值，left和top
- `label-width`:标签宽度
- `babel-suffix`:标签后缀
- `inine`:行内表单
- `disabled`设置整个form中的表单组件全部`disabled`，优先级低于表单组件自身的`disabled`

```
provide() {
	return {
		elForm: this
	}
}
```

```
// el-input源码
inputDisabled() {
	return this.disabled || (this.elForm || {}）.disabled;
}
```

size: 设置表单组件尺寸

```
// form-item.vue

mixins: [emitter],
provide() {
	return {
		elFormItem: this
	};
},
inject: ['elForm'],
props: {}
```

```
// input.vue
inject: {
	elForm: {
		default: ''
	},
	elFormItem: {
		default: ''
	}
}
```

> el-form源码解析

```
element-ui/index.js
```

```
// index.js
const components = [...];

const install = function(Vue, opts = {}) {
	locale.use(opts.locale);
	locale.i18n(opts.i18n);
	
	components.forEach(component => {
		Vue.component(component.name, component);
	});
	
	Vue.use(InfiniteScroll);
	Vue.use(Loading.directive);
	
	Vue.prototype.$ELEMENT = {
		size: opts.size || '',
		zIndex: opts.zIndex || 2000
	}
}
```

```
// form.vue

<template>
	<form class="el-form" :class="[
		labelPosition ? 'el-form--label-' + labelPosition : '',
		{ 'el-form--inline': inline}
	]">
	 <slot></slot>
	</form>
</template>

watch: {
	rules() {
		this.fields.forEach(field => {
			field.removeValidateEvents();
			field.addValidateEvents();
		});
		if (this.validateOnRuleChange) {
			this.validate(() => {});
		}
	}
}
```

```
// form-item.vue
<template>
 <div class="el-form-item" :class="[{
	 'el-form-item--feedback': elForm && elForm.statusIcon,
	 'is-error': validateState === 'error',
	 'is-validating': validateState === 'validating',
	 ''
 }]"
 
watch: {
	error: {
		immediate: true,
		handler(value) {
			this.validateMessage = value;
		}
	}
}
```

> vuex实现原理

> vue-router实现原理

> vue-router实例化时会初始化this.history，不同mode对应不同的history

```
vue-router还提供了两个组件：

Vue.component('RouterView', View)

Vue.component('RouterLink', Link)
```

vuex 相当于一个公共仓库，保存这所有组件都能共用的数据

## vue-router路由守卫

创建router.js:

```
import Vue from 'vue'
import Route from 'vue-router'
import HelloWorld from './components/HelloWorld'

Vue.use(Route)

const routes = [
	{ path: '/hello-world', component: HelloWorld }
]
const router = new Route({
	routes
})

export default router
```

## 全局守卫/局部守卫

```
router.beforeEach((to, from, next) => {
	console.log('beforeEach', to, from)
	next()
})

router.beforeResolve((to, from, next) => {
	console.log('beforeResolve', to, from)
	next()
})

router.afterEach((to, from) => {
	console.log('afterEach', to, from)
})
```

局部守卫

```
beforeRouteEnter(to, from, next) {
	// 不能获取组件实例 this
	console.log('beforeRouteEnter', to, from)
	next()
},

beforeRouteUpdate(to, from, next) {
	console.log('beforeRouteUpdate', to, from)
	next()
},

beforeRouteLeave(to, from, next) {
	console.log('beforeRouteLeave', to, from)
	next()
}
```

```
beforeEach
beforeRouteUpdate
beforeResolve
afterEach
```

```
beforeRouteLeave
beforeEach
beforeRosolve
afterEach
beforeDestroy
```

> 错误

```
// 改变title
beforeCreate() {
	console.log('beforeCreate')
	document.title = 'A' // 每个组件都加这个不太好
}
```

```
router.beforeEach((to, from, next) => {
	console.log('beforeEach', to, from)
	if (to.meta.title) {
		document.title = to.meta.title
	} else {
		...
	}
	next()
})
```

router.js

```
Vue.mixin({
	beforeCreate() {
		console.log('beforeCreate', this.$router, this.$route)
		...
	}
})
```

```
// 2
Vue.mixin({
	beforeCreate() {
		console.log('beforeCreate', this.$router, this.$route)
		// $route当前路由
		if (this.$route.meta.title) {
			document.title = this.$route.meta.title
		} else {
			document.title = 'xxx'
		}
		...
	}
})
```

> 路由API

使用router.addRoutes动态添加路由

```
addRoute() {
	this.$router.addRoutes([{
		path: '/b', component: B, meta: { title: 'Custom Title B'},
	}])
}
```

可以访问B：

```
<router-link to="/b"> to b </router-link>
```

> 前端框架搭建

vue-element-admin源码

```
git clone https://github.com/PanJiaChen/vue-element-admin

cd vue-element-admin

npm i

npm run dev
```

项目精简

- 删除src/views下的源码，保留
- dashboard：首页
- error-page: 异常页面
- login:登录
- redirect:重定向
- 对src/router/index进行相应修改
- 删除src/router/modules文件夹
- 删除src/vendor文件夹

> index.js

```
export const asyncRoutes = [
	{ path: '*', redirect: '/404', hidden: true}
]

const createRouter = () => new Router({
	scrollBehavior: () => ({ y: 0 }),
	routes: constantRoutes
})

const router = createRouter()
```

> vue-admin-template

vue-element-admin实现了登录模块，包括token校验，网络请求等

## 项目配置和源码调试方法

项目配置
通过 `src/settings.js` 进行全局配置：

title：站点标题，进入某个页面后，格式为：
页面标题 - 站点标题
- showSettings：是否显示右侧悬浮配置按钮
- tagsView：是否显示页面标签功能条
- fixedHeader：是否将头部布局固定
- sidebarLogo：菜单栏中是否显示LOGO
- errorLog：默认显示错误日志的环境

默认源码title

```
// get-page-title.js

import defaultSettings from '@/settings'
const title = defaultSettings.title || '达达前端'

export default function getPageTitle(pageTitle) {
	if (pageTitle) {
		return `${pageTitle} - ${title}`
	}
	return `${title}`
}
```

permission.js

```
import { getToken } from '@/utils/auth'
import getPageTitle from '@/utils/get-page-title'

NProgress.configure({ showSpinner: false })

const whiteList = ['/login', '/auth-redirect']

router.beforeEach(async(to, from, next) => {
	NProgress.start()
	
	document.title = getPageTitle(to.meta.title)
	
	const hasToken = getToken()
})
```

> 项目结构

```
api：接口请求
assets：静态资源
components：通用组件
directive：自定义指令
filters：自定义过滤器
icons：图标组件
layout：布局组件
router：路由配置
store：状态管理
styles：自定义样式
utils：通用工具方法
auth.js：token 存取
permission.js：权限检查
request.js：axios 请求封装
index.js：工具方法
views：页面
permission.js：登录认证和路由跳转
settings.js：全局配置
main.js：全局入口文件
App.vue：全局入口组件
```

> 源码调试

如果需要进行源码调试，需要修改 vue.config.js：
```
config
  // https://webpack.js.org/configuration/devtool/#development
  .when(process.env.NODE_ENV === 'development',
    config => config.devtool('cheap-source-map')
  )
```
将 cheap-source-map 改为 source-map，如果希望提升构建速度可以改为 eval

通常建议开发时保持 eval 配置，以增加构建速度，当出现需要源码调试排查问题时改为 source-map

## 有什么针对项目体积精简的方案？

代码压缩：使用 webpack 中的 compression-webpack-plugin/UglifyJS 对代码进行压缩，

移除不必要的模块：对项目中没有使用到的文件 / 模块 / 代码内容进行删除，
选择可替代的体积较小的模块：如项目中引入了 moment.js 但是只使用了 Datetime 的格式化及各种运算，可以考虑使用体积更小的 day.js 来替代 moment.js。

或者使用支持 tree-shaking 的模块（通过 export function 定义的函数即可支持 tree-shaking）

按需引入模块：当你面对一个巨无霸的，捆绑式的大型模块时，可能你并不会使用到它的所有的功能，你只需要按照你的需求引入模块就可以了。

## 前端代码调试的方法有哪些？

分析报错信息：根据控制台的报错信息定位到代码进行代码分析

onsole 大法：打印出报错前后的内容

对比大法：如果有相同页面，可以和其他页面进行对比看看是否缺少插件 / 模块

注释代码大法：如果不知道报错位置，可以分批注释代码，定位到报错位置的代码进行分析

```
// AppMain.vue
<router-view :key="key" />

<keep-alive :include="cachedViews">
	<router-view :key="key" />
</keep-alive>

cachedView() {
	return this.$store.state.tagsView.cachedViews
}
```

> store

> utils

单页应用，也是单组件
