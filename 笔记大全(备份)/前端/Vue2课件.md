# Vue2 课件

1. **模板只能有一个根节点**。`<template>` 最外层必须一个 `<div>` 包住全部内容，多根直接编译报错。

---

## 第一阶段｜模板渲染与基础指令

> 阶段目标：让学生亲眼看到"数据变了，页面自己变"。这三节不碰样式、不碰事件复杂度，只建立响应式的直觉。

### 1. 插值语法

**本节目标**

- 能用 `{{ }}` 把 `data` 里的数据显示到页面
- 会做字符串拼接、算术运算
- 知道插值里能写什么、不能写什么

**讲解**

Vue 页面上的所有动态内容，都来自组件里的 `data`。`data` 是一个函数，返回一个对象——**这个返回的对象里放的，就是页面的"数据源"**。为什么要用函数返回对象而不是直接写对象？因为组件可能被复用多次，每次都要一份独立的数据，不能共享同一个引用。这一点先记住，后面讲组件会反复遇到。

`{{ }}` 是 Vue2 的插值语法，官方叫 Mustache（大胡子），也叫"大括号语法"。Vue3叫组合式api语法,我们后面再学

```vue
<template>
  <div>
    <h2>{{ msg }}</h2><!-- 大括号语法 / 大胡子语法 / 插值语法 -->
    <h2>{{ firstName }}{{ lastName }}</h2>
    <h2>{{ firstName + lastName }}</h2>
    <h2>{{ firstName + '-' + lastName }}</h2>
    <h2>{{ firstName }}-{{ lastName }}</h2>
    <h2>{{ count * 2 }}</h2>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 这里就是我们的数据源泉
      msg: '我是美国人',
      firstName: 'zhang',
      lastName: 'san',
      count: 100
    }
  }
}
</script>
```

**运行效果**：页面上依次显示 `我是美国人`、`zhangsan`、`zhangsan`、`zhang-san`、`zhang-san`、`200`。

**本节小结**：`{{ }}` 负责把数据显示到标签中间；里面可以写表达式和运算，但不能写语句（`if`、`for`）和异步。

---

### 2. 几个常见的指令以及点击事件

**本节目标**

- 分清 `v-text` 和 `v-html`
- 会用 `v-for` 渲染列表并正确设置 `:key`
- 会绑定点事件，理解"加不加括号"的区别

**讲解**

上一节的 `{{ }}` 只能放在标签**中间**。如果想让内容成为标签的**属性**或者**整体替换元素内容**，就要用指令。指令是 `v-` 开头的特殊属性，Vue 认它，浏览器不认。

`v-text` 输出纯文本，会把元素里原有的内容**整个覆盖**——所以下面第一行里的 `,你好` 永远不会显示，而是变成变量msg的内容 , 这是学生最容易困惑的地方。

`v-html` 会解析标签。要讲清风险：如果这个字符串来自用户输入，就等于把 XSS 漏洞直接开在页面上。**只用于自己可控的富文本**。

`v-for`**（列表渲染）**你把一个数组丢给它，它就会帮你把这个数组里的每一项，自动变成页面上的 HTML 标签。

```vue
<template>
  <div id="app">
    <h2 v-text="msg">,你好</h2>
    <h2 v-html="url"></h2>//v-html解析标签,所以这里显示解析过后的a标签
//第一种写法:只拿值
//item in list：意思是:遍历 list 数组，把每一次循环拿到的对象，命名为 item
//在标签里，你就可以用 {{ item.name }} 把名字显示出来,name是数组设置的。
    <ul>
      <li v-for="item in list" :key="item.id">{{ item.name }}</li>
    </ul>
//第二种写法:拿值 + 拿下标(不推荐用索引绑定key)
    <ul>
      <li v-for="(item, index) in list" :key="index">{{ item.name }}</li>
    </ul>

    <h3>{{ count }}</h3>
    <button @click="addCount">添加</button>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      count: 0,
      msg: 'Hello World',
      url: '<a href="https://www.baidu.com">百度</a>',
      list: [
        { id: 1, name: '张三' },
        { id: 2, name: '李四' },
        { id: 3, name: '王五' },
        { id: 4, name: '赵六' }
      ]
    }
  },
  methods: {
    addCount() {
      // 这里的 this 指向 Vue 实例
      console.log(this)
      this.count++
    }
  }
}
</script>
```

**运行效果**：第一行显示 `Hello World`（`,你好` 被覆盖）；第二行是能点的"百度一下"链接(v-html解析)；两个列表各显示四个名字；点"添加"，数字往上走。

#### v-for讲解

**`1. :key` 必须讲透**

`v-for` 要配 `:key`，值必须唯一。想象 Vue 是一个“流水线包工头”，它根据你的 `list` 数组来画 `<li>`。如果你突然往数组中间加了一项，或者删了一项，流水线工人得知道“到底是谁变了”。
`:key` 就是给每个 `<li>` 贴的一张**“身份证”**。当数据发生变化时，Vue 会对比新旧身份证，只更新变化的那一项，其他的直接复用，不用重新画，极大地提升了性能。

**2 .为什么你代码里一个写 `item.id`，另一个写 `index`？**

- `:key="item.id"` （**推荐**）：这是绝对唯一且稳定的，就像人的身份证号。哪怕数组重新排序、增删，张三的 `id` 永远是 `1`，Vue 精准定位。
- `:key="index"` （**极不推荐，职场大忌**）：索引是动态变化的。如果你把数组里的第一项删了，原本排第二的就会变成索引 0。这会导致 Vue 认错人，不仅性能变差，还可能导致输入框里的文字错位、复选框乱勾。**记住一条铁律：只要数据里有唯一标识（比如 id），绝不用 index 当 key。**

**3.`key`为什么加冒号？**

在 Vue 中，**冒号 `:` 是 `v-bind` 的缩写**。它的作用是告诉 Vue：“**后面引号里的内容，不要当成普通字符串，请把它当成一个 JavaScript 表达式/变量来解析。**”

**看个对比（秒懂）：**

- 没有冒号：`key="item.id"`
  - Vue 会把引号里的 `item.id` 当成**纯文本字符串**。页面上显示出来的就是死板的 `"item.id"`。
- 有冒号：`:key="item.id"`
  - Vue 会执行引号里的代码，拿到 `item` 对象里 `id` 的**真实数值**（比如 `1`、`2`、`3`）。

#### method方法讲解

**1.@click="addCount"是什么意思**

- [x] **语法拆解**

- **`@`**：这是 Vue 里的**语法糖**（缩写）。它的全称是 `v-on`。`@` 后面紧跟事件类型，比如 `@click`（点击）、`@mouseover`（鼠标悬停）、`@keydown`（键盘按下）。
- **`=`**：赋值/绑定。
- **`"addCount"`**：你要执行的**函数名**（定义在 Vue 的 `methods` 里）。

- [x] **对比你之前学的原生 JS**

你回想一下原生 JS，如果要绑定点击事件，你得这么写：

```js
button.addEventListener('click', addCount)
```

**2.加不加括号**

- 没参数：`@click="addCount"` 和 `@click="addCount()"` 完全等价
- 有参数：必须写括号 `@click="addCount(1)"`
- 既想传参又想拿 event：`@click="addCount($event, 1)"`（10 里细讲）

**课堂练习**

1. 把 `list` 改成从 1 到 10 的数字数组，页面上渲染成 `第 1 项`、`第 2 项`……

2. 给每行加一个"删除"按钮，点击后从列表中移除该项（提示：`splice`）。

   ```vue
   <template>
     <div><!--list没有id,直接用num或者index就行-->
       <p v-for="(num,index) in list" :key="num">第{{num}}项<button @click="remove(index)">删除</button></p>
     </div>
   </template>
   
   <script>
   export default {
     data() {
       return {
       list: [1,2,3,4,5,6,7,8,9,10]
       }
     },
     methods: {
       remove(index){
           //this指向当前的这个 Vue 组件实例,删数组的项,也就是直接删除了页面内容了,splice删除内容常用
           this.list.splice(index,1)
       }
     }
   }
   </script>
   
   <style scoped lang="less">
   
   </style>
   ```

   

**本节小结**：`v-text` 盖掉内容，`v-html` 解析标签（危险），`v-for` 必须配唯一 `:key`，`@click` 是 `v-on:click` 的简写。

---

### 3. 衍生案例：计数器

**本节目标**

- 走完"数据 → 事件 → 改数据 → 页面更新"的完整闭环
- 对比行内写法和 `methods` 写法

**讲解**

这一节是把前两节缝起来的第一针。页面上要看到的东西：一个数字、加减两个按钮。点一下，数字变一次，页面自己刷新——**全程没有一行 DOM 操作**，这就是响应式。

```vue
<template>
  <div>
    <h2>{{ count }}</h2>

    <button @click="count++">+</button>
    <button @click="count--">-</button>

    <button @click="add">用方法 +</button>
    <button @click="sub">用方法 -</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      count: 0
    }
  },
  methods: {
    add() {
      this.count++
    },
    sub() {
      this.count--
    }
  }
}
</script>
```

**为什么放两套写法**：行内 `count++` 只适合一行能搞定的场景；一旦要加判断（比如不能减到负数），就必须进 `methods`。这是"什么时候该抽方法"的第一个判断标准。

**易错点**

- `methods` 里写箭头函数 → `this` 不是组件实例，`this.count++` 无效且报 `Cannot read properties of undefined`
- 把 `count` 写在 `data` 外面 → 它不是响应式的，改了页面不动

**课堂练习**

1. 让数字不能小于 0，减到 0 时按钮变灰（提示：`:disabled="count === 0"`，为下一阶段埋伏笔）。
2. 加一个"重置"按钮，点一下归零。

**本节小结**：改数据 → 页面自动更新；箭头函数在 `methods` 里是雷区。

---

## 第二阶段｜属性绑定与样式控制

### 4. v-bind 指令

**本节目标**

- 理解 `v-bind` 是给**属性**绑定变量
- 记住简写是冒号 `:`(之前的:key)
- 分清"变量"和"字符串字面量"

**讲解**

