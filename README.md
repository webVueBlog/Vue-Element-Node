# Vue-Element-Node.js

`Vue-Element+Node.js`开发企业通用管理后台系统

前端

- 登录：登录请求；登录鉴权；路由重定向
- 添加图书：图书上传，图书新增，用户引导
- 图书列表：列表组件，翻页组件，查询组件

服务端

- 用户API: 登录，jwt鉴权，用户信息
- 图书API：图书上传，图书解析，目录解析，图书增删改查
- 异常：全局异常，自定义异常

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






















