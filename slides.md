---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: './bg-img.jpg'
title: '技术沙龙'
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
---

# 浅谈JavaScript数据类型


<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    开始 <carbon:arrow-right class="inline"/>
  </span>
</div>

---


# JavaScript中的数据类型 

<div class="float-left">
<v-click>

### 基本数据类型

</v-click>

<v-click>

- Number(数字类型)
- String(字符串类型)
- Boolean(布尔类型)
- Undefined(Undefined类型)
- Null(Null类型)
  
</v-click>

<v-click>

es6新增：
- Symbol(符号类型)：唯一且不可修改的值
- BigInt(BigInt类型)：用来表示任意精度的整数

</v-click>

</div>

<div class="float-right mr-20">
<v-click>

### 引用数据类型

</v-click>

<v-click>

- Object(对象类型)
- Function(函数类型)
- Array(数组类型)
  
</v-click>
</div>

---

# JavaScript数据存储方式

<v-click>

```md
原始数据类型直接存储在栈（stack）中的简单数据段，占据空间小、大小固定

属于被频繁使用数据，所以放入栈中存储
```
</v-click>

<v-click>

```md
引用数据类型存储在堆（heap）中的对象，占据空间大、大小不固定

如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针

该指针指向堆中该实体的起始地址

当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体
```

</v-click>

---

# 怎么判断数据类型

<v-click>

- #### typeof 运算符:返回一个字符串，表示未经运算的操作数的类型

</v-click>

<v-click>

##### 基本数据类型：

```ts {1|2|3|4|5,4|6|7,6|8|all}
- var a1 = 1                  typeof a1 --> 'number'
- var a2 = true               typeof a2 --> 'boolean'
- var a3 = undefined          typeof a3 --> 'undefined'
- var a4 = 'ticknet'          typeof a5 --> 'string'
- var a5 = 'ticknet'          a4 === a5 -->  true
- var a6 = Symbol('ticknet')  typeof a6 --> 'symbol'
- var a7 = Symbol('ticknet')  a6 === a7 -->  false
- var a8 = 42n                typeof a8 --> 'bigint' 
```

</v-click>

<v-click>


```ts{1,2|1,3|1,4|all}
特殊
- typeof(typeof(a7))  --> 'string'
- typeof NAN          --> 'number'
- typeof null         --> 'object'

解释：在 JavaScript 最初的实现中，JavaScript 中的值是由一个表示类型的标签和实际数据值表示的。
对象的类型标签是 0。由于 null 代表的是空指针（大多数平台下值为 0x00）
因此，null 的类型标签是 0，typeof null 也因此返回 "object"

曾有一个 ECMAScript 的修复提案（通过选择性加入的方式），但被拒绝了。该提案会导致 typeof null === 'null'。

```

</v-click>

---

#### 引用数据类型

<v-click>


``` js
// 对象
typeof {a: 1} === 'object'

// 函数
typeof function() {} === 'function'

// 使用 Array.isArray() 或者 Object.prototype.toString.call()
// 区分数组和普通对象
typeof [1, 2, 4] === 'object'

//内置对象
typeof new Date() === 'object'
typeof /regex/ === 'object'

// 下面的例子令人迷惑，非常危险，没有用处。避免使用它们。
typeof new Boolean(true) === 'object'
typeof new Number(1) === 'object'
typeof new String('abc') === 'object'


```

</v-click>

---

# 构造函数，原型，原型链

<v-click>

##### 构造函数：是一种特殊的函数，主要用来初始化对象，即为对象成员变量赋初始值，它总与new操作符一起使用。我们可以把对象中一些公共的属性和方法抽取出来，然后封装到这个函数里面
</v-click>

<v-click>

##### new实例化过程：

</v-click>

<v-clicks>

 - 在内存中创建一个新的空对象
 - 将构造函数的作用域赋给这个对象(即让this就指向了这个对象)
 - 执行构造函数中的代码(为这个对象添加属性和方法，以及执行构造函数中其他的代码)
 - 将这个新对象返回(因此构造函数里面不需要return)
  
</v-clicks>

<v-click>

```md
返回的是一个对象，return语句之前的定义都会被覆盖
如果返回的是一个非对象，那么return语句之前的定义不会被覆盖
```



</v-click>

---
layout:
class: w-30vw h-60vh flex
---


```js{1,2,3,4,5,6,7,8,9,10|11|all}
// 创建构造函数
function Person(name, nation) {
            this.name = name
            this.nation = nation
            this.introduce = function(nation) {
                console.log(`${this.name}的国籍是${this.nation}`)
            }
        }
var Dad = new Person("大明", "中国")
var Son = new Person("小明", "中国")
Dad.introduce === Son.introduce --> false 比较的是地址值

```

<v-click>

<img src="Page1.png" class="w-70vw h-50vh float-right" />
</v-click>

<v-click>
<div class="fixed mt-100 text-sm ml-2">构造函数方法虽然很好用但是会存在浪费内存的问题</div>
</v-click>


---

# 构造函数原型 prototype