`{{ }}` 管标签中间的内容，**属性上的值它管不到**。`<img src="123.jpg">` 里的 `src` 是写死的字符串；要想让它来自变量，就必须加 `v-bind`，简写一个 `:`。

```vue
<template>
  <div>
    <img v-bind:src="imgURL" alt="" title="图片1" :width="width1">
    <img :src="imgURL" alt="" title="图片2" :width="width2">
    <button @click="bs">放大/缩小</button><!--后面的练习-->
    
     <a v-bind:href="aHerf">百度一下（v-bind 全写）</a>
     <a :href="aHerf">百度一下（简写）</a>
  </div>
</template>

<script>
// v-bind 绑定事件  语法糖:
export default {
  data() {
    return {
       imgURL:"https://cn.bing.com/th?id=OIP.NaSKiHPRcquisK2EehUI3gHaE8&pid=Api&rs=1",
       aHerf:"http://www.baidu.com",
       width1:200,
       width2:400
    }
  },
  methods: {
    bs(){
        if (this.width1===200&&this.width2===400) {
            this.width1=400
            this.width2=200
        }else{
            this.width1=200
            this.width2=400
        }
    }
  }
}
</script>
<style scoped lang="less">
</style>
```

**运行效果**：两张同样的图片、两个能跳转的链接。

**必须强调的一点**：`:` 后面**不加引号是变量，加单引号是字符串**。

| 写法 | 含义 |
|---|---|
| `:src="imgURL"` | 取 `data` 里 `imgURL` 的值 |
| `:src="'imgURL'"` | 输出字符串 `imgURL`，图片必然加载失败 |

**易错点**：图片是外链，第一次加载要联网。课堂网络不稳时，提前把图片放 `src/assets/` 下，用 `:src="require('@/assets/demo.jpg')"`。

**课堂练习**：给图片加上 `:title`、`:width`，宽度用一个变量控制，再放一个按钮点一下把宽度在 200 和 400 之间切换。

**本节小结**：属性绑变量 → 加 `:`；`:` 后面是 JS 表达式，不是字符串。

---

### 5. v-bind 案例：三元表达式控制类名

**本节目标**

- 完成"点击切换高亮"这个最常见的交互
- 掌握三元表达式写 `:class`

**讲解**

先明确需求：一个电影列表，默认第一项红色，点哪一项哪一项变红变大。

> **要点**：这里的 `:class` 绑定的是一个**表达式的结果**，结果要么是类名字符串 `'active'`，要么是空字符串 `''`。空字符串等于没有类名。

```vue
<template>
  <div>
<!--:class是绑定属性,用三元运算符,如果满足如果满足设置class = active--,也可以写成这样:   :class="{active:currentIndex==index}">
<!--change(index),点击后向change()里面传入index,将currentIndex赋值为index,也就是点击后设置成class = active-->
    <div
      v-for="(item, index) in movies"
      :key="index"
      :class="index == currentIndex ? 'active' : ''"
      @click="change(index)"
    >
      {{ item }}--{{ index }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentIndex: 0,
      movies: ['海王', '海贼王', '火影忍者', '复仇者联盟','蜘蛛侠']
    }
  },
  methods: {
    // 注意：不要写成箭头函数，箭头函数里的 this 不是 Vue 实例
    change(i) {
      console.log(i)
      this.currentIndex = i
    }
  }
}
</script>

<style scoped>
.active {
  color: red;
  font-size: 20px;
}
</style>
```

**运行效果**：四行文字，第一行红色 20px；点第二行，红色跳到第二行。

**本节最大的坑**：不能写箭头函数`change: (i) => { this.currentIndex = i }`。箭头函数没有自己的 `this`，会往外层找，找到 `undefined` 或 `window`，赋值直接失效。这是**初学者 80% 的"点了没反应"都源于此**。

**课堂练习**：把高亮样式改成"加背景色 + 左侧竖线"，要求学生只用 CSS 完成，不动 JS。

**本节小结**：三元能应付"二选一"，多类名就该换对象写法了。

---

### 6. v-bind 案例：对象语法控制类名

**本节目标**

- 掌握 `:class="{ 类名: 布尔值 }"` 语法
- 理解"为 true 就加这个类"

**讲解**

三元表达式只能二选一。如果要同时判断多个类名（比如"选中态 + 禁用态"），三元会写成嵌套地狱。对象语法解决的就是这个问题：`{ active: 条件 }`，条件为真就加 `active`，为假就不加。

**对象语法**

```js
//写法 （对象语法）：
:class="{ active: currentIndex == index }"
//逻辑：{ 类名: 布尔值 }。意思是：如果 currentIndex == index 为 true，就加上 active 类名；如果为 false，就移除 active 类名。
```

```vue
<template>
  <div>
    <div
      v-for="(item, index) in movies"
      :key="index"
      :class="{ active: currentIndex == index }"
      @click="change(index)"
    >
      {{ item }}--{{ index }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentIndex: 0,
      movies: ['海王', '海贼王', '火影忍者', '复仇者联盟']
    }
  },
  methods: {
    change(i) {
      console.log(i)
      this.currentIndex = i
    }
  }
}
</script>

<style scoped>
.active {
  color: red;
  font-size: 20px;
}
</style>
```

**课堂对比**：让学生把 5 的三元写法和这一节的对象写法并排放，指出**结果完全一致**，但对象写法可以继续往里加条件：`{ active: cond1, disabled: cond2 }`。

**易错点**：对象里的 `active` **不要加引号写成 `"active"`**——虽然加了也能跑，但一旦类名带短横线（`is-active`）就必须加引号 `{ 'is-active': cond }`。提前讲清楚，避免学生以后自己踩。

**课堂练习**：再加一个类 `big`，让**偶数项**字号更大（提示：`{ big: index % 2 === 0 }`）。

**本节小结**：`{ 类名: 条件 }` 是多类名判断的标准写法。

---

### 7. v-bind 对象写法封装到函数里

**本节目标**

- 把判断逻辑从模板抽到 `methods`
- 理解"模板只负责渲染，逻辑放脚本"

**讲解**

**它和之前的写法有什么区别？**

- **之前的写法（内联）**：`:class="{ active: currentIndex === index }"`
  逻辑全写在 HTML 模板里，如果逻辑一长，模板就会变成一锅粥。
- **现在的写法（函数）**：`:class="changeMovie(index)"`
  HTML 看起来极其干净，只有一个函数名。具体的判断逻辑被包在了 JS 里面，以后要改规则（比如要加个变红、变粗的条件），只需要改 JS 函数即可。

```vue
<template>
  <div>
    <div
      v-for="(item, index) in movies"
      :key="index"
      :class="changeMovie(index)"
      @click="change(index)"
    >
      {{ item }}--{{ index }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentIndex: 0,
      movies: ['海王', '海贼王', '火影忍者', '复仇者联盟']
    }
  },
  methods: {
    change(i) {
      console.log(i)
      this.currentIndex = i
    },
    // 返回一个类名对象，模板里直接 :class="changeMovie(index)"
    changeMovie(index) {
      return { active: this.currentIndex === index,
              big:index%2===0 }//同时绑定两个条件类 
    }
  }
}
</script>

<style scoped>
.active {
  color: red;
  font-size: 20px;
}
.big{
  color: aqua;
  font-size: 18px;
}
</style>
```

**要讲清的代价**：函数**每次渲染都会执行**，没有缓存。列表 20 条、判断逻辑复杂时，这就是不必要的开销。这时候该用**计算属性**返回整个数组——但计算属性不带参数，需要先把"当前索引"变成依赖项（看完 12 再回头看这段，会有新理解）,实际开发中建议使用computed。

**课堂练习**：把 `changeMovie` 改成返回 `{ active: ..., big: index % 2 === 0 }`，两个类同时生效。

**本节小结**：模板写调用，逻辑写函数；函数无缓存，复杂场景换计算属性。

---

### 8. v-bind 数组的写法

**本节目标**

- 掌握 `:class="[a, b]"` 数组语法
- 分清"加引号是字符串、不加引号是变量"

**讲解**

数组语法用于**同时绑多个独立的类名**，数组里可以放字符串、变量、对象，甚至三元表达式。这是三种写法里最灵活的，也是最容易写错的——错就错在引号。

```vue
<template>
  <div>
    <!-- 加单引号：当成字符串，类名就是 active、line -->
    <h2 class="title" :class="['active', 'line']">{{ message }}</h2>
    <!-- 不加引号：当成变量，取到 data 里的 'aaa'、'bbb' -->
    <h2 class="title" :class="[active, line]">{{ message }}</h2>
    <!-- 用方法返回数组 -->
    <h2 class="title" :class="getActive()">{{ message }}</h2>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello World',
      active: 'aaa',
      line: 'bbb'
    }
  },
  methods: {
    getActive() {
      return [this.active, this.line]
    }
  }
}
</script>

<style scoped>
.title {
  font-weight: normal;
}
.active {
  color: red;
}
.line {
  text-decoration: underline;
}
.aaa {
  color: green;
}
.bbb {
  font-style: italic;
}
</style>
```

**运行效果**（这一节必须让学生自己看差异）：

- 第一行 → 类名是 `active` `line` → **红字 + 下划线**
- 第二行 → 类名是 `aaa` `bbb` → **绿字 + 斜体**
- 第三行 → 和第一行一样

**易错点**

- `:class="['title']"` 和 `class="title"` 都能加类，但前者是动态绑定，后者是静态，两者可以共存（如上例）
- 数组里混对象是合法写法：`:class="['title', { active: isActive }]"`

**课堂练习**：用数组语法实现"基础类 `item` + 选中类 `active` + 偶数类 `even`"三合一。

**本节小结**：数组管"多个类"，对象管"有条件才加的类"，实际项目里两者经常混着写。

---

## 第三阶段｜条件渲染与事件处理

> 阶段目标：让页面能"藏东西"，让事件能"精细控制"。这一阶段结束，学生应该能独立做出带弹窗、带表单提交、带键盘快捷键的页面。

### 9. v-if 和 v-show 等

**本节目标**

