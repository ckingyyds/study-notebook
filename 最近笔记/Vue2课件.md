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

`v-for`**（列表渲染）**你把一个数组丢给它，它就会帮你把这个数组里的每一项，自动变成页面上的 HTML 标签。`v-for` **写在哪个元素上，就重复生成哪个元素及其内部内容**

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
**(@语法糖)就是v-on的简写**

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
    <button @click="btnClick2">按钮5（拿到 event,也就是点击事件）</button>

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

### 11.冒泡、阻止默认行为、其他事件

**本节目标**

- 会用 `.stop` 阻止冒泡、`.prevent` 阻止默认行为
- 掌握 `@keydown`、`@keyup`、`@change`、`@input` 四种绑定事件的触发时机

**讲解**
**绑定事件:**`@keydown`、`@keyup`、`@change`、`@input`,类似之前的点击@click

```vue
<template>
  <div>
    <h2>{{ msg }}</h2>
    
    <!-- 1. 阻止冒泡：点按钮不触发外层 div 的点击 -->
    <div class="box" @click="divClick">
      外层 div（点这里会打印 divClick）
      <!-- 这个加了.stop阻止冒泡,不触发外层 -->
      <button @click.stop="btnClick">阻止冒泡的按钮</button>
      <!-- 对比项,这个没有加.stop阻止冒泡,会触发外层 -->
      <button @click="divClick1">内部按钮</button>
    </div>

    <!-- 2. 阻止默认行为：表单不再刷新页面 -->
    <!--第一种绑定事件:提交-->
    <!--正常提交表单后页面会自动刷新,加.prevent阻止刷新-->
    <form @submit.prevent="onSubmit">
    <!--v-model将表单输入绑定给form变量-->
      账号 <input type="text" v-model="form.user" />
      密码 <input type="text" v-model="form.password" />
      <input type="submit" value="登录" />
    </form>

    <!-- 3. 键盘与输入事件 -->
    <!--第二种绑定事件:键盘按下回车,keydown是键盘按下,.enter是只管回车-->
    <input type="text" placeholder="按下回车触发" @keydown.enter="getDown" />
    <!--第三种绑定事件:输入-->
    <input type="text" placeholder="每次输入都触发" @input="getInput" />
    <!--第四种绑定事件:失焦或者回车-->
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
      console.log('外层divClick 被触发了')
    },
    divClick1() {
      console.log('内层divClick 被触发了')
    },
    btnClick() {
      console.log('btnClick：加了 .stop，没有冒泡到外层 div,外层没有触发')
    },
    onSubmit() {
      console.log('提交被 .prevent 拦住，页面没有刷新', this.form)
    },
    <!--.target:源头-->
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

| 修饰符     | 作用                                          |
| ---------- | --------------------------------------------- |
| `.stop`    | 阻止冒泡（等于 `event.stopPropagation()`）    |
| `.prevent` | 阻止默认行为（等于 `event.preventDefault()`） |

**易错点**：`.prevent=""` 这种写法（原始注释里就是）在 Vue2 里能跑，但没必要，写成 `@submit.prevent="onSubmit"` 更规范。

**课堂练习**：做一个搜索框，按回车才发起"搜索"（用 `console.log` 模拟），并说明为什么不用 `@input`。
```vue
<template>
  <div>
    <input type='text' @keydown.enter="findcontext" placeholder='请输入搜索内容,按回车搜索' style="width: 300px;">
  </div>
</template>

<script>
export default {
  data() {
    return {
      
    }
  },
  methods: {
    findcontext(e) {
      console.log('搜索内容：',e.target.value)
    }
  }
}
</script>

<style scoped lang="less">
</style>
```

**本节小结**：冒泡靠 `.stop`，默认行为靠 `.prevent`，键盘靠按键修饰符。

---

## 第四阶段｜计算属性与双向绑定

> 阶段目标：从"手动拼字符串"升级到"数据自动派生"，再让派生值能反写回去。这一阶段是学生从"会用模板"到"会抽象数据"的分水岭。

### 12. 计算属性computed的简单使用

**本节目标**

- 会写 `computed`
- 分清 `computed` 和 `methods` 的使用差异（带不带括号）

**讲解**

`{{ firstName }}-{{ lastName }}` 这种拼接，模板里写一两次还行，写十次就重复了。计算属性就是给"算出来的值"起个名字，当成 `data` 一样用。

```vue
<template>
  <div>
    <div>{{firstName}}-{{lastName}}</div>
     <!-- 计算属性：不加括号 -->
    <div>计算属性得到的：{{computedFullName}}</div>
    <!-- 方法：必须加括号 -->
    <div>方法得到的：{{methodFullName()}}</div>
  </div>
</template>

