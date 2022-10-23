# vue3学习策略

## 1.基础JavaScript和typescript知识掌握

## 2.掌握vue常用指令

### 2.1 文本插值语法{{}}[#](https://cn.vuejs.org/guide/essentials/template-syntax.html#text-interpolation)

最基本的数据绑定形式是文本插值，它使用的是“Mustache”语法 (即双大括号)：

template

```
<span>Message: {{ msg }}</span>
```

双大括号标签会被替换为相应组件实例中 `msg` 属性的值。同时每次 `msg` 属性更改时它也会同步更新。



#### v-text[#](https://cn.vuejs.org/api/built-in-directives.html#v-text)

更新元素的文本内容。

- **期望的绑定值类型：**`string`

- **详细信息**

  `v-text` 通过设置元素的 [textContent](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent) 属性来工作，因此它将覆盖元素中所有现有的内容。如果你需要更新 `textContent` 的部分，应该使用 [mustache interpolations](https://cn.vuejs.org/guide/essentials/template-syntax.html#text-interpolation) 代替。

- **示例**

  template

  ```
  <span v-text="msg"></span>
  <!-- 等同于 -->
  <span>{{msg}}</span>
  ```

------

### 2.2 v-html指令[#](https://cn.vuejs.org/guide/essentials/template-syntax.html#raw-html)

双大括号会将数据解释为纯文本，而不是 HTML。若想插入 HTML，你需要使用 [`v-html` 指令](https://cn.vuejs.org/api/built-in-directives.html#v-html)：

template

```
<template>
    <p>
    Using v-html directive:<span style="color: red" v-html="rawHtml"></span>
    </p>  
</template>

<script>
export default {
  data() {
    return {
      rawHtml: "This should be red.",
    };
  },
};
</script>
```



Using v-html directive:: <span style="color: red">This should be red.</span>



这里我们遇到了一个新的概念。这里看到的 `v-html` attribute 被称为一个**指令**。指令由 `v-` 作为前缀，表明它们是一些由 Vue 提供的特殊 attribute，你可能已经猜到了，它们将为渲染的 DOM 应用特殊的响应式行为。这里我们做的事情简单来说就是：在当前组件实例上，将此元素的 innerHTML 与 `rawHtml` 属性保持同步。

`span` 的内容将会被替换为 `rawHtml` 属性的值，插值为纯 HTML——数据绑定将会被忽略。注意，你不能使用 `v-html` 来拼接组合模板，因为 Vue 不是一个基于字符串的模板引擎。在使用 Vue 时，应当使用组件作为 UI 重用和组合的基本单元。

安全警告

在网站上动态渲染任意 HTML 是非常危险的，因为这非常容易造成 [XSS 漏洞](https://en.wikipedia.org/wiki/Cross-site_scripting)。请仅在内容安全可信时再使用 `v-html`，并且**永远不要**使用用户提供的 HTML 内容。

------

### 2.3  Attribute 绑定 v-bind[#](https://cn.vuejs.org/guide/essentials/template-syntax.html#attribute-bindings)

双大括号不能在 HTML attributes 中使用。想要响应式地绑定一个 attribute，应该使用 [`v-bind` 指令](https://cn.vuejs.org/api/built-in-directives.html#v-bind)：

template

```
<template>
  <a :href="url">微软搜索</a>
</template>

<script>
export default {
  data() {
    return {
      url: "https://cn.bing.com/",
    };
  },
};
</script>
```

`v-bind` 指令指示 Vue 将元素的 `id` attribute 与组件的 `dynamicId` 属性保持一致。如果绑定的值是 `null` 或者 `undefined`，那么该 attribute 将会从渲染的元素上移除。

#### 简写[#](https://cn.vuejs.org/guide/essentials/template-syntax.html#shorthand)

因为 `v-bind` 非常常用，我们提供了特定的简写语法：

开头为 `:` 的 attribute 可能和一般的 HTML attribute 看起来不太一样，但它的确是合法的 attribute 名称字符，并且所有支持 Vue 的浏览器都能正确解析它。此外，他们不会出现在最终渲染的 DOM 中。简写语法是可选的，但相信在你了解了它更多的用处后，你应该会更喜欢它。

> 接下来的指引中，我们都将在示例中使用简写语法，因为这是在实际开发中更常见的用法。

### 2.4 布尔型 Attribute[#](https://cn.vuejs.org/guide/essentials/template-syntax.html#boolean-attributes)

[布尔型 attribute](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Attributes#布尔值属性) 依据 true / false 值来决定 attribute 是否应该存在于该元素上。[`disabled`](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/disabled) 就是最常见的例子之一。

`v-bind` 在这种场景下的行为略有不同：

:disabled=“true”

```
<template>
  <button :disabled="isButtonDisabled">Button</button>
</template>

<script>
export default {
  data() {
    return {
      isButtonDisabled: true,
    };
  },
};
</script>
```



<button disabled></button><button disabled>Button</button>



:disabled=“false”

```
<template>
  <button :disabled="isButtonDisabled">Button</button>
</template>

<script>
export default {
  data() {
    return {
      isButtonDisabled: false,
    };
  },
};
</script>
```

> <button></button> <button>Button</button> 

当 `isButtonDisabled` 为[真值](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)或一个空字符串 (即 `<button disabled="">`) 时，元素会包含这个 `disabled` attribute。而当其为其他[假值](https://developer.mozilla.org/en-US/docs/Glossary/Falsy)时 attribute 将被忽略。

### 2.5 动态绑定多个值[#](https://cn.vuejs.org/guide/essentials/template-syntax.html#dynamically-binding-multiple-attributes)

如果你有像这样的一个包含多个 attribute 的 JavaScript 对象：

js

```
data() {
  return {
    objectOfAttrs: {
      id: 'container',
      class: 'wrapper'
    }
  }
}
```

通过不带参数的 `v-bind`，你可以将它们绑定到单个元素上：

template

```
<div v-bind="objectOfAttrs"></div>
```

### 2.6  v-pre[#](https://cn.vuejs.org/api/built-in-directives.html#v-pre)

跳过该元素及其所有子元素的编译。

- **无需传入**

- **详细信息**

  元素内具有 `v-pre`，所有 Vue 模板语法都会被保留并按原样渲染。最常见的用例就是显示原始双大括号标签及内容。

- **示例：**

  template

  ```
  <span v-pre>{{ this will not be compiled }}</span>
  ```

### 2.7 v-once[#](https://cn.vuejs.org/api/built-in-directives.html#v-once)

仅渲染元素和组件一次，并跳过之后的更新。

- **无需传入**

- **详细信息**

  在随后的重新渲染，元素/组件及其所有子项将被当作静态内容并跳过渲染。这可以用来优化更新时的性能。

  template

  ```
  <!-- 单个元素 -->
  <span v-once>This will never change: {{msg}}</span>
  <!-- 带有子元素的元素 -->
  <div v-once>
    <h1>comment</h1>
    <p>{{msg}}</p>
  </div>
  <!-- 组件 -->
  <MyComponent v-once :comment="msg" />
  <!-- `v-for` 指令 -->
  <ul>
    <li v-for="i in list" v-once>{{i}}</li>
  </ul>
  ```

数据更新时，只会显示初始状态值，后续不随数据更新而改变。

### 2.8 v-memo [#](https://cn.vuejs.org/api/built-in-directives.html#v-memo)

- **期望的绑定值类型：**`any[]`

- **详细信息**

  缓存一个模板的子树。在元素和组件上都可以使用。为了实现缓存，该指令需要传入一个固定长度的依赖值数组进行比较。如果数组里的每个值都与最后一次的渲染相同，那么整个子树的更新将被跳过。举例来说：

  template

  ```
  <div v-memo="[valueA, valueB]">
    ...
  </div>
  ```

  当组件重新渲染，如果 `valueA` 和 `valueB` 都保持不变，这个 `<div>` 及其子项的所有更新都将被跳过。实际上，甚至虚拟 DOM 的 vnode 创建也将被跳过，因为缓存的子树副本可以被重新使用。

  正确指定缓存数组很重要，否则应该生效的更新可能被跳过。`v-memo` 传入空依赖数组 (`v-memo="[]"`) 将与 `v-once` 效果相同。

  **与 `v-for` 一起使用**

  `v-memo` 仅用于性能至上场景中的微小优化，应该很少需要。最常见的情况可能是有助于渲染海量 `v-for` 列表 (长度超过 1000 的情况)：

  template

  ```
  <div v-for="item in list" :key="item.id" v-memo="[item.id === selected]">
    <p>ID: {{ item.id }} - selected: {{ item.id === selected }}</p>
    <p>...more child nodes</p>
  </div>
  ```

  当组件的 `selected` 状态改变，默认会重新创建大量的 vnode，尽管绝大部分都跟之前是一模一样的。`v-memo` 用在这里本质上是在说“只有当该项的被选中状态改变时才需要更新”。这使得每个选中状态没有变的项能完全重用之前的 vnode 并跳过差异比较。注意这里 memo 依赖数组中并不需要包含 `item.id`，因为 Vue 也会根据 item 的 `:key` 进行判断。

  警告

  当搭配 `v-for` 使用 `v-memo`，确保两者都绑定在同一个元素上。**`v-memo` 不能用在 `v-for` 内部。**

  `v-memo` 也能被用于在一些默认优化失败的边际情况下，手动避免子组件出现不需要的更新。但是一样的，开发者需要负责指定正确的依赖数组以免跳过必要的更新。

### 2.9 v-bind

#### 2.9.1 Class 与 Style 绑定[#](https://cn.vuejs.org/guide/essentials/class-and-style.html#class-and-style-bindings)



数据绑定的一个常见需求场景是操纵元素的 CSS class 列表和内联样式。因为 `class` 和 `style` 都是 attribute，我们可以和其他 attribute 一样使用 `v-bind` 将它们和动态的字符串绑定。但是，在处理比较复杂的绑定时，通过拼接生成字符串是麻烦且易出错的。因此，Vue 专门为 `class` 和 `style` 的 `v-bind` 用法提供了特殊的功能增强。除了字符串外，表达式的值也可以是对象或数组。

##### 绑定 HTML class[#](https://cn.vuejs.org/guide/essentials/class-and-style.html#binding-html-classes)

##### 绑定对象[#](https://cn.vuejs.org/guide/essentials/class-and-style.html#binding-to-objects)

我们可以给 `:class` (`v-bind:class` 的缩写) 传递一个对象来动态切换 class：

template

```
<div :class="{ active: isActive }"></div>
```

上面的语法表示 `active` 是否存在取决于数据属性 `isActive` 的[真假值](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)。

你可以在对象中写多个字段来操作多个 class。此外，`:class` 指令也可以和一般的 `class` attribute 共存。举例来说，下面这样的状态：

js

```
data() {
  return {
    isActive: true,
    hasError: false
  }
}
```

配合以下模板：

template

```
<div
  class="static"
  :class="{ active: isActive, 'text-danger': hasError }"
></div>
```

渲染的结果会是：

template

```
<div class="static active"></div>
```

当 `isActive` 或者 `hasError` 改变时，class 列表会随之更新。举例来说，如果 `hasError` 变为 `true`，class 列表也会变成 `"static active text-danger"`。

绑定的对象并不一定需要写成内联字面量的形式，也可以直接绑定一个对象：

js

```
data() {
  return {
    classObject: {
      active: true,
      'text-danger': false
    }
  }
}
```

template

```
<div :class="classObject"></div>
```

这也会渲染出相同的结果。我们也可以绑定一个返回对象的[计算属性](https://cn.vuejs.org/guide/essentials/computed.html)。这是一个常见且很有用的技巧：

js

```
data() {
  return {
    isActive: true,
    error: null
  }
},
computed: {
  classObject() {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

template

```
<div :class="classObject"></div>
```

### 绑定数组[#](https://cn.vuejs.org/guide/essentials/class-and-style.html#binding-to-arrays)

我们可以给 `:class` 绑定一个数组来渲染多个 CSS class：

js

```
data() {
  return {
    activeClass: 'active',
    errorClass: 'text-danger'
  }
}
```

template

```
<div :class="[activeClass, errorClass]"></div>
```

渲染的结果是：

template

```
<div class="active text-danger"></div>
```

如果你也想在数组中有条件地渲染某个 class，你可以使用三元表达式：

template

```
<div :class="[isActive ? activeClass : '', errorClass]"></div>
```

`errorClass` 会一直存在，但 `activeClass` 只会在 `isActive` 为真时才存在。

然而，这可能在有多个依赖条件的 class 时会有些冗长。因此也可以在数组中嵌套对象：

template

```
<div :class="[{ active: isActive }, errorClass]"></div>
```

### 在组件上使用[#](https://cn.vuejs.org/guide/essentials/class-and-style.html#with-components)

> 本节假设你已经有 [Vue 组件](https://cn.vuejs.org/guide/essentials/component-basics.html)的知识基础。如果没有，你也可以暂时跳过，以后再阅读。

对于只有一个根元素的组件，当你使用了 `class` attribute 时，这些 class 会被添加到根元素上，并与该元素上已有的 class 合并。

举例来说，如果你声明了一个组件名叫 `MyComponent`，模板如下：

template

```
<!-- 子组件模板 -->
<p class="foo bar">Hi!</p>
```

在使用时添加一些 class：

template

```
<!-- 在使用组件时 -->
<MyComponent class="baz boo" />
```

渲染出的 HTML 为：

template

```
<p class="foo bar baz boo">Hi</p>
```

Class 的绑定也是同样的：

template

```
<MyComponent :class="{ active: isActive }" />
```

当 `isActive` 为真时，被渲染的 HTML 会是：

template

```
<p class="foo bar active">Hi</p>
```

如果你的组件有多个根元素，你将需要指定哪个根元素来接收这个 class。你可以通过组件的 `$attrs` 属性来实现指定：

template

```
<!-- MyComponent 模板使用 $attrs 时 -->
<p :class="$attrs.class">Hi!</p>
<span>This is a child component</span>
```

template

```
<MyComponent class="baz" />
```

这将被渲染为：

html

```
<p class="baz">Hi!</p>
<span>This is a child component</span>
```

你可以在[透传 Attribute](https://cn.vuejs.org/guide/components/attrs.html) 一章中了解更多组件的 attribute 继承的细节。

## 绑定内联样式[#](https://cn.vuejs.org/guide/essentials/class-and-style.html#binding-inline-styles)

### 绑定对象[#](https://cn.vuejs.org/guide/essentials/class-and-style.html#binding-to-objects-2)

`:style` 支持绑定 JavaScript 对象值，对应的是 [HTML 元素的 `style` 属性](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/style)：

js

```
data() {
  return {
    activeColor: 'red',
    fontSize: 30
  }
}
```

template

```
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

尽管推荐使用 camelCase，但 `:style` 也支持 kebab-cased 形式的 CSS 属性 key (对应其 CSS 中的实际名称)，例如：

template

```
<div :style="{ 'font-size': fontSize + 'px' }"></div>
```

直接绑定一个样式对象通常是一个好主意，这样可以使模板更加简洁：

js

```
data() {
  return {
    styleObject: {
      color: 'red',
      fontSize: '13px'
    }
  }
}
```

template

```
<div :style="styleObject"></div>
```

同样的，如果样式对象需要更复杂的逻辑，也可以使用返回样式对象的计算属性。

### 绑定数组[#](https://cn.vuejs.org/guide/essentials/class-and-style.html#binding-to-arrays-2)

我们还可以给 `:style` 绑定一个包含多个样式对象的数组。这些对象会被合并后渲染到同一元素上：

template

```
<div :style="[baseStyles, overridingStyles]"></div>
```

### 自动前缀[#](https://cn.vuejs.org/guide/essentials/class-and-style.html#auto-prefixing)

当你在 `:style` 中使用了需要[浏览器特殊前缀](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix)的 CSS 属性时，Vue 会自动为他们加上相应的前缀。Vue 是在运行时检查该属性是否支持在当前浏览器中使用。如果浏览器不支持某个属性，那么将测试加上各个浏览器特殊前缀，以找到哪一个是被支持的。

### 样式多值[#](https://cn.vuejs.org/guide/essentials/class-and-style.html#multiple-values)

你可以对一个样式属性提供多个 (不同前缀的) 值，举例来说：

template

```
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

数组仅会渲染浏览器支持的最后一个值。在这个示例中，在支持不需要特别前缀的浏览器中都会渲染为 `display: flex`。

### 使用 JavaScript 表达式[#](https://cn.vuejs.org/guide/essentials/template-syntax.html#使用-javascript-表达式)

至此，我们仅在模板中绑定了一些简单的属性名。但是 Vue 实际上在所有的数据绑定中都支持完整的 JavaScript 表达式：

template

```
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div :id="`list-${id}`"></div>
```

这些表达式都会被作为 JavaScript ，以组件为作用域解析执行。

在 Vue 模板内，JavaScript 表达式可以被使用在如下场景上：

- 在文本插值中 (双大括号)
- 在任何 Vue 指令 (以 `v-` 开头的特殊 attribute) attribute 的值中

### 仅支持表达式[#](https://cn.vuejs.org/guide/essentials/template-syntax.html#仅支持表达式)

每个绑定仅支持**单一表达式**，也就是一段能够被求值的 JavaScript 代码。一个简单的判断方法是是否可以合法地写在 `return` 后面。

因此，下面的例子都是**无效**的：

template

```
<!-- 这是一个语句，而非表达式 -->
{{ var a = 1 }}

<!-- 条件控制也不支持，请使用三元表达式 -->
{{ if (ok) { return message } }}
```

## 3.掌握如何使用vue-cli 和 vite 快速创建工程化的Vue项目

## 4.掌握SFC单文件组件的各种详细用法

## 5.掌握如何实现组件之间的数据共享

## 6.掌握Vue项目中如何正确配置和使用axios

## 7.掌握自定义指令、过滤器、插槽的使用

## 8.掌握 setup 语法糖 vue-router4 和pinia的使用

## 9.掌握dev tools webpack vite 调试打包工具的使用

## 10.掌握 ref  动态组件 $nextTick 的使用

## 11.掌握Element-plus ant  design 组件库的使用

## 12.寻找Vue 实现企业级项目的开发