- 会用 `v-if` / `v-else-if` / `v-else` 条件判断做条件渲染
- 说清 `v-if` 和 `v-show` 的本质区别，并能正确选型

**讲解**

这是全课最重要的一个"选型题"。先给结论：

| | `v-if` | `v-show` |
|---|---|---|
| 为 false 时 | DOM 节点**被移除** | 节点还在，加 `display: none`隐藏了 |
| 切换开销 | 大（销毁 + 重建） | 小（只改样式） |
| 初始开销 | 小（条件为假就不渲染） | 大（无论如何先渲染） |
| 适用 | 条件少变、权限控制 | 频繁切换（tab、弹窗） |

```vue
<template>
  <div>
    <div v-if="age < 18">小于 18 岁，未成年</div>
    <div v-else-if="age < 30">大于等于 18 岁小于 30 岁，正值青年</div>
    <div v-else-if="age < 60">大于等于 30 岁小于 60 岁，正值中年</div>
    <div v-else>大于 60 岁，暮年</div>

    <hr />

    <button @click="isFlag = !isFlag">切换 isFlag：{{ isFlag }}</button>
    <h2 v-if="isFlag">v-if：为 true 时我存在，为 false 时我整个 DOM 都没了</h2>
    <h2 v-show="isFlag">v-show：为 true 时我显示，为 false 时我只是 display:none</h2>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isFlag: false,
      age: 68
    }
  }
}
</script>
```

**课堂演示**：点按钮切换，打开 F12 元素面板。`v-if` 那一行整个消失；`v-show` 那一行还在，只是多了 `style="display: none;"`。**这个动作一定要现场做一遍**，比讲十遍都管用。

**必须提醒的两个坑**

1. `v-if` 和 `v-for` 不要写在同一个元素上。Vue2 里 `v-for` 优先级更高，会先循环再判断，容易出意外也影响性能。
2. `v-if` 会触发组件的销毁与重建。如果这个元素里有表单输入，切换回来数据就清空了——这时候该用 `v-show`。

**课堂练习**：做一个"登录 / 未登录"切换：未登录显示"请登录"按钮，登录后显示"欢迎，张三"和"退出"按钮。

```vue
<template>
  <div>
    <h3>状态:{{msg}}</h3>
    <button @click="change(isFlag)">点击登录</button>
    <p v-show="isFlag">欢迎，张三</p>
    <button @click="quit" v-show="isFlag">退出</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      msg:"未登录",
      isFlag:false
    }
  },
  methods: {
    change(i){
      if (i==false) {
        this.msg='已登录',
        this.isFlag=true
      }
    },
      quit(){
        this.msg='未登录',
        this.isFlag=false
      }
    }
  }

</script>
```

**本节小结**：删 DOM 用 `v-if`，藏 DOM 用 `v-show`，频繁切换选后者。

---

### 10. v-on 传参和不传参

**本节目标**

- 搞清"加括号 / 不加括号"时参数到底是什么
- 掌握 `$event` 的用法

**讲解**

这一节全是实验，八个按钮，按顺序点一遍，控制台会给出八种结果。学生把这张表记下来，后面写事件就不会再犹豫。

```vue
<template>
  <div>
    <!-- 不传参数：加不加括号都一样 -->
    <button @click="btnClick">按钮1</button>
    <button @click="btnClick()">按钮2</button>

    <!-- 传参数 -->
    <button @click="btnClick2(123)">按钮3（拿到 123）</button>
    <button @click="btnClick2()">按钮4（拿到 undefined）</button>
    <button @click="btnClick2">按钮5（拿到 event）</button>

    <!-- 既要 event，又要其他参数 -->
    <button @click="btnClick3($event, 123)">按钮6（event + 123）</button>

    <!-- 只要其他参数，不要 event -->
    <button @click="btnClick4('aaa', 123)">按钮7（aaa + 123）</button>

    <!-- 参数再多就用剩余参数接 -->
    <button @click="btnClick5('aaa', 123, 234, 444, 555, 666)">按钮8（全部收集）</button>
  </div>
</template>

<script>
export default {
  methods: {
    btnClick() {
      console.log('点击XXX')
    },
    btnClick2(id) {
      console.log('点击XXX', id)
    },
    btnClick3(event, value) {
      console.log('点击XXX', event, value)
    },
    btnClick4(val1, val2) {
      console.log('点击XXX', val1, val2)
    },
    btnClick5(...args) {
      console.log('点击XXX', args)
    }
  }
}
</script>
```

**预期输出**（板书用）

| 按钮 | 控制台 |
|---|---|
| 1、2 | `点击XXX` |
| 3 | `点击XXX 123` |
| 4 | `点击XXX undefined` |
| 5 | `点击XXX PointerEvent{...}` |
| 6 | `点击XXX PointerEvent{...} 123` |
| 7 | `点击XXX aaa 123` |
| 8 | `点击XXX ['aaa', 123, 234, 444, 555, 666]` |

**记忆口诀**：**括号不写，event 白给；括号一写，`$event` 自己递。**

**课堂练习**：写一个函数 `handle(type, e)`，既要知道点的是哪个按钮，又要拿到点击坐标（提示：`e.clientX`）。

**本节小结**：不写括号自动收 event，写括号就得手动传 `$event`。

---

### 11. 冒泡、阻止默认行为、其他事件

**本节目标**

- 会用 `.stop` 阻止冒泡、`.prevent` 阻止默认行为
- 掌握 `@keydown`、`@keyup`、`@change`、`@input` 的触发时机

**讲解**

原始文件里这些代码全被注释掉了（只剩一句 `Hello World`），这一节把它们全部放开，做成一个能点、能输入、能提交的完整页面。

```vue
<template>
  <div>
    <h2>{{ msg }}</h2>

    <!-- 1. 阻止冒泡：点按钮不触发外层 div 的点击 -->
    <div class="box" @click="divClick">
      外层 div（点这里会打印 divClick）
      <button @click.stop="btnClick">阻止冒泡的按钮</button>
    </div>

    <!-- 2. 阻止默认行为：表单不再刷新页面 -->
    <form @submit.prevent="onSubmit">
      账号 <input type="text" v-model="form.user" />
      密码 <input type="text" v-model="form.password" />
      <input type="submit" value="登录" />
    </form>

    <!-- 3. 键盘与输入事件 -->
    <input type="text" placeholder="按下回车触发" @keydown.enter="getDown" />
    <input type="text" placeholder="每次输入都触发" @input="getInput" />
    <input type="text" placeholder="失焦或回车触发" @change="getChange" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      msg: 'Hello World',
      form: { user: '', password: '' }
    }
  },
  methods: {
    divClick() {
      console.log('divClick 被触发了')
    },
    btnClick() {
      console.log('btnClick：加了 .stop，没有冒泡到外层 div')
    },
    onSubmit() {
      console.log('提交被 .prevent 拦住，页面没有刷新', this.form)
    },
    getDown(e) {
      console.log('keydown.enter', e.target.value)
    },
    getInput(e) {
      console.log('input', e.target.value)
    },
    getChange(e) {
      console.log('change', e.target.value)
    }
  }
}
</script>

<style scoped>
.box {
  padding: 20px;
  margin-bottom: 12px;
  background: #f5f5f5;
}
</style>
```

**课堂实验顺序**

1. 点灰色区域任意空白处 → 只打印 `divClick`
2. 点按钮 → **只**打印 `btnClick`（没有 `divClick`，说明 `.stop` 生效）
3. 把 `.stop` 删掉再点 → 两个都打印，这就是冒泡
4. 点"登录"→ 打印表单数据，**页面没有刷新**（`.prevent` 生效）
5. 三个输入框分别试：`@keydown` 每按一个键就触发、`@input` 每次内容变化触发、`@change` 失焦或回车才触发

**修饰符速查**

| 修饰符 | 作用 |
|---|---|
| `.stop` | 阻止冒泡（等于 `event.stopPropagation()`） |
| `.prevent` | 阻止默认行为（等于 `event.preventDefault()`） |

**易错点**：`.prevent=""` 这种写法（原始注释里就是）在 Vue2 里能跑，但没必要，写成 `@submit.prevent="onSubmit"` 更规范。

**课堂练习**：做一个搜索框，按回车才发起"搜索"（用 `console.log` 模拟），并说明为什么不用 `@input`。

**本节小结**：冒泡靠 `.stop`，默认行为靠 `.prevent`，键盘靠按键修饰符。

---

## 第四阶段｜计算属性与双向绑定

> 阶段目标：从"手动拼字符串"升级到"数据自动派生"，再让派生值能反写回去。这一阶段是学生从"会用模板"到"会抽象数据"的分水岭。

### 12. 计算属性的简单使用

**本节目标**

- 会写 `computed`
- 分清 `computed` 和 `methods` 的使用差异（带不带括号）

**讲解**

`{{ firstName }}-{{ lastName }}` 这种拼接，模板里写一两次还行，写十次就重复了。计算属性就是给"算出来的值"起个名字，当成 `data` 一样用。

```vue
<template>
  <div>
    <div>{{ firstName }}-{{ lastName }}</div>
    <!-- 计算属性：不加括号 -->
    <div>计算属性得到的：{{ fullName }}</div>
    <!-- 方法：必须加括号 -->
    <div>方法得到的：{{ getFullName() }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      firstName: 'zhang',
      lastName: 'san'
    }
  },
  methods: {
    getFullName() {
      return this.firstName + '-' + this.lastName
    }
  },
  computed: {
    fullName() {
      return this.firstName + '-' + this.lastName
    }
  }
}
</script>
```

**运行效果**：三行都是 `zhang-san`。区别在写法——计算属性当变量用，方法当函数用。

**一句话区分**：`data` 是**存**的，`computed` 是**算**的，`methods` 是**被调**的。

**必须强调**：计算属性里**不能写异步**。`setTimeout`、`axios`、`fetch` 都不行——它必须同步返回一个值。要异步，用 `watch` 或生命周期钩子。

**课堂练习**：用计算属性算出购物车总价（`list` 里若干商品，各含 `price`、`num`），要求模板里只写 `{{ total }}`。

**本节小结**：算出来的值起个名字，用的时候不带括号。