<script>
export default {
  data() {//数据
    return {
        firstName:"zhang",
        lastName:"san",
    }
  },
  methods: {//方法
    methodFullName(){
      return this.firstName+"-"+this.lastName
    }
  },
  computed: {//计算属性
    computedFullName(){
      return this.firstName+"-"+this.lastName
    }
  },
}
</script>
<style scoped lang="less">
</style>
```

**运行效果**：三行都是 `zhang-san`。区别在写法——计算属性当变量用，方法当函数用。

**一句话区分**：`data` 是**存**的，`computed` 是**算**的，`methods` 是**被调**的。
**为什么用计算属性:**
- **`methods`**：像个老实巴交的打工人，每次老板（页面）路过，他都要把活重新干一遍（没有缓存，耗性能）。
- **`computed`**：像台智能自动售货机，东西没卖完，你按按钮它直接掉出来（有缓存）；只有货空了（依赖数据变了），它才会去后台补货。

**必须强调**：计算属性里**不能写异步**。`setTimeout`、`axios`、`fetch` 都不行——它必须同步返回一个值。要异步，用 `watch` 或生命周期钩子。

**课堂练习**：用计算属性算出购物车总价（`list` 里若干商品，各含 `price`、`num`），要求模板里只写 `{{ total }}`。
```vue
<template>
  <div>
    <li v-for="(item,index) in list" :key="index">
        购物车列表：{{item.name}} 单价:{{item.price}} 数量:{{item.num}}
        <button @click="item.num++">+</button>
        <button @click="item.num>0?item.num--:null">-</button>
    </li>
    <p>购物车总价：{{ gettotal}}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list:[
        {name:"苹果",price:5,num:0},
        {name:"香蕉",price:3,num:0},
        {name:"橘子",price:4,num:0}
      ]
    }
  },
  methods: {
    
  },
  computed:{
    gettotal(){
        var total=0;
        for(var i=0;i<this.list.length;i++){
            total+=this.list[i].price*this.list[i].num;
        }
        return total;
    }
  }
}
</script>

<style scoped lang="less">
</style>
```

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


**既然开发时最常用简写,那什么时候要写完整形式呢**
当你希望“直接给计算属性赋值”时，就必须用完整形式。
```vue
<template>
  <div>
    <!-- 1. 基础数据展示 -->
    <div>基础数据：{{ firstName }} - {{ lastName }}</div>
    
    <!-- 2. 计算属性读取（触发 get） -->
    <div>计算属性得到的（get）：{{ fullName }}</div>

    <!-- 3. 方法展示 -->
    <div>方法得到的：{{ getFullName() }}</div>

    <hr>
    <!-- 4. 测试计算属性的赋值（触发 set） -->
    <button @click="changeName">修改 fullName 为 li-si</button>
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
    },
    // 点击按钮，给 计算方法fullName 重新赋值
    changeName() {
      this.fullName = 'li-si'; 
    }
  },
  computed: {
    // 完整写法：包含 get 和 set
    fullName: {
      // 当你去读取 fullName 的时候执行
      get() {
        console.log('触发了 get');
        return this.firstName + '-' + this.lastName;
      },
      // 当你手动给 fullName 赋值（this.fullName = 'xxx'）的时候执行
      set(newValue) {
        console.log('触发了 set，新值是：', newValue);
        // 把传进来的 'li-si' 用 '-' 劈开
        const parts = newValue.split('-');
        // 分别赋值给基础数据
        this.firstName = parts[0];
        this.lastName = parts[1];
      }
    }
  }
}
</script>
```

这就是"只有 getter 的计算属性是只读的"的实证。**让学生亲手触发一次报错**，比讲定义印象深。

**本节小结**：`fullName() {}` ≡ `{ get() {} }`；只有 get 就是只读。

---

### 14. v-model 的基本使用

**本节目标**

- 会用 `v-model` 做输入框双向绑定
- 理解 `v-model` 的本质是 `:value` + `@input`

**讲解**

v-model作用:
`v-model` 是 Vue 提供的**双向数据绑定指令**。
它实现了两个方向的同步：
- **数据 → 页面**：`msg` 的值是什么，输入框和 `{{msg}}` 就显示什么。
- **页面 → 数据**：你在输入框输入新内容，`msg` 的值也会自动更新。可以通过这个方法修改代码里的数据

前面的绑定都是单向的：数据 → 页面。表单要求反过来：用户输入 → 数据也要变。`v-model` 就是干这个的。
 
```vue
<template>
  <div>
    <!-- 1. 最基础：输入框和数据显示同步 -->
    <div>{{ msg }}</div>
    <input type="text" placeholder="请输入内容" v-model="msg" /><!--没有内容时显示请输入内容,否则显示绑定的内容msg内容-->

    <hr />

    <!-- 2. v-model 的本质(v-model被底层解析的结果就是下面这样)：:value 绑定 + @input 改数据 -->
    <!--和直接用v-model效果一样-->
    <input type="text" :value="msg" @input="msg = $event.target.value" />

    <hr />

    <!-- 3. 绑定多行文本 -->
    <textarea v-model="desc" placeholder="多行文本"></textarea>
    <p>desc：{{ desc }}</p>

    <!-- 4. 绑定复选框：单个是布尔，多个是数组 -->
<!--单个复选框false表示不勾选,true表示勾选,v-model绑定的是bool值-->
    <label><input type="checkbox" v-model="agree" /> 同意协议</label>
    <p>agree：{{ agree }}</p>
<!--多个复选框,v-model绑定的是数组Array-->
	<label>
      <input type="checkbox" v-model="fruits" value="苹果" />
      苹果
    </label>
    <label>
      <input type="checkbox" v-model="fruits" value="香蕉" />
      香蕉
    </label>
    <label>
      <input type="checkbox" v-model="fruits" value="橘子" />
      橘子
    </label>
    <p>选择的水果：{{ fruits }}</p>
    
    <!-- 5. 绑定下拉框 -->
    <select v-model="city">
      <option value="合肥">合肥</option>
      <option value="上海">上海</option>
      <option value="北京">北京</option>
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
      city: '请选择城市',
      fruits: []
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
```vue
<template>
  <div>
  <!--html本来就有一个属性是maxlength,用:绑定的话就可以被data里的变量控制了,限制输入字数数量-->
    <textarea v-model="content" :maxlength="max" placeholder='请输入内容' style="height: 150px;width: 250px;"></textarea>
    <p>已输入{{content.length}}字,还可输入{{ max - content.length }}字(上线{{max}}字)</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      max:200,
      content:''
    }
  },
  methods: {
    
  }
}
</script>

<style scoped lang="less">
</style>
```