<v-click>

```md
构造函数通过原型分配的函数是所有对象所共享的

JavaScript规定，每一个构造函数都有一个prototype属性，指向另一个对象。

这个prototype就是一个对象，称为原型对象，原型对象的所有属性和方法，都会被构造函数所拥有
prototype原型对象有一个constructor属性指回构造函数本身
所有我们就可以把那些不变的方法，直接定义在prototype对象上，这样所有对象的实例就可以共享这种方法

```

</v-click>

<v-click>

```js{all|4,5,6|8,9,10|all}
function Person(name, nation) {
            this.name = name
            this.nation = nation
            this.introduce = function() {
                console.log(`${this.name}的国籍是${this.nation}`)
            }
        }
        Person.prototype.introduce = function() {
            console.log(`${this.name}的国籍是${this.nation}`)
        }
var Dad = new Person("大明", "外国")
var Son = new Person("小明", "外国")
Dad.introduce === Son.introduce --> true
```

</v-click>

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

---

# 对象原型__proto__

```md
对象都会有一个属性__proto__指向构造函数的prototype原型对象

这也是为什么实例对象可以访问构造函数原型对象的属性和方法

且 构造函数的原型对象和实例对象的原型是等价的

但__proto__是一个非标准属性，该特性已经从 Web 标准中删除

虽然一些浏览器目前仍然支持它，但也许会在未来的某个时间停止支持，请尽量不要使用该特性。
```

---

# 原型链

```md
当对象查找一个属性的时候，如果没有在自身找到，那么就会查找自身的原型，

如果原型还没有找到，那么会继续查找原型的原型，直到找到 Object.prototype 的原型时，此时原型为 null，查找停止。 

这种通过原型链接的逐级向上的查找链被称为原型链
```

---

# 一种更好的方法判断数据的数据类型

#### 使用：Object.prototype.toString.call()

##### <br>每个对象都有一个 toString() 方法，当该对象被表示为一个文本值时，或者一个对象以预期的字符串方式引用时自动调用。  

##### 默认情况下，toString() 方法被每个 Object 对象继承。  

##### 如果此方法在自定义对象中未被覆盖，toString() 返回 "[object type]"，其中 type 是对象的类型。以下代码说明了这一点：

<v-click>

``` js
const a = ['Hello','World']
a.toString() //"Hello,World" 
const b = {
            0: 'Hello',
            1: 'World'
        }
b.toString() //"[object Object]"

```

</v-click>

---

所以我们可以通过下列方法来更严谨地检测数据的数据类型

<v-click>

```js{1,2|3|4|5|6|7|8|9|10|11|12|13|14|15|16|17|18|19|20|21|22|all}
var a = 1
Object.prototype.toString.call(a) // "[object Number]"
var b = true
Object.prototype.toString.call(b) // "[object Boolean]"
var c = null
Object.prototype.toString.call(c) // "[object Null]"
var d = undefined 
Object.prototype.toString.call(d) // "[object Undefined]"
var e = 'ticknet'
Object.prototype.toString.call(e) // "[object String]"
var f = Symbol('ticknet')
Object.prototype.toString.call(f) // "[object Symbol]"
var g = 42n
Object.prototype.toString.call(g) // "[object BigInt]"
var h = [1,2,3,4]
Object.prototype.toString.call(h) // "[object Array]"
var i = function(){}
Object.prototype.toString.call(i) // "[object Function]"
var j = new Object()
Object.prototype.toString.call(j) // "[object Object]"
var j = new Date()
Object.prototype.toString.call(j) // "[object Date]"
```

</v-click>

---


# What is Slidev? 

- 📝 Markdown 支持 —— 使用你最喜欢的编辑器和工作流编写 Markdown 文件
- 🧑‍💻 开发者友好 —— 内置代码高亮、实时编码等功能
- 🎨 可定制主题 —— 以 npm 包的形式共享、使用主题
- 🌈 灵活样式 —— 使用 Windi CSS 按需使用的实用类和易用的内嵌样式表
- 🤹 可交互 —— 无缝嵌入 Vue 组件
- 🎙 演讲者模式 —— 可以使用另一个窗口，甚至是你的手机来控制幻灯片
- 🎨 绘图 - 在你的幻灯片上进行绘图和批注
- 📰 图表支持 —— 使用文本描述语言创建图表
- 🌟 图标 —— 能够直接从任意图标库中获取图标
- 💻 编辑器 —— 集成的编辑器，或者使用 VS Code 扩展
- 📤 跨平台 —— 能够导出 PDF、PNG 文件，甚至是一个可以托管的单页应用
- ⚡️ 快速 —— 基于 Vite 的即时重载
- 🛠 可配置 —— 支持使用 Vite 插件、Vue 组件以及任何的 npm 包

<br>
<br>
<div class="abs-br m-6 flex">
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