---

### 13. 计算属性的 getter

**本节目标**

- 知道 `fullName() {}` 是 `{ get() {} }` 的简写
- 为下一节的 `set` 做铺垫

**讲解**

学生写完上一节会以为 `computed` 里只能写函数。其实那是简写。完整形式是一个对象，里面至少有个 `get`。理解这一点，下一步加 `set` 才不突兀。

```vue
<template>
  <div>
    <div>{{ firstName }}-{{ lastName }}</div>
    <!-- 计算属性 -->
    <div>计算属性得到的：{{ fullName }}</div>
    <!-- 方法 -->
    <div>方法得到的：{{ getFullName() }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      firstName: 'zhang',
      lastName: 'san'
    }
  },
  methods: {
    getFullName() {
      return this.firstName + '-' + this.lastName
    }
  },
  computed: {
    // 简写形式，平时开发中最常用
    // fullName() {
    //   return this.firstName + '-' + this.lastName
    // }

    // 完整形式：只有 getter
    fullName: {
      get() {
        return this.firstName + '-' + this.lastName
      }
    }
  }
}
</script>
```

**课堂现场做这个实验**：在任意按钮里写 `this.fullName = 'li-si'`，控制台会报

```
Computed property "fullName" was assigned to but it has no setter.
```

这就是"只有 getter 的计算属性是只读的"的实证。**让学生亲手触发一次报错**，比讲定义印象深。

**本节小结**：`fullName() {}` ≡ `{ get() {} }`；只有 get 就是只读。

---

### 14. v-model 的基本使用

**本节目标**

- 会用 `v-model` 做输入框双向绑定
- 理解 `v-model` 的本质是 `:value` + `@input`

**讲解**

前面的绑定都是单向的：数据 → 页面。表单要求反过来：用户输入 → 数据也要变。`v-model` 就是干这个的。

原始文件整段被注释了（`<!-- ... -->` 从第一行包到最后一行），复制进去是空白页，这一节补成完整可运行版本。

```vue
<template>
  <div>
    <!-- 1. 最基础：输入框和数据显示同步 -->
    <div>{{ msg }}</div>
    <input type="text" placeholder="请输入内容" v-model="msg" />

    <hr />

    <!-- 2. v-model 的本质：:value 绑定 + @input 改数据 -->
    <input type="text" :value="msg" @input="msg = $event.target.value" />

    <hr />

    <!-- 3. 多行文本 -->
    <textarea v-model="desc" placeholder="多行文本"></textarea>
    <p>desc：{{ desc }}</p>

    <!-- 4. 复选框：单个是布尔，多个是数组 -->
    <label><input type="checkbox" v-model="agree" /> 同意协议</label>
    <p>agree：{{ agree }}</p>

    <!-- 5. 下拉框 -->
    <select v-model="city">
      <option value="hf">合肥</option>
      <option value="sh">上海</option>
      <option value="bj">北京</option>
    </select>
    <p>city：{{ city }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      msg: '你好',
      desc: '',
      agree: false,
      city: 'hf'
    }
  }
}
</script>
```

**第 2 段是全篇的钥匙**：`v-model` 不是魔法，它就是一个语法糖。把这段抄在黑板上，学生以后写自定义组件支持 `v-model` 就不会懵。

```html
<input :value="msg" @input="msg = $event.target.value" />
<!-- 等价于 -->
<input v-model="msg" />
```

**易错点**：`v-model` 只对**表单元素**和**组件**有效，写在 `<div>` 上没有任何作用（也不会报错，最难查）。

**课堂练习**：做一个"实时字数统计"——textarea 里输入，下面显示"已输入 N 字，还可输入 X 字"（上限 200）。

**本节小结**：`v-model` = `:value` + `@input`，只对表单和组件有效。

---

### 15. 计算属性的完整写法（get + set）

**本节目标**

- 会给计算属性加 `set`
- 理解"读时走 get，写时走 set"

**讲解**

上一节的计算属性只能读。如果想让 `v-model` 绑到计算属性上（改它，反过来更新源数据），就必须有 `set`。

场景：页面上三个输入框，改 `firstName` 或 `lastName`，`fullName` 自动变；反过来，直接改 `fullName` 也要能拆回前两个。这是"双向的派生关系"。

```vue
<template>
  <div>
    firstName：
    <input type="text" v-model="firstName" />
    lastName：
    <input type="text" v-model="lastName" />
    fullName：
    <input type="text" v-model="fullName" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      firstName: 'zhang',
      lastName: 'san'
    }
  },
  computed: {
    // 平时开发中常写的简写形式（只读）
    // fullName() {
    //   return this.firstName + '-' + this.lastName
    // }

    // 完全体：get + set
    fullName: {
      get() {
        return this.firstName + '-' + this.lastName
      },
      set(newVal) {
        console.log('set 收到的值：', newVal)
        const names = newVal.split('-')
        this.firstName = names[0]
        this.lastName = names[1]
      }
    }
  }
}
</script>
```

**运行效果**：在 fullName 框里输入 `li-si`，上面两个框同步变成 `li` 和 `si`。

**两个坑（必须讲）**

1. 只输 `lisi`（没有横杠）时，`names[1]` 是 `undefined`，lastName 框变空。真实项目要兜底：`this.lastName = names[1] || ''`。
2. `get` 里**不要改数据**，否则死循环：渲染 → 改数据 → 重新渲染 → …

**课堂练习**：把 `fullName` 的分隔符换成空格，并处理"用户只输入一个词"的情况。

**本节小结**：`get` 负责读，`set` 负责写；写的时候记得校验和兜底。

---

### 16. 计算属性和方法的区别（缓存）

**本节目标**

- 用实验证明计算属性有缓存
- 能根据场景选择 `computed` 还是 `methods`

**讲解**

这是全课最后一个知识点，也是面试常问的。不讲理论，直接看控制台：同一个结果，模板里写四次，方法执行四次，计算属性只执行一次。

```vue
<template>
  <div>
    <!-- methods：即使 firstName 和 lastName 没变，也要执行 4 次 -->
    <h2>{{ getFullName() }}</h2>
    <h2>{{ getFullName() }}</h2>
    <h2>{{ getFullName() }}</h2>
    <h2>{{ getFullName() }}</h2>

    <!-- 计算属性：有缓存，只算 1 次 -->
    <h2>{{ fullName }}</h2>
    <h2>{{ fullName }}</h2>
    <h2>{{ fullName }}</h2>
    <h2>{{ fullName }}</h2>
  </div>
</template>

<script>
export default {
  data() {
    return {
      firstName: 'zhang',
      lastName: 'san'
    }
  },
  methods: {
    getFullName() {
      console.log('调用了 getFullName')
      return this.firstName + this.lastName
    }
  },
  computed: {
    fullName() {
      console.log('调用了 fullName')
      return this.firstName + this.lastName
    }
  }
}
</script>
```

**预期结果**：控制台里 `调用了 getFullName` 出现 **4 次**，`调用了 fullName` 只出现 **1 次**。

**为什么**：计算属性会缓存结果，只有它**依赖的响应式数据**（这里是 `firstName`、`lastName`）发生变化时才重新计算。

**选型口诀**

| 场景 | 选择 |
|---|---|
| 结果要多次引用、依赖不变 | `computed` |
| 需要传参数 | `methods` |
| 要做副作用（请求、定时器、打日志） | `methods` 或 `watch` |

**课堂练习**：在页面上加一个 `<input v-model="firstName">`，观察控制台——改一次输入，方法执行 4 次，计算属性执行 1 次。

**本节小结**：计算属性算一次存着，方法调一次算一次。

---

## 第五阶段｜v-model 进阶与组件化开发

> 阶段目标：把"一个页面写到底"改成"一个页面由一堆零件拼起来"。这一阶段结束后，学生应该能独立拆组件、定接口（props / 事件）、并且知道页面在什么时刻该去要数据。

### 17. v-model 的进阶用法与修饰符

**本节目标**

- 会使用 `.lazy`、`.number`、`.trim` 三个修饰符
- 会把一组复选框选中的值保存到数组中

**讲解**

修饰符写在 `v-model` 后面，用来调整数据同步的方式。

| 修饰符 | 作用 |
|---|---|
| `.lazy` | 输入框失焦或回车后再同步数据 |
| `.number` | 尝试把输入值转换为数字 |
| `.trim` | 自动去掉输入值首尾的空格 |

下面用一个紧凑的表单逐个观察它们的效果：

```vue
<template>
  <div>
    <h3>简单表单</h3>

    <label>姓名：<input v-model.lazy="name" /></label>
    <p>姓名：{{ name }}（失焦后更新）</p>

    <label>年龄：<input v-model.number="age" type="number" /></label>
    <p>年龄：{{ age }}，类型：{{ typeof age }}</p>

    <label>关键字：<input v-model.trim="keyword" /></label>
    <p>关键字：[{{ keyword }}]</p>

    <p>爱好：</p>
    <label v-for="hobby in hobbyList" :key="hobby">
      <input v-model="hobbies" type="checkbox" :value="hobby" />
      {{ hobby }}
    </label>
    <p>已选爱好：{{ hobbies }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: '小明',
      age: 18,
      keyword: '',
      hobbyList: ['阅读', '音乐', '运动'],
      hobbies: []
    }
  }
}
</script>
```

复选框组的 `v-model` 绑定同一个数组。勾选时，当前复选框的 `value` 会加入数组；取消勾选时，会从数组中移除。

**课堂练习**

给表单增加“城市”输入框，使用 `v-model.trim` 绑定 `city`，并在下方显示去掉首尾空格后的内容。

**本节小结**：`.lazy` 改变同步时机，`.number` 转换数字，`.trim` 去掉首尾空格；复选框组可以把多个选中值保存到数组。

---

### 18. 组件化开发：第一个组件

**本节目标**

- 会创建一个简单的 `.vue` 组件
- 会在父组件中局部注册并使用它
- 记住组件的 `data` 必须是函数

**讲解**

组件就是可以重复使用的页面零件。我们先做一个只显示问候语的组件，再在 `App.vue` 中使用它。