**本节小结**：`v-model` = `:value` + `@input`，只对表单和组件有效。

---

### 15. 计算属性的完整写法（get + set）

**本节目标**

- 会给计算属性加 `set`
- 理解"读时走 get，写时走 set"

**讲解**

其实我们在第13节讲解get方法时就已经提到了这个问题,computed计算属性的完整写法,既可以get,也可以set

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
        console.log('set 收到的值：',newval)
          var names=newval.split("-")
          //分割后的数组
          console.log(names)
          this.firstName=names[0]
          this.lastName=names[1]
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
```vue
//修改计算属性即可
export default {
computed: {
    fullName:{
        get(){
            return this.firstName+" "+this.lastName
        },
        set(newval){ 
          console.log('set 收到的值：',newval)
          var names=newval.split(" ")
          if(names.length<2){
            console.log(names)
            this.firstName=names[0]
            this.lastName=""
          }else{
            console.log(names)
            this.firstName=names[0]
            this.lastName=names[1]
          }
        }
    }
  }
  }
```

**本节小结**：`get` 负责读，`set` 负责写；写的时候记得校验和兜底。

---

### 16. 计算属性和方法的区别（缓存）

**本节目标**

- 用实验证明计算属性有缓存
- 能根据场景选择 `computed` 还是 `methods`

**讲解**

这是全课最后一个知识点，也是面试常问的。不讲理论，直接看控制台：同一个结果，模板里写四次，方法执行四次，计算属性只执行一次,然后将结果缓存起来,其余的直接读取缓存。

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
// 可见计算属性有缓存，在属性不变的情况下，
// methods调用了四次，而计算属性才调用了一次，
// 性能上计算属性明显比methods好
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

| 场景                               | 选择                 |
| ---------------------------------- | -------------------- |
| 结果要多次引用、依赖不变           | `computed`           |
| 需要传参数                         | `methods`            |
| 要做副作用（请求、定时器、打日志） | `methods` 或 `watch` |

**课堂练习**：在页面上加一个 `<input v-model="firstName">`，观察控制台——改一次输入，方法执行 4 次，计算属性执行 1 次。

**本节小结**：计算属性算一次存着，方法调一次算一次。

---

## 第五阶段｜v-model 进阶与组件化开发

> 阶段目标：把"一个页面写到底"改成"一个页面由一堆零件拼起来"。这一阶段结束后，学生应该能独立拆组件、定接口（props / 事件）、并且知道页面在什么时刻该去要数据。

### 17. v-model 的进阶用法与修饰符

**本节目标**

- 会使用 `.lazy`、`.number`、`.trim` 三个修饰符修饰v-model
- 会把一组复选框选中的值保存到数组中

**讲解**

修饰符写在 `v-model` 后面，用来调整数据同步的方式。

| 修饰符       | 作用                 |
| --------- | ------------------ |
| `.lazy`   | 输入框失焦或回车后再同步数据     |
| `.number` | 将用户输入的内容尽可能转换为数字类型 |
| `.trim`   | 自动去掉输入值首尾的空格       |

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
    <!--label就是表单元素的说明标签，点击它的文字也能操作对应的表单控件,v-for 应该写在你希望重复生成的元素上-->
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
//给组件定义一个名称，方便调试和识别。
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
     <HelloMessage/><!-- 第三步：使用组件 -->
     <!-- <HelloMessage></HelloMessage> -->
  </div>
</template>

<script>
//第一步：导入组件
import HelloMessage from './components/HelloMessage1.vue';
export default {
  data() {
    return {
      
    }
  },
  methods: {
    
  },
  components: {//第二步：注册组件
    HelloMessage
  },
}

</script>
<style scoped>
</style>
```

局部注册分三步：先用 `import` 引入，再写进 `components`，最后在模板中当成标签使用。


全局注册也能让组件在所有页面使用，但本节不展开，日常练习或者开发优先使用局部注册。

**全局组件注册**
在main.js中导入
```js
//全局注册组件 在main.js中  对应app文件夹中App 20.vue文件
// 第一步：引入组件，
// 第二步：用Vue.component()方法注册组件
// 第三步 到页面中直接调用组件
//两种导入方法
// Vue.component('HelloMessage',()=>import('./components/HelloMessage.vue'))
// Vue.component('HelloMessage',HelloMessage)

import Vue from 'vue'
import App from './App.vue'
import './assets/reset.css'
import router from './router'
// 第一步：引入组件，
import HelloMessage from './components/HelloMessage1.vue'
Vue.config.productionTip = false//生产环境的提示
// 第二步：用Vue.component()方法注册组件
Vue.component('hello-message',HelloMessage)
//事件总线 prototype 原型   第一步:
Vue.prototype.$bus = new Vue()


new Vue({
  router,
  render: h => h(App),
}).$mount('#app')

```
App.vue中
```vue
<template>
  <div>
    <!-- <HelloMessage></HelloMessage> -->
    <!--第三步 到页面中直接调用组件-->
     <hello-message></hello-message>
  </div>
</template>

<script>

</script>
<style scoped>
</style>
```
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
    <p>爱好：{{ books }}</p>
    <p>对象：{{ obj }}</p>
  </div>
</template>

<script>
export default {
  name: 'UserCard',
  data() {
    return {
       
    }
  },
//   数组式props,只声明名字,不做任何校验:
//   props:['name','age','books','obj']

//对象式props,每个 prop 可以配置校验规则,开发时用对象式
props:{//props 是只读的 不能直接修改
    name:{
        type:String,
//required: true 表示这个 Prop（组件属性）是必传的。
        required:true,//必填
    },
    age:{
//小细节:required: true 和 default 不该同时写
        type:Number,
        default:18
    },
    books:{
        type:Array,
//对象或者数组你要写默认值，写的必须是一个函数,你可以不写默认值
        default(){
            return []
        }
    },
    obj:{
        type:Object,
        default(){
            return {}
        }
    }
}
 
}
</script>
```

