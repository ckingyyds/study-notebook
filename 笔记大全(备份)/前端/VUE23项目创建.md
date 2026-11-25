我为你整理了一份**“前端环境备忘录”**，以后每次创建项目，直接照抄这份作业就行：

------

### 🚀 一、Vue 3 项目（现代主流路线：Vite + pnpm）

这是目前企业里最常用的开发方式，速度极快，适合你自己做练习和未来找工作。

**1. 环境准备**

bash

```
nvm use 24.19.0      # 切换到高版本 Node
pnpm -v               # 确保 pnpm 可用
```



**2. 创建项目**

bash

```
cd D:\VueWork          # 进入你的工作目录
pnpm create vue@latest # 运行创建命令
# 选项建议：TypeScript(No), JSX(No), Router(Yes), Pinia(Yes), 其他全选No
```



**3. 启动项目**

bash

```
cd 你的项目名
pnpm install
pnpm dev               # 注意：是 dev 不是 serve
```



- **访问地址**：`http://localhost:5173/`

**4. Vue 3 核心代码（组合式 API）**

vue

```
<template>
  <div>
    <h1>{{ msg }}</h1>
    <button @click="changeMsg">点击修改</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const msg = ref('这是 Vue 3 的响应式数据')
const changeMsg = () => {
  msg.value = 'Vue 3 修改成功！'
}
</script>
```



------

### 🏫 二、Vue 2 项目（传统/作业路线：Vue CLI + npm）

这是很多学校教材还在用的老框架，底层是 Webpack。**容易踩坑，必须用特定的低版本和包管理器。**

**1. 环境准备**

bash

```
nvm use 16.20.2      # 必须切回老版本 Node (Vue CLI 不兼容高版本)
```



**2. 创建项目**

bash

```
cd D:\Vue2Work         # 进入工作目录
vue create vuedemo1 -m npm 
# 🚨 重点：必须加上 -m npm，强制使用 npm，否则 pnpm 会报 ERR_PNPM_IGNORED_BUILDS 的错！
# 选项：选 Default ([Vue 2] babel, eslint)
```



**3. 启动项目**

bash

```
cd vuedemo1
npm run serve          # 注意：是 serve 不是 dev
```



- **访问地址**：`http://localhost:8080/`

**4. Vue 2 核心代码（选项式 API）**

vue

```
<template>
  <div id="app">
    <h1>{{ msg }}</h1>
    <button @click="changeMsg">点击修改</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      msg: '这是 Vue 2 的响应式数据'
    }
  },
  methods: {
    changeMsg() {
      this.msg = 'Vue 2 修改成功！'
    }
  }
}
</script>
```



------

### 📊 三、一张表看懂核心区别（防混淆）

| 对比维度           | Vue 3 (Vite)              | Vue 2 (Vue CLI)                      |
| :----------------- | :------------------------ | :----------------------------------- |
| **推荐 Node 版本** | v20 / v24 及以上          | **v16** 及以下                       |
| **包管理器**       | **pnpm** (完美兼容)       | **npm** (不要用 pnpm)                |
| **创建命令**       | `pnpm create vue@latest`  | `vue create 项目名 -m npm`           |
| **启动命令**       | `pnpm dev`                | `npm run serve`                      |
| **默认端口**       | **5173**                  | **8080**                             |
| **代码写法**       | `<script setup>` (组合式) | `export default { data() }` (选项式) |

### 项目准备
**1.下载依赖**
下载别人的项目要先
```bash
pnpm install
```

如果安装时出现了Ignored build scripts: core-js@3.51.0,yorkie@2.0.0
```bash
pnpm approve-builds
```

下载依赖再启动,目前还下了这三个依赖
```bash
pnpm add axios vue-router@3 echarts
```

 **2. 安装开发依赖（只在开发时用的工具）**

因为你的代码里写了 `<style scoped lang="less">`，所以必须把 `less` 相关的编译器装上，不然会报之前那个红屏错误：
```bash
pnpm add -D less less-loader@7
```

_(加上 `-D` 代表装到 `devDependencies` 里，表示这只是开发辅助工具，打包上线的时候不需要它。同样，`less-loader` 必须锁定 `@7` 版本才能和 Vue2 兼容。)_


### 基础报错问题

**jsconfig.json文件报错,加一句**

```
"ignoreDeprecations": "6.0",  // 👈 加上这一行
```

 **文件名有空格报错,在package.json文件的rule里面加上**


```
"vue/multi-word-component-names": "off"
```

**文件名有空格浏览器报错,找到 `vue.config.js` 这个文件（如果找不到，就在根目录自己建一个）。**

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  // 👇 加上这一句，关闭保存时的 ESLint 检查
  lintOnSave: false 
})
```


### 配置框架快捷方式

第一步：打开配置文件
在 VS Code 里按 Ctrl + Shift + P。

输入 snippets，选择 配置用户代码片段（Preferences: Configure User Snippets）。

在弹出的列表里，直接输入 vue，选中 vue.json。

第二步：覆盖代码
把 vue.json 里面所有的旧代码全部删掉（全选 -> 删除），然后直接把下面这段代码原封不动地复制粘贴进去：

```json
{
	"Vue2 Template": {
		"prefix": "vue2",
		"body": [
			"<template>",
			"  <div>$1</div>",
			"</template>",
			"",
			"<script>",
			"export default {",
			"  data() {",
			"    return {",
			"      $2",
			"    }",
			"  },",
			"  methods: {",
			"    $3",
			"  }",
			"}",
			"</script>",
			"",
			"<style scoped lang=\"less\">",
			"$4",
			"</style>"
		],
		"description": "生成 Vue2 的完整模板"
	},
	"Vue3 Template": {
		"prefix": "vue3",
		"body": [
			"<template>",
			"  <div>$1</div>",
			"</template>",
			"",
			"<script setup>",
			"import { ref } from 'vue'",
			"",
			"$2",
			"</script>",
			"",
			"<style scoped lang=\"less\">",
			"$3",
			"</style>"
		],
		"description": "生成 Vue3 的完整模板"
	}
}
```