**子组件 `src/components/HelloMessage.vue`**

```vue
<template>
  <div class="hello">
    <h3>{{ message }}</h3>
  </div>
</template>

<script>
export default {
  name: 'HelloMessage',
  data() {
    return {
      message: '你好，这是我的第一个组件！'
    }
  }
}
</script>

<style scoped>
.hello {
  color: #42b983;
}
</style>
```

组件的 `data` 必须写成函数并返回对象，这样每次使用组件时都能得到一份独立的数据。`scoped` 表示这里的样式只作用于当前组件。

**父组件 `App.vue`：局部注册**

```vue
<template>
  <div>
    <h2>组件演示</h2>
    <HelloMessage />
  </div>
</template>

<script>
import HelloMessage from './components/HelloMessage.vue'

export default {
  components: {
    HelloMessage
  }
}
</script>
```

局部注册分三步：先用 `import` 引入，再写进 `components`，最后在模板中当成标签使用。



全局注册也能让组件在所有页面使用，但本节不展开，日常练习或者开发优先使用局部注册。

**课堂练习**

把 `message` 改成自己的名字，再在 `App.vue` 中多使用一次 `<HelloMessage />`，观察页面变化。

**本节小结**：组件可以复用；局部注册依次完成“引入、注册、使用”；组件的 `data` 必须是函数。

---

### 19. 组件通讯（一）：props 父传子

**本节目标**

- 会用 props 把父组件的数据传给子组件
- 会写数组和对象两种 props 声明
- 会使用 `type`、`default`、`required`，并知道 props 只读

**讲解**

props 是子组件对外声明的“接收口”。父组件在使用子组件时传值，子组件通过 props 接收并显示。

props 最简单的数组写法是：

```js
props: ['name', 'age']
```

如果需要说明类型、是否必填和默认值，就使用对象写法。下面的 `name` 是必填字符串，`age` 是有默认值的数字。

**子组件 `src/components/UserCard.vue`**

```vue
<template>
  <div>
    <h3>{{ name }}</h3>
    <p>年龄：{{ age }}</p>
  </div>
</template>

<script>
export default {
  name: 'UserCard',
  props: {
    name: {
      type: String,
      required: true
    },
    age: {
      type: Number,
      default: 18
    }
  }
}
</script>
```

**父组件 `App.vue`**

```vue
<template>
  <div>
    <UserCard :name="user.name" :age="user.age" />
    <UserCard name="小红" />
  </div>
</template>

<script>
import UserCard from './components/UserCard.vue'

export default {
  components: { UserCard },
  data() {
    return {
      user: {
        name: '小明',
        age: 20
      }
    }
  }
}
</script>
```

第一张卡片显示“小明、20 岁”。第二张卡片没有传 `age`，所以显示默认值 `18`。传字符串可以直接写 `name="小红"`；传变量或数字时要加 `:`，例如 `:age="user.age"`。

**props 是只读的**：子组件可以显示 props，但不要直接修改它，例如不要写 `this.name = '别的名字'`。数据属于父组件，需要修改时应由父组件处理。

**课堂练习**

给 `UserCard` 增加 `city` prop，类型为 `String`，默认值为 `'合肥'`，并在卡片中显示城市。

**本节小结**：父组件通过属性传值，子组件用 props 接收；对象声明可以配置 `type`、`default`、`required`；props 只读。

---

### 20. 组件通讯（二）：自定义事件 $emit 子传父

**本节目标**

- 会用 `this.$emit()` 从子组件发送一个自定义事件
- 会在父组件中接收事件并修改自己的数据

**讲解**

子组件的 props 只读，但子组件可以通知父组件：“请修改你的数据。”本节只做一个功能：点击子组件的“删除”按钮，让父组件删除对应事项。

流程是：子组件用 `$emit` 发事件并携带 id，父组件用 `@事件名` 接收，最后由父组件修改数组。

**子组件 `src/components/TodoItem.vue`**

```vue
<template>
  <li>
    {{ todo.text }}
    <button @click="remove">删除</button>
  </li>
</template>

<script>
export default {
  name: 'TodoItem',
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  methods: {
    remove() {
      this.$emit('remove', this.todo.id)
    }
  }
}
</script>
```

`$emit` 的第一个参数是事件名 `remove`，第二个参数是要交给父组件的 id。

**父组件 `App.vue`**

```vue
<template>
  <div>
    <h3>我的待办</h3>
    <ul>
      <TodoItem
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        @remove="removeTodo"
      />
    </ul>
  </div>
</template>

<script>
import TodoItem from './components/TodoItem.vue'

export default {
  components: { TodoItem },
  data() {
    return {
      todos: [
        { id: 1, text: '学习 Vue' },
        { id: 2, text: '完成作业' }
      ]
    }
  },
  methods: {
    removeTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id)
    }
  }
}
</script>
```

点击“删除”后，子组件只负责发出通知，真正删除数据的是父组件的 `removeTodo`。

**课堂练习**

在 `todos` 中增加一项“整理房间”，运行页面并点击它的“删除”按钮，确认只删除这一项。

**本节小结**：子组件用 `$emit('事件名', 数据)` 发通知，父组件用 `@事件名="方法"` 接收并修改数据。

---

### 21. 组件通讯（三）：ref 标记标签和组件

**本节目标**

- 会用 `ref` 获取普通标签对应的 DOM 元素
- 会用 `ref` 获取子组件实例，并调用子组件的方法
- 能分清 `this.$refs` 在两种情况下的取值

**讲解**

`ref` 用来给标签或组件起一个引用名称，之后可以通过 `this.$refs.名字` 获取它。它作用在普通标签和组件标签上时，获取到的内容不同。

**1. `ref` 作用在普通标签上：获取 DOM 元素**

下面的输入框写了 `ref="keyword"`，点击按钮后，`this.$refs.keyword` 就是这个输入框的 DOM 元素，因此可以调用它的 `focus()` 方法。

```vue
<template>
  <div>
    <input ref="keyword" placeholder="请输入关键字" />
    <button @click="focusInput">聚焦输入框</button>
  </div>
</template>

<script>
export default {
  methods: {
    focusInput() {
      this.$refs.keyword.focus()
    }
  }
}
</script>
```

这里不需要生命周期：代码只在点击按钮后执行，此时输入框已经显示在页面中。普通数据变化仍应通过响应式数据完成，`ref` 主要用于聚焦、播放视频等必须直接操作 DOM 的场景。

**2. `ref` 作用在组件标签上：获取子组件实例**（用的不多）

前面学习了父组件通过 props 传数据、子组件通过 `$emit` 发事件。`ref` 还可以让父组件获取子组件实例，直接调用子组件的方法。

下面做一个计数器：子组件负责计数，父组件通过 `ref` 调用子组件的 `reset` 方法，把计数清零。

**子组件 `src/components/ChildCounter.vue`**

```vue
<template>
  <div>
    <p>子组件计数：{{ count }}</p>
    <button @click="count++">子组件 +1</button>
  </div>
</template>

<script>
export default {
  name: 'ChildCounter',
  data() {
    return {
      count: 0
    }
  },
  methods: {
    reset() {
      this.count = 0
    }
  }
}
</script>
```

**父组件 `App.vue`**

```vue
<template>
  <div>
    <h3>父组件</h3>
    <ChildCounter ref="childCounter" />
    <button @click="resetChild">父组件：将子组件清零</button>
  </div>
</template>

<script>
import ChildCounter from './components/ChildCounter.vue'

export default {
  components: { ChildCounter },
  methods: {
    resetChild() {
      // childCounter 与上面的 ref 名称一致
      this.$refs.childCounter.reset()
    }
  }
}
</script>
```

运行后，页面会显示计数和两个按钮。先点击“子组件 +1”，再点击“父组件：将子组件清零”，可以看到子组件的数字变为 0。

**3. 两种情况对比**

| `ref` 写在哪里 | `this.$refs.名字` 获取到什么 | 可以做什么 |
|---|---|---|
| 普通标签，如 `<input ref="keyword">` | DOM 元素 | 调用 `focus()` 等 DOM 方法 |
| 组件标签，如 `<ChildCounter ref="childCounter">` | 子组件实例 | 调用子组件的 `reset()` 等方法 |

两种情况的 `ref` 名称都要与 `$refs` 后面的名称一致。普通数据传递仍优先使用 props 和 `$emit`；`ref` 适合聚焦、清零等需要主动触发某个动作的场景。

**课堂练习**

1. 添加一个 `ref="username"` 的输入框和一个按钮，点击按钮时通过 `this.$refs.username.focus()` 让它获得焦点。
2. 给子组件增加一个 `addFive` 方法，让 `count` 一次增加 5；再在父组件增加一个“父组件：加 5”按钮，通过 `this.$refs.childCounter.addFive()` 调用它。

**本节小结**：`ref` 写在普通标签上时，`$refs` 获取的是 DOM 元素；写在组件标签上时，获取的是子组件实例。前者可操作 DOM，后者可调用子组件方法。

---

### 22. 组件通讯（四）：事件总线与 provide / inject 依赖注入 （用的比较少）

**本节目标**

- 会用 EventBus 让一个组件发送消息、另一个组件接收消息
- 会用 `provide` / `inject` 从祖先组件向后代组件传一个值

**讲解**

两个组件没有直接的父子关系时，可以用一个空的 Vue 实例作为“事件中转站”，这就是 EventBus（事件总线）。

**第一步：在 `main.js` 中创建 `$bus`**

```js
import Vue from 'vue'
import App from './App.vue'

Vue.prototype.$bus = new Vue()

new Vue({
  render: h => h(App)
}).$mount('#app')
```

**第二步：发送方 `src/components/Sender.vue`**

```vue
<template>
  <button @click="sendMessage">发送问候</button>
</template>

<script>
export default {
  methods: {
    sendMessage() {
      this.$bus.$emit('message-change', '你好，接收方！')
    }
  }
}
</script>
```

**第三步：接收方 `src/components/Receiver.vue`**