**父组件 `App.vue`**
负责准备数据并把数据"下发"给上一轮那个 `UserCard` 子组件。两者合起来就是一套完整的 props 传递链路。

```vue
<template>
  <div>
      <!-- <UserCard :name="name" :age="age"></UserCard>
      <UserCard name="小刚"></UserCard>传静态值不需要前面加冒号
      <UserCard :books="books"></UserCard> -->
       <!-- 第三步：使用组件 -->
        <!-- 绑定顺序无所谓,Vue 不关心 -->
      <UserCard :obj="obj" :books="books" :name="name" :age="age"></UserCard>
 </div>
</template>

<script>
//第一步：导入组件
import UserCard from './components/UserCard2.vue'
export default {
  data() {
    return {
       name:'王五',
       age:18,
        books: [
        {
          name: "《算法导论》",
          beginDate: "2006-9",
          price: 85.00,
          count: 1,
          status: false
        },
        {
          name: "《UNIX编程艺术》",
          beginDate: "2006-2",
          price: 59.00,
          count: 1,
          status: true
        },
        {
          name: "《编程大全》",
          beginDate: "2008-10",
          price: 39.00,
          count: 1,
          status: false
        },
        {
          name: "《代码大全》",
          beginDate: "2006-3",
          price: 128.00,
          count: 1,
          status: true
        },
      ],
      obj:{
        name:'小王',
        age:23
      }
    }
  },
  methods: {
    
  },
  //第二步：注册组件
  components: {
    UserCard
  },
}

</script>
<style scoped lang="less">
</style>
```

第一张卡片显示“小明、20 岁”。第二张卡片没有传 `age`，所以显示默认值 `18`。传字符串可以直接写 `name="小红"`；传变量或数字时要加 `:`，例如 `:age="user.age"`。

**props 是只读的**：子组件可以显示 props，但不要直接修改它，例如不要写 `this.name = '别的名字'`。数据属于父组件，需要修改时应由父组件处理。


**⭐ 为什么对象/数组的 default 必须是函数?(高频面试题)**

因为**组件会被复用很多次**。

假设允许你写 `default: []`,那么这个空数组在组件定义时就**只创建了一份**,所有 `<UserCard>` 实例共享**同一个数组引用**:
写成函数(叫**工厂函数**),Vue 每次创建实例都会**调用一次这个函数**,拿到一个全新的数组/对象:
```js
default() { return [] }   // A 调一次得到 [],B 调一次又得到一个新的 []
```



而 `Number`、`String`、`Boolean` 是**基本类型**,存的是值不是引用,`18` 复制多少份都互不影响,所以可以直接写。


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
    <!-- 绑定子组件自己的方法 -->
    <button @click="remove()">删除</button>
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
      //emit：派发   两个参数：方法名 你需要带的数据
      this.$emit('removeItem', this.todo)
//翻译成人话:"我(子组件)身上发生了个事,事件名叫 removeItem,这是相关数据,谁爱管谁管。"
    }
  }
}
</script>
```

`$emit` 的第一个参数是事件名 `removeItem`，第二个参数是要交给父组件的 id。

**父组件 `App.vue`**

```vue
<template>
  <div>
    <h3>我的待办</h3>
    <ul>
<!-- 第三步：使用组件 -->
 <!-- 子组件传递的方法叫removeItem,绑定父组件自己的方法叫remove111 -->
      <TodoItem
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        @removeItem="remove111"
      />
    </ul>
  </div>
</template>

<script>
//第一步：导入组件
import TodoItem from './components/TodoItem3.vue'

export default {
  //第二步：注册组件
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
    remove111(todo) {
      //生成一个新数组,里面只保留"id 不等于要删的那个"的项
      this.todos = this.todos.filter(item => item.id !== todo.id)
    }
  }
}
</script>
//父传子：子组件用props来接收数据
//子传父：用$emit来派发一个方法以及一些参数给父组件，父组件监听这个从子组件派发来的方法，并执行相应的方法。
```

点击“删除”后，子组件只负责发出通知，真正删除数据的是父组件的 `remove111`。

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

| `ref` 写在哪里                                   | `this.$refs.名字` 获取到什么 | 可以做什么                    |
| ------------------------------------------------ | ---------------------------- | ----------------------------- |
| 普通标签，如 `<input ref="keyword">`             | DOM 元素                     | 调用 `focus()` 等 DOM 方法    |
| 组件标签，如 `<ChildCounter ref="childCounter">` | 子组件实例                   | 调用子组件的 `reset()` 等方法 |

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

| 顺序 | 钩子              | 触发时机                                             | 此时能不能拿到 DOM | 常见用途                             |
| ---- | ----------------- | ---------------------------------------------------- | ------------------ | ------------------------------------ |
| 1    | `beforeCreate`    | 实例刚创建，`data`、`methods` **都还没初始化**       | 不能               | 极少用（插件初始化）                 |
| 2    | `created`（重要） | `data`、`methods`、`computed` 已可用，**DOM 还没有** | 不能               | **向后端发初始请求、赋值**           |
| 3    | `beforeMount`     | 模板编译完成，即将挂载到页面                         | 不能（还没替换）   | 极少用                               |
| 4    | `mounted`（重要） | 已挂载到页面，**DOM 可访问**                         | 能                 | **操作 DOM、初始化图表、拿 `$refs`** |
| 5    | `beforeUpdate`    | 数据变了，**页面还没重新渲染**                       | 能（旧 DOM）       | 更新前记录滚动位置                   |
| 6    | `updated`         | 数据和页面都已是新的                                 | 能（新 DOM）       | 数据变化后操作 DOM（少用，易死循环） |
| 7    | `beforeDestroy`   | 实例即将销毁，**此时还有 `this`**                    | 能                 | **清定时器、解绑 `$bus`、取消请求**  |
| 8    | `destroyed`       | 实例已销毁，`this` 上的东西基本没了                  | 不能               | 极少用                               |

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

| 钩子          | 触发时机                                    |
| ------------- | ------------------------------------------- |
| `activated`   | 组件被激活（第一次进入 + 每次从缓存切回来） |
| `deactivated` | 组件被缓存起来（切走时）                    |

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

## 第六阶段｜网络请求与路由（重要）

> 阶段目标：从"本地假数据"跨到"真后端"，再跨到"多页面"。这一阶段结束，学生应该能独立接一个真实接口，并做出带导航、带参数、带登录拦截的多页面应用。

### 26. axios 的基本使用

**本节目标**

- 会安装 axios，并用 `async / await` 发送 GET 请求
- 会显示最基本的加载中和请求失败提示
- 会发送一个简单的 POST 请求

**讲解**

Vue 本身不提供网络请求功能，先安装 axios：

```bash
pnpm install axios
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