Read more about [Why Slidev?](https://sli.dev/guide/why)

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>


---

# Components

<div grid="~ cols-2 gap-4">
<div>

You can use Vue components directly inside your slides.

We have provided a few built-in components like `<Tweet/>` and `<Youtube/>` that you can use directly. And adding your custom components is also super easy.

```html
<Counter :count="10" />
```

<!-- ./components/Counter.vue -->
<Counter :count="10" m="t-4" />

Check out [the guides](https://sli.dev/builtin/components.html) for more.

</div>
<div>

```html
<Tweet id="1390115482657726468" />
```

<Tweet id="1390115482657726468" scale="0.65" />

</div>
</div>


---
class: px-20
---

# Themes

Slidev comes with powerful theming support. Themes can provide styles, layouts, components, or even configurations for tools. Switching between themes by just **one edit** in your frontmatter:

<div grid="~ cols-2 gap-2" m="-t-2">

```yaml
---
theme: default
---
```

```yaml
---
theme: seriph
---
```

<img border="rounded" src="https://github.com/slidevjs/themes/blob/main/screenshots/theme-default/01.png?raw=true">

<img border="rounded" src="https://github.com/slidevjs/themes/blob/main/screenshots/theme-seriph/01.png?raw=true">

</div>

Read more about [How to use a theme](https://sli.dev/themes/use.html) and
check out the [Awesome Themes Gallery](https://sli.dev/themes/gallery.html).

---
preload: false
---

# Animations

Animations are powered by [@vueuse/motion](https://motion.vueuse.org/).

```html
<div
  v-motion
  :initial="{ x: -80 }"
  :enter="{ x: 0 }">
  Slidev
</div>
```

<div class="w-60 relative mt-6">
  <div class="relative w-40 h-40">
    <img
      v-motion
      :initial="{ x: 800, y: -100, scale: 1.5, rotate: -50 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://sli.dev/logo-square.png"
    />
    <img
      v-motion
      :initial="{ y: 500, x: -100, scale: 2 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://sli.dev/logo-circle.png"
    />
    <img
      v-motion
      :initial="{ x: 600, y: 400, scale: 2, rotate: 100 }"
      :enter="final"
      class="absolute top-0 left-0 right-0 bottom-0"
      src="https://sli.dev/logo-triangle.png"
    />
  </div>

  <div
    class="text-5xl absolute top-14 left-40 text-[#2B90B6] -z-1"
    v-motion
    :initial="{ x: -80, opacity: 0}"
    :enter="{ x: 0, opacity: 1, transition: { delay: 2000, duration: 1000 } }">
    Slidev
  </div>
</div>

<!-- vue script setup scripts can be directly used in markdown, and will only affects current page -->
<script setup lang="ts">
const final = {
  x: 0,
  y: 0,
  rotate: 0,
  scale: 1,
  transition: {
    type: 'spring',
    damping: 10,
    stiffness: 20,
    mass: 2
  }
}
</script>

<div
  v-motion
  :initial="{ x:35, y: 40, opacity: 0}"
  :enter="{ y: 0, opacity: 1, transition: { delay: 3500 } }">

[Learn More](https://sli.dev/guide/animations.html#motion)

</div>

---

# LaTeX

LaTeX is supported out-of-box powered by [KaTeX](https://katex.org/).

<br>

Inline $\sqrt{3x-1}+(1+x)^2$

Block
$$
\begin{array}{c}

\nabla \times \vec{\mathbf{B}} -\, \frac1c\, \frac{\partial\vec{\mathbf{E}}}{\partial t} &
= \frac{4\pi}{c}\vec{\mathbf{j}}    \nabla \cdot \vec{\mathbf{E}} & = 4 \pi \rho \\

\nabla \times \vec{\mathbf{E}}\, +\, \frac1c\, \frac{\partial\vec{\mathbf{B}}}{\partial t} & = \vec{\mathbf{0}} \\

\nabla \cdot \vec{\mathbf{B}} & = 0

\end{array}
$$

<br>

[Learn more](https://sli.dev/guide/syntax#latex)

---

# Diagrams

You can create diagrams / graphs from textual descriptions, directly in your Markdown.

<div class="grid grid-cols-3 gap-10 pt-4 -mb-6">

```mermaid {scale: 0.5}
sequenceDiagram
    Alice->John: Hello John, how are you?
    Note over Alice,John: A typical interaction
```

```mermaid {theme: 'neutral', scale: 0.8}
graph TD
B[Text] --> C{Decision}
C -->|One| D[Result 1]
C -->|Two| E[Result 2]
```

```plantuml {scale: 0.7}
@startuml

package "Some Group" {
  HTTP - [First Component]
  [Another Component]
}

node "Other Groups" {
  FTP - [Second Component]
  [First Component] --> FTP
}

cloud {
  [Example 1]
}


database "MySql" {
  folder "This is my folder" {
    [Folder 3]
  }
  frame "Foo" {
    [Frame 4]
  }
}


[Another Component] --> [Example 1]
[Example 1] --> [Folder 3]
[Folder 3] --> [Frame 4]

@enduml
```

</div>

[Learn More](https://sli.dev/guide/syntax.html#diagrams)


---
layout: center
class: text-center
---

# Thank You