```vue
<template>
  <div>
     <p>收到的消息：{{ message }}</p>
     <SenderMsg></SenderMsg>
  </div>
</template>

<script>
import SenderMsg from './components/SenderMsg.vue'
export default {
  data() {
    return {
      message: '暂无消息'
    }
  },
  created() {
    this.$bus.$on('message-change', this.receiveMessage)
  },
  beforeDestroy() {
    this.$bus.$off('message-change', this.receiveMessage)
  },
  methods: {
    receiveMessage(message) {
      this.message = message
    }
  },
  components: {
    SenderMsg
  }
}
</script>
```

把 `Sender` 和 `Receiver` 注册到 `App.vue` 并放在页面中，点击按钮即可看到消息变化。`$emit` 负责发送，`$on` 负责监听；组件销毁前要用 `$off` 取消监听，事件名和回调函数都要与监听时一致。

**provide / inject 的基本传值**（用的比较少 是父亲和后代组件进行通讯）

祖先组件用 `provide` 提供数据，后代组件用 `inject` 接收，中间组件不需要传递。

```js
// 祖先组件 App.vue
export default {
   provide() {
       return {
           appName:'我是父组件App里面的内容'
       }
   },
}
```

```vue
<!-- 任意深度的后代组件 -->
<template>
  <p>{{ appName }}</p>
</template>

<script>
export default {
  inject: ['appName']
}
</script>
```

**课堂练习**

把发送方的事件参数改成“今天也要加油”，运行页面，观察接收方显示的内容。

**本节小结**：EventBus 使用 `$emit`、`$on`、`$off` 完成发送、监听和取消监听；`provide` / `inject` 可以从祖先组件向后代组件传值。

---

### 23. 插槽 slot：让组件"留个口"  （重要）

**本节目标**

- 会用默认插槽在组件中放置内容
- 会用具名插槽放置指定区域的内容

**讲解**

props 传递的是数据，插槽传递的是模板内容。下面做一个小卡片：卡片结构由子组件提供，标题和正文由父组件填写。

**子组件 `src/components/MyCard.vue`**

```vue
<template>
  <div class="card">
    <div class="card-header">
      <slot name="header">默认标题</slot>
    </div>
    <div class="card-body">
      <slot>暂无内容</slot>
    </div>
  </div>
</template>

<style scoped>
.card {
  width: 240px;
  border: 1px solid #ddd;
  border-radius: 4px;
}
.card-header,
.card-body {
  padding: 12px;
}
.card-header {
  border-bottom: 1px solid #ddd;
}
</style>
```

没有 `name` 的 `<slot>` 是默认插槽，用来接收普通内容；`name="header"` 表示名为 `header` 的具名插槽。父组件没有提供内容时，会显示插槽中的备用文字。

**父组件 `App.vue`**

```vue
<template>
  <div>
    <MyCard>
      <template #header>
        <strong>每日提醒</strong>
      </template>

      <p>完成今天的 Vue 练习。</p>
    </MyCard>
  </div>
</template>

<script>
import MyCard from './components/MyCard.vue'

export default {
  components: { MyCard }
}
</script>
```

`#header` 会把内容放进名为 `header` 的插槽；没有包在具名模板中的 `<p>` 会进入默认插槽。

**课堂练习**

把卡片的标题改为“学习计划”，正文改为“复习组件通讯”，观察两个插槽内容的位置。

**本节小结**：默认插槽接收普通内容，具名插槽按名字接收指定区域的内容；插槽中的文字可以作为备用内容。

---

### 24. Vue 的生命周期 （重要）

**本节目标**

- 记住 8 个生命周期钩子的名字和**触发时机**
- 明确"后端请求该写在哪个钩子里"，并说清理由
- 知道 `keep-alive` 会多出两个钩子

**讲解**

生命周期就是"组件从生到死"的几个关键时刻。Vue 允许你在这些时刻插入自己的代码——本质是一组**回调函数**，Vue 在特定时机替你调用。

**8 个钩子（按执行顺序）**

| 顺序 | 钩子 | 触发时机 | 此时能不能拿到 DOM | 常见用途 |
|---|---|---|---|---|
| 1 | `beforeCreate` | 实例刚创建，`data`、`methods` **都还没初始化** | 不能 | 极少用（插件初始化） |
| 2 | `created`（重要） | `data`、`methods`、`computed` 已可用，**DOM 还没有** | 不能 | **向后端发初始请求、赋值** |
| 3 | `beforeMount` | 模板编译完成，即将挂载到页面 | 不能（还没替换） | 极少用 |
| 4 | `mounted`（重要） | 已挂载到页面，**DOM 可访问** | 能 | **操作 DOM、初始化图表、拿 `$refs`** |
| 5 | `beforeUpdate` | 数据变了，**页面还没重新渲染** | 能（旧 DOM） | 更新前记录滚动位置 |
| 6 | `updated` | 数据和页面都已是新的 | 能（新 DOM） | 数据变化后操作 DOM（少用，易死循环） |
| 7 | `beforeDestroy` | 实例即将销毁，**此时还有 `this`** | 能 | **清定时器、解绑 `$bus`、取消请求** |
| 8 | `destroyed` | 实例已销毁，`this` 上的东西基本没了 | 不能 | 极少用 |

```vue
<template>
  <div>
    <h3>{{ title }}</h3>
    <p>共 {{ list.length }} 条</p>
    <div ref="box">我会在 mounted 里被改样式</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: '生命周期演示',
      list: [],
      timer: null
    }
  },
  beforeCreate() {
    console.log('1 beforeCreate：data 是', this.title) // undefined
  },
  created() {
    console.log('2 created：data 是', this.title) // 有值了
    console.log('2 created：$el 是', this.$el) // undefined，DOM 还没有
    this.fetchList() // ★ 初始请求放这里
  },
  beforeMount() {
    console.log('3 beforeMount：$el 是', this.$el) // 有节点，但还没替换成真实内容
  },
  mounted() {
    console.log('4 mounted：$el 是', this.$el) // 真实 DOM，可以操作
    this.$refs.box.style.background = '#ffe'
    this.timer = setInterval(() => console.log('定时器在跑'), 3000)
  },
  beforeUpdate() {
    console.log('5 beforeUpdate：页面还是旧的')
  },
  updated() {
    console.log('6 updated：页面已更新')
  },
  beforeDestroy() {
    console.log('7 beforeDestroy：可以在这里做清理')
    clearInterval(this.timer) // ★ 定时器必须清
    this.timer = null
  },
  destroyed() {
    console.log('8 destroyed：已经没了')
  },
  methods: {
    async fetchList() {
      // 假装在请求后端
      this.list = [{ id: 1 }, { id: 2 }, { id: 3 }]
    }
  }
}
</script>
```

**后端请求到底写在哪：`created` 还是 `mounted`？**

这是面试和实战都会被问的问题。结论：**写 `created`，特殊情况才写 `mounted`**。

理由三条：

1. **更早**。`created` 比 `mounted` 早一步执行，请求能早一点发出去，首屏能快一点。
2. **不依赖 DOM**。请求只关心数据，跟页面渲染没关系，放 `mounted` 属于"等你渲染完了我才开始"，白白浪费一段渲染时间。
3. **服务端渲染（SSR）也能跑到**。`mounted` 在服务端**根本不会执行**，把请求写那儿，SSR 场景下首屏永远是空的。

**什么时候必须写 `mounted`**：

- 需要拿到 DOM 元素做初始化（ECharts 初始化要容器、地图要 canvas、轮播要量宽度）
- 需要先测量布局再决定请求参数（比如"按容器宽度决定拉几条数据"）
- 需要操作 `$refs`

**要清东西的地方就在 `beforeDestroy`**

这一条比请求位置更常出事。三个必清项：

```js
beforeDestroy() {
  clearInterval(this.timer)          // 定时器
  this.$bus.$off('some-event', this.handler) // 事件总线
  window.removeEventListener('resize', this.onResize) // 全局事件
}
```

不清的后果：组件切走了定时器还在跑、回调还在改一个已经销毁的组件的数据，控制台一堆报错，页面越用越卡。

**`keep-alive` 会多两个钩子**（了解）

用 `<keep-alive>` 包住的组件不会被销毁，而是被缓存。此时 `beforeDestroy` / `destroyed` **不会触发**，换成：

| 钩子 | 触发时机 |
|---|---|
| `activated` | 组件被激活（第一次进入 + 每次从缓存切回来） |
| `deactivated` | 组件被缓存起来（切走时） |

```html
<keep-alive>
  <router-view />
</keep-alive>
```

**课堂练习**

1. 在 8 个钩子里各打一条日志，从 A 页面跳到 B 页面再跳回来，把控制台的顺序抄下来，标出哪些只执行一次、哪些每次都执行。
2. 写一个组件，每 1 秒给自己加 1，然后给它加一个"销毁"按钮（用 `v-if` 控制），验证 `beforeDestroy` 有没有清掉定时器。

**本节小结**：`created` 发请求、`mounted` 碰 DOM、`beforeDestroy` 做清理；`keep-alive` 里换成 `activated` / `deactivated`。



---

## 第六阶段｜网络请求与路由

> 阶段目标：从"本地假数据"跨到"真后端"，再跨到"多页面"。这一阶段结束，学生应该能独立接一个真实接口，并做出带导航、带参数、带登录拦截的多页面应用。

### 26. axios 的基本使用

**本节目标**

- 会安装 axios，并用 `async / await` 发送 GET 请求
- 会显示最基本的加载中和请求失败提示
- 会发送一个简单的 POST 请求

**讲解**

Vue 本身不提供网络请求功能，先安装 axios：

```bash
npm install axios
```

下面使用免费的 JSONPlaceholder 测试接口。打开页面后发送 GET 请求，把返回的 5 篇文章显示成列表。