- 理解什么是同源、什么是跨域
- 会使用本项目的 `devServer.proxy` 配置开发环境代理
- 会使用 `axios.create()`、拦截器和 `request()` 封装公共请求
- 会通过 `src/api/index.js` 调用接口，并在组件中显示数据

**一、什么是跨域**

浏览器判断是否同源，需要同时比较三个部分：**协议、域名、端口**。三者完全相同才是同源，只要有一个不同，请求就属于跨域。

例如，本项目开发时可能使用下面两个地址：

| 内容         | 地址                    |
| ------------ | ----------------------- |
| Vue 前端     | `http://localhost:8080` |
| FastAPI 后端 | `http://127.0.0.1:8000` |

它们的域名和端口都不同。如果前端直接使用 Axios 请求 `http://127.0.0.1:8000/api/homedata`，浏览器就会按照跨域请求处理。

**跨域是浏览器的安全限制，不是 Vue 或 Axios 出现了故障。** 后端可能已经收到并处理了请求，但浏览器会阻止前端读取响应。常见解决方式有两种：

1. 后端配置 CORS，明确允许前端来源访问。
2. 开发环境使用代理，让浏览器先请求同源的 Vue 开发服务器，再由开发服务器转发给后端。

本项目使用第二种方式。请求过程如下：

```text
浏览器请求 /api/homedata
        ↓
Vue 开发服务器接收请求
        ↓
转发到 http://127.0.0.1:8000/api/homedata
```

这样浏览器访问的是同源地址，后端接口由开发服务器请求，因此开发环境中不会出现浏览器跨域限制。

**二、配置本项目代理：`vue.config.js`**

在项目根目录配置代理：

```js
const { defineConfig } = require('@vue/cli-service')

module.exports = defineConfig({
  transpileDependencies: true,
  lintOnSave: false,
  devServer: {
    proxy: {
      '/api': {
        target: 'http://127.0.0.1:8000',
        changeOrigin: true
      }
    }
  }
})
```

各配置的含义：

- `'/api'`：匹配以 `/api` 开头的请求。
- `target`：请求真正要转发到的后端地址。
- `changeOrigin: true`：修改代理请求的源信息，是代理服务器的常用配置。

假设接口文件中的地址是 `/homedata`，Axios 的 `baseURL` 是 `/api`，最终请求地址就是 `/api/homedata`，代理会把它转发为：

```text
http://127.0.0.1:8000/api/homedata
```

**本项目不要删除 `/api` 前缀**，因为 FastAPI 注册的路由本身就是 `@app.post("/api/homedata")`。如果添加下面的配置：

```js
pathRewrite: {
  '^/api': ''
}
```

请求就会被转发为 `http://127.0.0.1:8000/homedata`，与后端路由不一致，最终返回 404。只有后端路由本身不带 `/api` 时，才需要这样重写路径。

修改 `vue.config.js` 后，需要重新启动前端开发服务器。

**三、封装公共请求：`src/utils/axios.js`**

每个组件都写完整地址、超时时间和响应处理，会产生大量重复代码。本项目把公共逻辑放进 `src/utils/axios.js`。

**1. 创建 Axios 实例**

```js
import axios from 'axios'

const service = axios.create({
  baseURL: '/api',
  timeout: 6 * 1000
})
```

`baseURL` 会自动拼接到接口地址前，`timeout` 表示请求超过 6 秒仍未完成就判定为超时。

**2. 添加请求拦截器**

请求拦截器可以在每次发送请求前统一处理配置。本项目从 `sessionStorage` 中读取 token，并放入请求头：

```js
service.interceptors.request.use(config => {
  const data = window.sessionStorage.getItem('userinfo') || '{}'
  const { token = '' } = JSON.parse(data)

  if (!config.headers.Authorization) {
    config.headers.Authorization = 'Bearer ' + token
  }
  return config
}, error => {
  return Promise.reject(error)
})
```

这样就不用在每个接口中重复编写 `Authorization`。

**3. 添加响应拦截器**

本项目后端统一返回 `data`、`msg`、`statusCode`：

```js
{
  data: {
    name: '王五',
    age: 28,
    title: '大模型工程师',
    sex: '男'
  },
  msg: '请求成功',
  statusCode: 200
}
```

响应拦截器在请求成功时取出内层 `data`：

```js
service.interceptors.response.use(res => {
  if (!res.data.statusCode) {
    return res.data
  }

  const { data, statusCode } = res.data
  if (statusCode === 200) {
    return data
  }
}, error => {
  return Promise.reject(error)
})
```

这里要分清三个层级：

- `res`：Axios 的完整响应对象。
- `res.data`：后端返回的完整数据，包含 `data`、`msg`、`statusCode`。
- 拦截器返回的 `data`：真正的业务数据，组件最终拿到的就是它。

**4. 封装统一的 `request()` 函数**

```js
function request(options) {
  options.method = options.method || 'get'

  if (options.method.toLowerCase() === 'get') {
    options.params = options.data
  }

  return service(options)
}

export default request
```

这个函数统一接收请求配置。按照本项目的约定，GET 请求会把 `data` 转成 `params`，拼接到 URL 后面；POST 请求的 `data` 仍然作为请求体发送。

| 请求方式 | 接口文件写法                       | 参数最终位置 |
| -------- | ---------------------------------- | ------------ |
| GET      | `{ method: 'GET', data: params }`  | URL 查询参数 |
| POST     | `{ method: 'POST', data: params }` | JSON 请求体  |

注意：**`params` 和 `data` 的区别是参数位置不同，不是简单的 GET 与 POST 的区别。** POST 也可以发送查询参数，具体使用哪一种必须与后端接口保持一致。本项目当前的 `/api/homedata` 使用 POST，并通过 Pydantic 模型接收 JSON，所以前端使用 `data` 传递参数。

**四、封装具体接口：`src/api/index.js`**

公共请求封装完成后，在 `src/api/index.js` 中集中管理具体接口。下面保留本节需要的 `getHomeList`：

```js
import request from '../utils/axios'

export default {
  getHomeList: params => request({
    url: '/homedata',
    method: 'POST',
    data: params
  })
}
```

组件不需要关心 `baseURL`、token 和响应结构，只需要调用 `getHomeList()` 并传入参数。

**五、在 `App.vue` 中调用接口**

```vue
<template>
  <div>
    <div>姓名：{{ person.name }}</div>
    <div>年龄：{{ person.age }}</div>
  </div>
</template>

<script>
import API from './api'

export default {
  data() {
    return {
      person: {
        name: '',
        age: ''
      }
    }
  },
  methods: {
    getList() {
      const title = '大模型工程师'
      const sex = '男'

      API.getHomeList({ title, sex }).then(res => {
        console.log('res=>', res)
        this.person.name = res.name
        this.person.age = res.age
      })
    }
  },
  created() {
    this.getList()
  }
}
</script>
```

因为响应拦截器已经返回了内层 `data`，所以这里直接使用 `res.name` 和 `res.age`，不能再写成 `res.data.data.name`。

**课堂练习**

1. 修改 `title` 和 `sex`，在浏览器 Network 面板中确认请求地址为 `/api/homedata`，并查看 JSON 请求体。
2. 将接口返回的姓名、年龄、职位和性别全部显示在页面上。
3. 临时添加删除 `/api` 的 `pathRewrite`，观察 404，再说明产生错误的原因，最后恢复配置。

**本节小结**：协议、域名或端口不同就会产生跨域；本项目通过 `/api` 代理转发到 FastAPI，且不能重写掉后端已有的 `/api` 前缀；`utils/axios.js` 负责公共配置和拦截器，`api/index.js` 负责具体接口，组件只负责传参和使用返回数据。

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
pnpm install vue-router@3
```

Vue2 使用 vue-router 3.x。先准备两个简单页面：

```vue
<!-- src/views/HomeView.vue -->
<template>
  <h2>首页</h2>
</template>
```

```vue
<!-- src/views/AboutView.vue -->
<template>
  <h2>关于我们</h2>
</template>
```

**第一步：配置路由表 `src/router/index.js`**

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import HomeView from '@/views/HomeView.vue'
import AboutView from '@/views/AboutView.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    component: About
  }
]

const router = new VueRouter({
  routes
})

export default router
```

`name` 表示路由名称，不是必填项，但每个名称必须唯一。配置名称后，除了通过路径跳转，还可以通过名称跳转：

```vue
<router-link :to="{ name: 'About' }">关于</router-link>
```

```js
this.$router.push({ name: 'About' })
```

如果只用 `to="/about"` 或 `this.$router.push('/about')` 按路径跳转，不写 `name` 也可以。实际项目中建议添加名称，以后修改 `path` 时，通过 `name` 跳转的代码不需要跟着修改。

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

| 对象      | 作用               | 例子                                       |
| --------- | ------------------ | ------------------------------------------ |
| `$route`  | 读取当前路由的信息 | `$route.params.id`、`$route.query.keyword` |
| `$router` | 调用路由的跳转方法 | `$router.push('/about')`                   |

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

- 会用 `children` 配置嵌套路由，用 `redirect` 设置默认页面
- 分清全局守卫、路由独享守卫、组件内守卫
- 掌握 Vue Router 3 的 7 个导航守卫

Vue Router 3 常用的守卫如下：

| 分类     | 守卫                | 触发时机                                     |
| -------- | ------------------- | -------------------------------------------- |
| 全局     | `beforeEach`        | 每次导航开始前                               |
| 全局     | `beforeResolve`     | 组件内守卫和异步组件解析后、导航确认前       |
| 全局     | `afterEach`         | 导航完成后，没有 `next`                      |
| 路由独享 | `beforeEnter`       | 进入指定路由时                               |
| 组件内   | `beforeRouteEnter`  | 进入组件前，此时不能通过 `this` 获取组件实例 |
| 组件内   | `beforeRouteUpdate` | 当前组件复用、路由参数变化时                 |
| 组件内   | `beforeRouteLeave`  | 离开当前组件时                               |