```vue
<template>
  <div>
    <h3>文章列表</h3>
    <p v-if="loading">加载中...</p>
    <p v-else-if="error">{{ error }}</p>
    <ul v-else>
      <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
    </ul>

    <button @click="addPost">模拟新增文章</button>
    <p>{{ message }}</p>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      posts: [],
      loading: false,
      error: '',
      message: ''
    }
  },
  created() {
    this.getPosts()
  },
  methods: {
    async getPosts() {
      this.loading = true
      this.error = ''
      try {
        const res = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: { _limit: 5 }
        })
        this.posts = res.data
      } catch (e) {
        this.error = '加载失败，请稍后重试'
      } finally {
        this.loading = false
      }
    },
    async addPost() {
      const res = await axios.post('https://jsonplaceholder.typicode.com/posts', {
        title: '我的新文章',
        body: '这是一段测试内容',
        userId: 1
      })
      this.message = `新增成功，模拟返回的 id 是 ${res.data.id}`
    }
  }
}
</script>
```

`loading` 在请求开始时设为 `true`，在 `finally` 中恢复为 `false`；请求出错会进入 `catch`，给 `error` 赋值。接口返回的数据在 `res.data` 中。

GET 示例中的 `params` 会拼接到 URL 后面；POST 示例的第二个参数是请求体，也就是 `data`。可以简单记成：**GET 查询参数常用 `params`，POST 提交数据常用 `data`。**

**课堂练习**

把 GET 请求的 `_limit` 改为 `3`，再把 POST 的 `title` 改成自己的文章标题，观察页面变化。

**本节小结**：用 `async / await` 配合 `try...catch...finally` 处理请求、错误和加载状态；GET 常用 `params`，POST 常用 `data`。

---

### 27. axios 封装与跨域代理

**本节目标**

- 会用 `axios.create()` 创建统一的请求实例
- 会写一个简单的响应拦截器并封装 GET 请求
- 会配置最小的开发环境跨域代理

**讲解**

每个组件都写完整接口地址，以后修改起来很麻烦。可以把公共配置放进 `request.js`，再把具体接口单独封装。

**第一步：`src/utils/request.js`**

```js
import axios from 'axios'

const request = axios.create({
  baseURL: '/api',
  timeout: 5000
})

request.interceptors.response.use(
  response => response.data,
  error => Promise.reject(error)
)

export default request
```

`baseURL: '/api'` 会自动加到请求地址前。响应拦截器直接返回 `response.data`，所以组件拿到的就是接口数据。

**第二步：封装一个 GET 请求 `src/api/post.js`**

```js
import request from '@/utils/request'

export function getPostList() {
  return request.get('/posts', {
    params: { _limit: 5 }
  })
}
```

**第三步：在组件中调用**

```vue
<template>
  <ul>
    <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
  </ul>
</template>

<script>
import { getPostList } from '@/api/post'

export default {
  data() {
    return {
      posts: []
    }
  },
  created() {
    this.loadPosts()
  },
  methods: {
    async loadPosts() {
      this.posts = await getPostList()
    }
  }
}
</script>
```

**开发环境跨域代理**

上面的 `/api` 请求还需要代理到测试服务器。在项目根目录的 `vue.config.js` 中写入：

```js
module.exports = {
  devServer: {
    proxy: {
      '/api': {
        target: 'https://jsonplaceholder.typicode.com',
        changeOrigin: true,
        pathRewrite: {
          '^/api': ''
        }
      }
    }
  }
}
```

修改配置后重启开发服务器。这样 `/api/posts` 会由开发服务器转发为 `https://jsonplaceholder.typicode.com/posts`，浏览器不再直接跨域；这个代理只用于开发环境。

**课堂练习**

把 `getPostList` 中的 `_limit` 改为 `3`，确认组件仍然只需调用 `getPostList()` 就能显示列表。

**本节小结**：`axios.create()` 保存公共配置，响应拦截器统一取出 `response.data`，`api` 文件封装具体请求；开发环境可用 `devServer.proxy` 转发请求。

---

### 28. Vue Router 起步：路由是什么，怎么配

**本节目标**

- 会安装 Vue2 对应的 `vue-router@3`
- 会配置 Home 和 About 两个页面
- 会使用 `router-link` 导航和 `router-view` 显示页面

**讲解**

前端路由让页面切换时不重新加载整个网页。URL 发生变化后，匹配到的组件会显示在 `<router-view>` 中。

**安装**

```bash
npm install vue-router@3
```

Vue2 使用 vue-router 3.x。先准备两个简单页面：

```vue
<!-- src/views/Home.vue -->
<template>
  <h2>首页</h2>
</template>
```

```vue
<!-- src/views/About.vue -->
<template>
  <h2>关于我们</h2>
</template>
```

**第一步：配置路由表 `src/router/index.js`**

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '@/views/Home.vue'
import About from '@/views/About.vue'

Vue.use(VueRouter)

const routes = [
  { path: '/', component: Home },
  { path: '/about', component: About }
]

const router = new VueRouter({
  routes
})

export default router
```

这里没有设置 `mode`，所以使用默认的 hash 模式，地址会类似 `http://localhost:8080/#/about`。

**第二步：在 `main.js` 中挂载路由**

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'

new Vue({
  router,
  render: h => h(App)
}).$mount('#app')
```

**第三步：在 `App.vue` 中写导航和路由出口**

```vue
<template>
  <div id="app">
    <nav>
      <router-link to="/">首页</router-link> |
      <router-link to="/about">关于</router-link>
    </nav>
    <hr />
    <router-view />
  </div>
</template>
```

点击 `router-link` 会切换地址，`router-view` 会显示对应的页面组件。以后如果看到 `history` 模式，只需先知道它去掉了 `#`，但部署时需要额外的服务器配置。

**课堂练习**

点击“首页”和“关于”，观察地址栏与 `router-view` 中的内容；再把 About 页面文字改成“关于我的网站”。

**本节小结**：Vue2 配置 `vue-router@3`；写好路由表并在 `main.js` 挂载后，用 `router-link` 导航、用 `router-view` 显示页面；入门时使用默认 hash 模式即可。

---

### 29. 路由跳转与传参

**本节目标**

- 分清 `$route` 和 `$router`
- 会用 `push` 跳转页面
- 会分别用 `params` 和 `query` 传递一个参数

**讲解**

| 对象 | 作用 | 例子 |
|---|---|---|
| `$route` | 读取当前路由的信息 | `$route.params.id`、`$route.query.keyword` |
| `$router` | 调用路由的跳转方法 | `$router.push('/about')` |

可以记成：`$route` 表示“我在哪”，`$router` 负责“我要去哪”。`push` 会跳转到新页面，并保留浏览历史。

在上一节的路由表中引入两个页面，并添加两条规则：

```js
import MovieDetail from '@/views/MovieDetail.vue'
import Search from '@/views/Search.vue'

// 添加到 routes 数组中
{ path: '/movie/:id', name: 'MovieDetail', component: MovieDetail },
{ path: '/search', component: Search }
```

**params：把 id 放在路径中**

`/movie/:id` 中的 `:id` 是路径占位符。使用命名路由跳转时，`params.id` 会填到这个位置。

```vue
<!-- src/views/Home.vue -->
<template>
  <div>
    <h2>首页</h2>
    <button @click="goMovie">查看 1 号电影</button>
    <button @click="goSearch">搜索 Vue</button>
  </div>
</template>

<script>
export default {
  methods: {
    goMovie() {
      this.$router.push({
        name: 'MovieDetail',
        params: { id: 1 }
      })
    },
    goSearch() {
      this.$router.push({
        path: '/search',
        query: { keyword: 'Vue' }
      })
    }
  }
}
</script>
```

点击第一个按钮后，地址是 `/movie/1`，详情页通过 `$route.params.id` 取值：

```vue
<!-- src/views/MovieDetail.vue -->
<template>
  <h2>电影详情，id：{{ $route.params.id }}</h2>
</template>
```

**query：把搜索词放在问号后面**

点击第二个按钮后，地址是 `/search?keyword=Vue`。query 不需要在路径中设置占位符，目标页面通过 `$route.query.keyword` 取值：

```vue
<!-- src/views/Search.vue -->
<template>
  <h2>搜索内容：{{ $route.query.keyword }}</h2>
</template>
```

**课堂练习**

把 `params.id` 改成 `2`，把 `query.keyword` 改成 `Router`，分别点击两个按钮，观察地址和目标页面。

**本节小结**：`$router.push` 负责跳转，`$route` 负责读取参数；`params` 配合路径中的 `:id` 使用，`query` 以 `?keyword=Vue` 的形式放在地址后面。

---

### 30. 嵌套路由、重定向与路由守卫

**本节目标**

- 会用 `children` 配置一个简单的嵌套路由
- 会用 `redirect` 设置默认页面
- 会用最简 `beforeEach` 做登录拦截

**讲解**

如果多个页面共用同一组导航，可以把导航放在 Layout 中，让子页面显示在 Layout 的 `router-view` 里。

**`src/router/index.js`**

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Login from '@/views/Login.vue'
import Layout from '@/layout/Layout.vue'
import Home from '@/views/Home.vue'
import User from '@/views/User.vue'

Vue.use(VueRouter)

const routes = [
  { path: '/login', component: Login },
  {
    path: '/',
    component: Layout,
    redirect: '/home',
    children: [
      {
        path: 'home',
        component: Home,
        meta: { requiresAuth: true }
      },
      {
        path: 'user',
        component: User,
        meta: { requiresAuth: true }
      }
    ]
  }
]

const router = new VueRouter({ routes })

router.beforeEach((to, from, next) => {
  const token = localStorage.getItem('token')
  if (to.meta.requiresAuth && !token) {
    next('/login')
  } else {
    next()
  }
})

export default router
```

子路由的 `path` 写成 `home` 和 `user`，不要加 `/`，它们分别对应 `/home` 和 `/user`。访问 `/` 时，`redirect` 会自动转到 `/home`。

`meta.requiresAuth` 是我们自己添加的登录标记。`beforeEach` 会在跳转前检查它：需要登录但没有 token 就去登录页，否则调用 `next()` 放行。

**`src/layout/Layout.vue`**

```vue
<template>
  <div>
    <nav>
      <router-link to="/home">首页</router-link> |
      <router-link to="/user">用户页</router-link>
    </nav>
    <hr />
    <router-view />
  </div>