下面用一个完整案例演示所有守卫。`main.js` 沿用第 28 节，确保已经挂载 `router`。

**1. 路由与全局守卫：`src/router/index.js`**

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Login from '@/views/Login.vue'
import HomeView from '@/views/HomeView.vue'
import HomeDemo from '@/views/HomeDemo.vue'
import UserDemo from '@/views/UserDemo.vue'
import MovieDetail from '@/views/MovieDetail.vue'
import SearchView from '@/views/SearchView.vue'

Vue.use(VueRouter)

const routes = [
  { path: '/login', component: Login },
  {
    path: '/',
    component: HomeView,
    redirect: '/homedemo',
    children: [
      {
        path: 'homedemo',
        component: HomeDemo,
        meta: { requiresAuth: true }
      },
      {
        path: 'userdemo',
        component: UserDemo,
        meta: { requiresAuth: true },
        // 路由独享守卫
        beforeEnter(to, from, next) {
          console.log('beforeEnter：进入用户路由')
          next()
        }
      }
    ]
  },
  { path: '/movie/:id', name: 'MovieDetail', component: MovieDetail },
  { path: '/search', component: SearchView }
]

const router = new VueRouter({ routes })

// 全局前置守卫：检查登录
router.beforeEach((to, from, next) => {
 console.log('全局前置守卫',to, from)
    next()
})

// 全局解析守卫
router.beforeResolve((to, from, next) => {
  console.log('beforeResolve：导航即将确认')
  next()
})

// 全局后置钩子：没有 next 参数
router.afterEach((to, from) => {
  console.log('afterEach：导航完成', to.fullPath)
})

export default router
```

子路由的 `path` 不加 `/`，所以 `homedemo` 和 `userdemo` 分别对应 `/homedemo`、`/userdemo`。访问 `/` 时会重定向到 `/homedemo`。`to.matched` 包含当前路由及其父路由，因此用 `some()` 检查 `meta` 更适合嵌套路由。

**2. 父组件：`src/views/HomeView.vue`**

```vue
<template>
  <div>
    <h2>首页</h2>

    <button @click="goMovie">查看 1 号电影</button>
    <button @click="goSearch">搜索 Vue</button>

    <router-view />
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

`HomeView` 中的 `<router-view />` 用来显示子组件。

**3. 一级路由出口与导航：`src/App.vue`**

```vue
<template>
  <div>
    <nav>
      <router-link to="/homedemo">首页</router-link> |
      <router-link to="/userdemo">用户页</router-link>
    </nav>
    <hr />
    <router-view />
  </div>
</template>
```

`App.vue` 的 `<router-view />` 显示 `HomeView`，`HomeView` 的 `<router-view />` 再显示对应的子组件。

**4. 首页组件：`src/views/HomeDemo.vue`**

```vue
<template><h2>后台首页</h2></template>
```

**5. 用户组件：`src/views/UserDemo.vue`**

```vue
<template><h2>用户页面</h2></template>
```

**6. 电影详情与组件内守卫：`src/views/MovieDetail.vue`**

```vue
<template>
  <div>
    <h2>电影详情：{{ $route.params.id }}</h2>
    <router-link :to="{ name: 'MovieDetail', params: { id: 1 } }">电影 1</router-link> |
    <router-link :to="{ name: 'MovieDetail', params: { id: 2 } }">电影 2</router-link>
    <button @click="$router.push('/homedemo')">返回首页</button>
  </div>
</template>

<script>
export default {
  // 组件创建前触发，此时不能使用 this
  beforeRouteEnter(to, from, next) {
    console.log('beforeRouteEnter：进入电影组件前')
    next(vm => {
      console.log('组件实例已创建：', vm.$route.fullPath)
    })
  },

  // 从 /movie/1 切到 /movie/2，组件被复用时触发
  beforeRouteUpdate(to, from, next) {
    console.log('beforeRouteUpdate：id 变为', to.params.id)
    next()
  },

  // 离开电影组件时触发，可以通过 this 使用组件实例
  beforeRouteLeave(to, from, next) {
    console.log('beforeRouteLeave：离开电影组件')
    next()
  }
}
</script>
```

**7. 搜索组件：`src/views/SearchView.vue`**

```vue
<template><h2>搜索关键词：{{ $route.query.keyword }}</h2></template>
```

**8. 登录组件：`src/views/Login.vue`**

```vue
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
      this.$router.push('/homedemo')
    }
  }
}
</script>
```

**使用规则**

- 带 `next` 的守卫必须调用 `next()`，否则导航会停住；`next('/login')` 表示重定向。
- `afterEach` 没有 `next`，适合记录日志或修改页面标题。
- `beforeRouteUpdate` 只在组件复用时触发，例如 `/movie/1` 切到 `/movie/2`，普通页面切换不会触发。
- 一次导航可能涉及的顺序为：`beforeRouteLeave` → `beforeEach` → `beforeRouteUpdate` → `beforeEnter` → `beforeRouteEnter` → `beforeResolve` → `afterEach`，实际只执行符合当前场景的守卫。

**课堂练习**

先执行 `localStorage.removeItem('token')`，访问 `/userdemo`，确认被拦截到登录页；登录后切换“首页”和“用户页”，再进入电影详情并切换电影 1、电影 2，在控制台观察各守卫的触发顺序。

**本节小结**：全局守卫处理所有导航，路由独享守卫处理指定路由，组件内守卫处理组件进入、复用和离开；登录拦截通常使用 `beforeEach` 配合 `meta`。

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

| 部分        | 作用                | 组件中怎么触发或读取 |
| ----------- | ------------------- | -------------------- |
| `state`     | 保存数据            | `$store.state.xxx`   |
| `getters`   | 根据 state 计算数据 | `$store.getters.xxx` |
| `mutations` | 同步修改 state      | `$store.commit()`    |
| `actions`   | 处理异步操作        | `$store.dispatch()`  |
| `modules`   | 把数据拆成多个模块  | 使用时加模块名       |

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
    name: '张三',
    age: 25,
    sex: '男'
  },

  // getters：相当于全局 computed
  getters: {
    userInfo(state) {
      return `${state.name}：${state.age} 岁，${state.sex}`
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
    <p>姓名：{{ $store.state.name }}</p>
    <p>年龄：{{ $store.state.age }}</p>
    <p>性别：{{ $store.state.sex }}</p>
    <p>个人信息：{{ $store.getters.userInfo }}</p>
  </div>
</template>
```

也可以用 `mapState` 和 `mapGetters`，把它们映射成计算属性：

```vue
<template>
  <div>
    <p>姓名：{{ name }}</p>
    <p>年龄：{{ age }}</p>
    <p>性别：{{ sex }}</p>
    <p>个人信息：{{ userInfo }}</p>
  </div>
</template>

<script>
import { mapState, mapGetters } from 'vuex'

export default {
  computed: {
    ...mapState(['name', 'age', 'sex']),
    ...mapGetters(['userInfo'])
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
    name: '张三',
    age: 25,
    sex: '男'
  },
  getters: {
    userInfo: state => `${state.name}：${state.age} 岁，${state.sex}`
  },
  mutations: {
    // 第一个参数是 state，第二个参数是调用时传入的数据
    SET_NAME(state, name) {
      state.name = name
    },
    ADD_AGE(state, step = 1) {
      state.age += step
    },
    SET_SEX(state, sex) {
      state.sex = sex
    }
  }
})
```

组件中使用 `commit('mutation 名称', 参数)` 调用：

```vue
<template>
  <div>
    <p>姓名：{{ $store.state.name }}</p>
    <p>年龄：{{ $store.state.age }}</p>
    <p>性别：{{ $store.state.sex }}</p>
    <button @click="changeName">改为李四</button>
    <button @click="addAge">年龄加 1</button>
    <button @click="changeSex">切换性别</button>
  </div>
</template>

<script>
export default {
  methods: {
    changeName() {
      this.$store.commit('SET_NAME', '李四')
    },
    addAge() {
      this.$store.commit('ADD_AGE', 1)
    },
    changeSex() {
      const sex = this.$store.state.sex === '男' ? '女' : '男'
      this.$store.commit('SET_SEX', sex)
    }
  }
}
</script>
```

**2. actions：处理异步操作**

异步任务放在 `actions` 中，任务完成后再通过 `commit` 调用 mutation：

```js
actions: {
  changeNameAsync({ commit }, name) {
    setTimeout(() => {
      commit('SET_NAME', name)
    }, 1000)
  }
}
```

组件中使用 `dispatch('action 名称', 参数)` 调用：

```js
methods: {
  changeNameLater() {
    this.$store.dispatch('changeNameAsync', '王五')
  }
}
```

**3. 使用辅助函数简化调用**

```js
import { mapMutations, mapActions } from 'vuex'

export default {
  methods: {
    ...mapMutations(['SET_NAME', 'ADD_AGE', 'SET_SEX']),
    ...mapActions(['changeNameAsync']),

    handleClick() {
      this.SET_NAME('李四')
      this.ADD_AGE(1)
      this.SET_SEX('女')
      this.changeNameAsync('王五')
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
    name: '张三',
    age: 25,
    sex: '男'
  },

  getters: {
    userInfo(state) {
      return `${state.name}：${state.age} 岁，${state.sex}`
    }
  },

  mutations: {
    SET_NAME(state, name) {
      state.name = name
    },
    ADD_AGE(state, step = 1) {
      state.age += step
    },
    SET_SEX(state, sex) {
      state.sex = sex
    }
  },

  actions: {
    changeNameAsync({ commit }, name) {
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
    <p>姓名：{{ name }}</p>
    <p>年龄：{{ age }}</p>
    <p>性别：{{ sex }}</p>
    <p>个人信息：{{ userInfo }}</p>
    <button @click="SET_NAME('李四')">修改姓名</button>
    <button @click="ADD_AGE(1)">年龄加 1</button>
    <button @click="SET_SEX(sex === '男' ? '女' : '男')">切换性别</button>
    <button @click="changeNameAsync('王五')">一秒后改为王五</button>
  </div>
</template>

<script>
import { mapState, mapGetters, mapMutations, mapActions } from 'vuex'

export default {
  computed: {
    // 第一个参数是模块名
    ...mapState('user', ['name', 'age', 'sex']),
    ...mapGetters('user', ['userInfo'])
  },
  methods: {
    ...mapMutations('user', ['SET_NAME', 'ADD_AGE', 'SET_SEX']),
    ...mapActions('user', ['changeNameAsync'])
  }
}
</script>
```

不使用辅助函数时，写法如下：

```js
// state：state 后面加模块名
this.$store.state.user.name
this.$store.state.user.age
this.$store.state.user.sex

// getters、mutations、actions：名称前加 user/
this.$store.getters['user/userInfo']
this.$store.commit('user/SET_NAME', '李四')
this.$store.commit('user/ADD_AGE', 1)
this.$store.commit('user/SET_SEX', '女')
this.$store.dispatch('user/changeNameAsync', '王五')
```

**课堂练习**

给 `user` 模块添加 `RESET_USER` mutation，将姓名、年龄、性别恢复为“张三、25、男”，并在页面中添加“重置信息”按钮。

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