</template>
```

切换子页面时，上面的导航继续保留，只有 Layout 中的 `router-view` 会更换内容。两个子页面先写简单文字即可：

```vue
<!-- src/views/Home.vue -->
<template><h2>后台首页</h2></template>
```

```vue
<!-- src/views/User.vue -->
<template><h2>用户页面</h2></template>
```

为了看到拦截效果，用一个按钮模拟登录：

```vue
<!-- src/views/Login.vue -->
<template>
  <div>
    <h2>请先登录</h2>
    <button @click="login">模拟登录</button>
  </div>
</template>

<script>
export default {
  methods: {
    login() {
      localStorage.setItem('token', 'demo-token')
      this.$router.push('/home')
    }
  }
}
</script>
```

这里的 token 只是课堂演示用的登录标记。清除浏览器中的这个 token 后访问 `/home`，就会被送到 `/login`；点击“模拟登录”后再回到首页。

**课堂练习**

在浏览器控制台执行 `localStorage.removeItem('token')`，再访问 `/user` 观察跳转；然后点击“模拟登录”，确认两个子页面都能正常切换。

**本节小结**：`children` 配置子页面，Layout 中的 `router-view` 显示子页面；`redirect` 设置默认地址；`beforeEach` 配合 `meta` 可以做最简登录拦截。

---

## 第七阶段｜Vuex 状态管理与自定义指令

> 阶段目标：解决"数据离得太远"和"DOM 操作重复写"这两件事。Vuex 是全局数据仓库，自定义指令是把重复的 DOM 操作封装成一句 `v-xxx`。

### 31. Vuex（一）：state 与 getters

**本节目标**

- 会创建 store，并挂到 Vue 实例上
- 会用 `state` 保存共享数据
- 会用 `getters` 计算数据

**讲解**

Vuex 用来保存多个组件都要用的数据。先记住五个部分的作用：

| 部分 | 作用 | 组件中怎么触发或读取 |
|---|---|---|
| `state` | 保存数据 | `$store.state.xxx` |
| `getters` | 根据 state 计算数据 | `$store.getters.xxx` |
| `mutations` | 同步修改 state | `$store.commit()` |
| `actions` | 处理异步操作 | `$store.dispatch()` |
| `modules` | 把数据拆成多个模块 | 使用时加模块名 |

**1. 安装**

```bash
npm install vuex@3
```

Vue2 使用 Vuex 3.x。

**2. 创建 `src/store/index.js`**

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  // state：相当于全局 data
  state: {
    count: 0
  },

  // getters：相当于全局 computed
  getters: {
    doubleCount(state) {
      return state.count * 2
    }
  }
})
```

**3. 在 `main.js` 中挂载 store**

```js
import Vue from 'vue'
import App from './App.vue'
import store from './store'

new Vue({
  store,
  render: h => h(App)
}).$mount('#app')
```

挂载后，组件中可以通过 `this.$store` 使用 Vuex，模板中可以直接写 `$store`。

**4. 组件中读取数据**

```vue
<template>
  <div>
    <p>原始值：{{ $store.state.count }}</p>
    <p>两倍值：{{ $store.getters.doubleCount }}</p>
  </div>
</template>
```

也可以用 `mapState` 和 `mapGetters`，把它们映射成计算属性：

```vue
<template>
  <div>
    <p>原始值：{{ count }}</p>
    <p>两倍值：{{ doubleCount }}</p>
  </div>
</template>

<script>
import { mapState, mapGetters } from 'vuex'

export default {
  computed: {
    ...mapState(['count']),
    ...mapGetters(['doubleCount'])
  }
}
</script>
```

**本节小结**：`state` 负责存数据，`getters` 负责算数据；`mapState` 和 `mapGetters` 都放在 `computed` 中。

---

### 32. Vuex（二）：mutations 与 actions

**本节目标**

- 会用 `mutations` 修改 state
- 会用 `actions` 处理异步操作
- 分清 `commit` 和 `dispatch`

**讲解**

**1. mutations：同步修改数据**

在刚才的 store 中添加 `mutations`：

```js
export default new Vuex.Store({
  state: {
    count: 0
  },
  getters: {
    doubleCount: state => state.count * 2
  },
  mutations: {
    // 第一个参数是 state，第二个参数是调用时传入的数据
    INCREMENT(state, step = 1) {
      state.count += step
    },
    RESET(state) {
      state.count = 0
    }
  }
})
```

组件中使用 `commit('mutation 名称', 参数)` 调用：

```vue
<template>
  <div>
    <p>{{ $store.state.count }}</p>
    <button @click="add">加 2</button>
    <button @click="reset">归零</button>
  </div>
</template>

<script>
export default {
  methods: {
    add() {
      this.$store.commit('INCREMENT', 2)
    },
    reset() {
      this.$store.commit('RESET')
    }
  }
}
</script>
```

**2. actions：处理异步操作**

异步任务放在 `actions` 中，任务完成后再通过 `commit` 调用 mutation：

```js
actions: {
  incrementAsync({ commit }, step) {
    setTimeout(() => {
      commit('INCREMENT', step)
    }, 1000)
  }
}
```

组件中使用 `dispatch('action 名称', 参数)` 调用：

```js
methods: {
  addLater() {
    this.$store.dispatch('incrementAsync', 2)
  }
}
```

**3. 使用辅助函数简化调用**

```js
import { mapMutations, mapActions } from 'vuex'

export default {
  methods: {
    ...mapMutations(['INCREMENT', 'RESET']),
    ...mapActions(['incrementAsync']),

    handleClick() {
      this.INCREMENT(2)
      this.incrementAsync(2)
    }
  }
}
```

**本节小结**：同步修改用 `commit` 调 mutation；异步操作用 `dispatch` 调 action，action 完成后再 commit mutation。

---

### 33. Vuex（三）：modules 拆分模块

**本节目标**

- 会把不同业务的数据拆成独立模块
- 会在组件中读取和调用模块里的内容

**讲解**

数据较多时，可以按业务拆分。例如用户数据放 `user.js`，购物车数据放 `cart.js`。每个模块都可以有自己的 `state`、`getters`、`mutations` 和 `actions`。

**1. 创建用户模块 `store/modules/user.js`**

```js
export default {
  // 开启命名空间，调用时加上 user/ 前缀
  namespaced: true,

  state: {
    name: '张三'
  },

  getters: {
    welcome(state) {
      return `你好，${state.name}`
    }
  },

  mutations: {
    SET_NAME(state, name) {
      state.name = name
    }
  },

  actions: {
    updateNameAsync({ commit }, name) {
      setTimeout(() => {
        commit('SET_NAME', name)
      }, 1000)
    }
  }
}
```

**2. 在 `store/index.js` 中注册模块**

```js
import Vue from 'vue'
import Vuex from 'vuex'
import user from './modules/user'

Vue.use(Vuex)

export default new Vuex.Store({
  modules: {
    user
  }
})
```

**3. 组件中使用模块**

```vue
<template>
  <div>
    <p>{{ name }}</p>
    <p>{{ welcome }}</p>
    <button @click="SET_NAME('李四')">修改名字</button>
    <button @click="updateNameAsync('王五')">一秒后修改名字</button>
  </div>
</template>

<script>
import { mapState, mapGetters, mapMutations, mapActions } from 'vuex'

export default {
  computed: {
    // 第一个参数是模块名
    ...mapState('user', ['name']),
    ...mapGetters('user', ['welcome'])
  },
  methods: {
    ...mapMutations('user', ['SET_NAME']),
    ...mapActions('user', ['updateNameAsync'])
  }
}
</script>
```

不使用辅助函数时，写法如下：

```js
// state：state 后面加模块名
this.$store.state.user.name

// getters、mutations、actions：名称前加 user/
this.$store.getters['user/welcome']
this.$store.commit('user/SET_NAME', '李四')
this.$store.dispatch('user/updateNameAsync', '王五')
```

**课堂练习**

1. 新建 `cart` 模块，用 state 保存商品数组。
2. 添加一个计算商品数量的 getter、一个添加商品的 mutation，并在页面中调用。

**本节小结**：`modules` 负责拆分数据；开启 `namespaced` 后，读取 getter 或调用 mutation、action 时记得加 `模块名/` 前缀。

---

### 34. 自定义指令

**本节目标**

- 会进行指令的全局注册和局部注册
- 会用 `v-focus` 让输入框自动聚焦
- 会用 `binding.value` 接收值，写出简单的 `v-color`

**讲解**

自定义指令可以把重复的 DOM 操作封装成一个 `v-xxx` 属性。下面只做两个例子：自动聚焦和修改文字颜色。

**全局注册：在 `main.js` 中注册 `v-focus`**

```js
Vue.directive('focus', {
  inserted(el) {
    el.focus()
  }
})
```

`inserted` 表示元素已经插入页面，这时可以调用 `focus()`。注册后，任何组件都可以写 `<input v-focus />`。

如果只在当前组件使用，也可以改成局部注册（与全局注册二选一）：

```js
export default {
  directives: {
    focus: {
      inserted(el) {
        el.focus()
      }
    }
  }
}
```

**全局注册 `v-color`：接收指令的值**

```js
Vue.directive('color', function (el, binding) {
  el.style.color = binding.value
})
```

这里的函数简写会同时用于指令初次绑定和后续更新。`binding.value` 就是使用指令时传入的颜色值；颜色变化后，文字颜色也会更新。

**在 `App.vue` 中使用两个指令**

```vue
<template>
  <div>
    <input v-focus placeholder="打开页面后自动聚焦" />

    <p v-color="textColor">这段文字会变色</p>
    <button @click="textColor = 'blue'">变成蓝色</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      textColor: 'red'
    }
  }
}
</script>
```

打开页面时，`v-focus` 让输入框聚焦，`v-color` 把文字设为红色；点击按钮后，`textColor` 变为 `blue`，指令再次执行，文字也跟着变蓝。

**课堂练习**

把 `textColor` 的初始值改为 `'green'`，再添加一个“变成红色”的按钮，将 `textColor` 改为 `'red'`。

**本节小结**：指令可以全局或局部注册；`v-focus` 在 `inserted` 中调用 `focus()`；`v-color` 通过 `binding.value` 接收颜色，函数简写可同时处理初次绑定和值更新。

