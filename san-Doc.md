# 完整Doc地址
[https://ecomfe.github.io/san/](https://ecomfe.github.io/san/)

# 安装/包下载

> 1. [https://github.com/ecomfe/san/releases](https://github.com/ecomfe/san/releases)
>
> * npm install san

# 特点

1. HTML模版

> 模板的写法与我们现在使用的模板写法类似，由于最后是在JS中引用模板，且使用时实际上是以字符串来解析，因此模板的存储方式和调用方式非常灵活（js/html/xml/etc...）

* 数据驱动

> 和很多新框架一样，也是采取数据驱动的方式，并且san的数据驱动是不需要任何服务器的，换言之，哪怕是一个静态的html页面，也是可以通过数据驱动的方式来决定展示内容。
>
> 虽然是数据驱动的方式，但是在有需要的情况下，只要能够保证数据的更新（利用钩子函数或手动同步数据），也是可以引用一般的动画插件（jquery）

* 组件化

> san的每个操作对象都是一个基于san创建的一个组件，组件可完全独立并且可相互引用

* 高性能视图

> 通过修改数据的方法，视图引擎能够直接刷新需要变更的视图区域，无需进行任何检测，性能更高。

* 组件反解

> 这个很大程度上可以解决js模板对于seo不友好的问题，以前的情况是，为了满足seo要求，会把需要的数据同步展示出来，然后js模板另外再写一套，交互时再把原来的内容替换掉这样，虽然也能解决问题，但是维护成本增大（毕竟至少要维护两套模板），而san这里的方式是，页面需要同步的html我们照常来写，然后把需要模板化的内容添加特殊html标签属性来进行模板的识别，然后就能通过这个html反解出组件对应的模板/事件/数据绑定

* 体积小巧

> <10k (gzipped) 的体积，无需担心对页面下载带来负担。体积强迫症患者的福音。

* 良好的兼容性

> ie8

* 模块管理自由

> 项目中可以任意选择 ESNext Module 或 AMD 管理模块。当然，如果你想要用全局变量也是支持的

* 引用方便

> 支持多种引用方式：NPM、GitHub、下载、HTTP 与 HTTPS CDN，让开发和线上引用更便利。

# 使用

## script 引用

``` html
<!-- 引用直接下载下来的San -->
<script src="san的目录/dist/san.js"></script>
<!-- 引用通过NPM下载下来的San -->
<script src="node_modules/san/dist/san.js"></script>
```
## AMD

``` javascript
require.config({
    packages: [
        {
            name: 'san',
            location: 'san-path/dist/san'
        }
    ]
});
```

# MVVM

MVVM: Model-View-ViewModel

一句话总结 Web 前端 MVVM：操作数据，就是操作视图，就是操作 DOM（所以无须操作 DOM ）。

> 三层架构：
>
> Model: Model: 域模型，用于持久化
>
> View: 作为视图模板存在
>
> ViewModel: 作为视图的模型，为视图服务

# Hello world

例1
```javascript
var MyApp = san.defineComponent({
    template: '<p>Hello {{name}}!</p>',
    initData: function () {
        return {
            name: 'world'
        };
    }
});
var myApp = new MyApp();
myApp.attach(document.body);
```

例子：./examples/example-1.html

> 基本只需要三步就能把需要的内容展示到页面上
>
> 1. 定义组件对象
>
> * 声明对象
>
> * 把对象放到页面对应位置
>
> __初始化内容时，模板内容必须为单个容器，所有内容也必须在这个容器中__

# 数据绑定

ps: 关于数据绑定，san的数据绑定方式与微信/支付宝小程序非常类似

## 数据展示绑定
例2
```html
<span text="{{name}}" title="This is {{name}}">{{name}}</span>
```
例3
```html
<p>Hello {{name | upper}}!</p>
```

> 数据绑定方式为 {{xxxxx}}这种方式，其中内容可以为变量、表达式以及过滤器，过滤的使用方式由例3所示

## 数据双向绑定
例4
```html
<input type="text" value="{= name =}">
<span>{{= name =}}</span>
```

例子：./examples/example-1.html

> 数据双向绑定方式为 {= xxxxxxx =} 这种方式，内容只可以为普通变量和属性访问表达式。
>
> 但是这里有一点要注意，在我测试的时候，发现有时候使用的是{= xxxx =}，而有时候却是 {{= xxxxxx =}} 这样的，具体原因未知，但归纳下来基本上这样的：
>
> 标签属性中使用的话为 {= xxxxx =} 这种方式， 而当需要插值展示的时候则为 {{= xxxxx =}} 这样。

## 表达式/过滤器

__关于表达式__

大致有以下这些

1. 普通变量
  1. <p>{{name}}</p>
* 属性访问
  1. <p>{{person.name}}</p>
  * <p>{{persons[1]}}</p>
* 一元否定
  1. <p>{{!isOK}}</p>
  * <p>{{!!isOK}}</p>
* 二元运算
  1. <p>{{num1 + num2}}</p>
  * <p>{{num1 - num2}}</p>
  * <p>{{num1 * num2}}</p>
  * <p>{{num1 / num2}}</p>
  * <p>{{num1 + num2 * num3}}</p>
* 二元关系
  1. <p>{{num1 > num2}}</p>
  * <p>{{num1 !== num2}}</p>
* 三元条件
  1. <p>{{num1 > num2 ? num1 : num2}}</p>
* 括号
  1. <p>{{a * (b + c)}}</p>

__关于过滤器__


## 数据操作

1.初始化时设置数据

例5
```javascript
san.defineComponent({
    initData: function () {
        return {
            width: 200,
            top: 100,
            left: -1000
        };
    }
});
```

例子：./examples/example-1.html

2.set 方法

例6
```javascript
san.defineComponent({
    attached: function () {
        requestUser().then(this.userReceived.bind(this));
    },
    userReceived: function (data) {
        this.data.set('user', data);
    },
    changeEmail: function (email) {
        this.data.set('user.email', email);
    }
});
```

例子：./examples/example-1.html

3.获取数据

例7
```javascript
san.defineComponent({
    alert: function () {
      alert(this.data.get('user'))
    }
});
```

例子：./examples/example-1.html

> 这里可以看出，在数据设置以及获取数据的方式上，与小程序是非常类似的，但是除此以外，san还提供了一些方法来设置数据

4.push
```javascript
//在数组末尾插入一条数据。
san.defineComponent({
    addUser: function (name) {
        this.data.push('users', {name: name});
    }
});
```

例子：./examples/example-2.html

5.pop
```javascript
//在数组末尾弹出一条数据
san.defineComponent({
    rmLast: function () {
        this.data.pop('users');
    }
});
```

例子：./examples/example-2.html

6.unshift
```javascript
//在数组开始插入一条数据。
san.defineComponent({
    addUser: function (name) {
        this.data.unshift('users', {name: name});
    }
});
```

例子：./examples/example-2.html

7.shift
```javascript
//在数组开始弹出一条数据。
san.defineComponent({
    rmFirst: function () {
        this.data.shift('users');
    }
});
```

例子：./examples/example-2.html

7.remove
```javascript
//移除一条数据。只有当数组项与传入项完全相等(===)时，数组项才会被移除。
san.defineComponent({
    rm: function (user) {
        this.data.remove('users', user);
    }
});
```

例子：./examples/example-2.html

8.removeAt
```javascript
//通过数据项的索引移除一条数据。
san.defineComponent({
    rmAt: function (index) {
        this.data.removeAt('users', index);
    }
});
```

例子：./examples/example-2.html

9.splice
```javascript
//向数组中添加或删除项目。
san.defineComponent({
    rm: function (index, deleteCount) {
        this.data.splice('users', [index, deleteCount]);
    }
});
```

例子：./examples/example-2.html

## 数据校验

关于数据校验，用法与react中非常类似，但是实际尝试后发现应用起来效果并不好，其中主要原因就是错误信息的抛出方式是console error，因此除了组件调用时的参数验证使用这种方式，正常情况下的数据验证还是自己进行比较好。具体例子可查看官方文档[https://ecomfe.github.io/san/tutorial/data-checking/](https://ecomfe.github.io/san/tutorial/data-checking/)

# 表单

具体例子参考 ./examples/example-3.html

# 生命周期

1. compiled - 组件视图模板编译完成
* inited - 组件实例初始化完成
* created - 组件元素创建完成
* attached - 组件已被附加到页面中
* detached - 组件从页面中移除
* disposed - 组件卸载完成

> 生命周期代表组件的状态，生命周期本质就是状态管理。
>
> 在生命周期到达时，对应的钩子函数会被触发运行。
>
> 并存。比如 attached 和 created 等状态是同时并存的。
>
> 互斥。attached 和 detached 是互斥的，disposed 会互斥掉其它所有的状态。
>
> 有的时间点并不代表组件状态，只代表某个行为。当行为完成时，钩子函数也会触发。如 updated 代表每次数据变化导致的视图变更完成。

# template

之前提到了，San 要求组件对应 一个 HTML 元素，所以视图模板定义时，只能包含一个 HTML 元素，其它所有内容需要放在这个元素下。因此在定义模板时需要把内容包在一个容器中，san 提供template标签来作为外层容器，效果与div等效

# slot/组件引用

组件引用：实际就与react中的引入组件是一样的，但是需要在当前组件中声明调用组件以及其别名才行

> 从例子中可以看出，当调用某个组件的时候，当前组件的数据并不会影响调用组件的数据，因此每个组件的数据都是独立的，除非全局函数，否则在闭包环境下数据都是安全的
>
> 不过和react一样，在调用时也可通过传递参数来控制调用组件的数据，定义方式也非常简单。

slot: 可以认为是组件引用的进阶版，支持通过在调用组件中定义插槽，然后在调用时插入需要的内容

例子参考 ./examples/example-5.html


# 实例化组件时定义数据

传入数据例子

```javascript
var test = new Test({
  data: {
    text: 'Hello world'
  }
});
```

> 在实例化时传入的数据会直接绑定进组件，且优先级要高于组件内的initData

# 过滤器定义

```javascript
var test = new Test({
  filters: {
    testFilter: function (arg1, arg2) {
      return arg1 + arg2;
    }
  }
});
```

在模板中调用方式是这样的，最后根据定义的内容得到结果是 text + 'add message'

```html
<div>{{text | testFilter('add message')}}</div>
```

# 消息 dispatch/message

消息将沿着组件树向上传递，__直到遇到第一个处理该消息的组件__，则停止。通过 messages 可以声明组件要处理的消息。messages 是一个对象，key 是消息名称，value 是消息处理的函数，接收一个包含 target(派发消息的组件) 和 value(消息的值) 的参数对象。

消息主要用于组件与非 owner 的上层组件进行通信。

例子为 ./examples/example-7.html

# 动态组件

运用场景： 当需要使用到某些子组件时，希望能在需要的时候才进行初始化。

用法： 在初始化组件时，不需在components中定义子组件，而是在某些事件中对子组件进行实例化再插入到需要的位置。

例子： ./examples/example-8.html

# 组件反解

官方文档：[https://ecomfe.github.io/san/tutorial/reverse/](https://ecomfe.github.io/san/tutorial/reverse/)

```html
<div id="wrap">
  <div>
    <!--s-data:{'text': 'hello'}-->
    <span on-click="handleClick"><!--s-text:{{text}}-->world<!--/s-text--></span>
  </div>
</div>
```

```javascript
var MyComponent = san.defineComponent({
  handleClick: function () {
    console.log(this.data.get('text'));
    this.data.set('text', this.data.get('text'));
  }
});
var myComponent = new MyComponent({
  el: document.getElementById('wrap')
});
```

这里要注意一点，s-data必须与s-text值一致，不然会造成数值不统一，继而在后续的操作中出现数据串行的问题。比如这里例子中，s-data中设置text为hello，而在s-text中设置text为world，这时候触发事件，console出来的值hello，然而拿这个值去调用set方法，这时候实际的值却是world。

# 组件API

详细参考官方文档：[https://ecomfe.github.io/san/doc/api/](https://ecomfe.github.io/san/doc/api/)

## data

实例化时候使用，在定义组件时对应方法为initData，initData要求直接返回object

```javascript
//initData
var MyComponent = san.defineComponent({
  initData: function () {
    return {
      text: 'hello world'
    }
  }
});
//data
var component = new MyComponent({
  data: {
    text: 'Hello world'
  }
});
```

## el

组件根元素。传入此参数意味着不使用组件的 template 作为视图模板，一般为在使用组件反解时使用。

```javascript
var component = new MyComponent({
  el: document.getElementById('test')
})
```

## compiled

组件钩子函数，组件视图模板编译完成时调用

```javascript
var MyComponent = san.defineComponent({
  compiled: function () { }
})
```

## inited

组件钩子函数，组件实例初始化完成

```javascript
var MyComponent = san.defineComponent({
  inited: function () { }
})
```

## created

组件钩子函数，组件元素创建完成时调用

```javascript
var MyComponent = san.defineComponent({
  created: function () { }
})
```

## attached

组件钩子函数，组件已被附加到页面中时调用

```javascript
var MyComponent = san.defineComponent({
  attached: function () { }
})
```

## detached

组件钩子函数，组件从页面中移除时调用

```javascript
var MyComponent = san.defineComponent({
  detached: function () { }
})
```

## disposed

组件钩子函数，组件卸载完成时调用

```javascript
var MyComponent = san.defineComponent({
  disposed: function () { }
})
```

## updated

组件钩子函数，组件由于数据变化，视图完成一次刷新时调用

```javascript
var MyComponent = san.defineComponent({
  updated: function () { }
})
```

## template

组件视图模板内容，定义组件时声明

```javascript
var MyComponent = san.defineComponent({
  template: [
    '<div></div>'
  ].join('')
})
```

## filters

声明组件过滤器，定义组件时声明

```javascript
var MyComponent = san.defineComponent({
  filters: {
    filter1: function (value, arg) {
      return value + arg;
    }
  }
});
```

## components

声明组件中调用的子组件，定义组件时声明

```javascript
var SubComponent = san.defineComponent({});
var MyComponent = san.defineComponent({
  components: {
    'ui-sub': SubComponent
  },
  template: [
    '<ui-sub></ui-sub>'
  ].join('')
});
```

## computed

声明组件中需要计算后得到的参数（尽可能保证模板纯净）

```javascript
var MyComponent = san.defineComponent({
  computed: {
    name: function () {
      return this.data.get('familyName') + this.data.get('personalName')
    }
  }
})
```

## message

message是当组件作为上层组件时捕捉下层组件dispatch声明的方法时调用的方法

```javascript
var SelectItem = san.defineComponent({
    template:
        '<li on-click="select" class="{{value === selectValue ? \'selected\' : \'\'">'
        + '<slot></slot>'
        + '</li>',
    // 子组件在各种时机派发消息
    select: function () {
        var value = this.data.get('value');
        this.dispatch('UI:select-item-selected', value);
    }
});
var Select = san.defineComponent({
    template: '<ul><slot></slot></ul>',
    // 上层组件处理自己想要的消息
    messages: {
        'UI:select-item-selected': function (arg) {
            var value = arg.value;
            this.data.set('value', value);
            // 原则上上层组件允许更改下层组件的数据，因为更新流是至上而下的
            var len = this.items.length;
            while (len--) {
                this.items[len].data.set('selectValue', value);
            }
        }
    },
    inited: function () {
        this.items = [];
    }
});
var MyComponent = san.defineComponent({
    components: {
        'ui-select': Select,
        'ui-selectitem': SelectItem
    },
    template: ''
        + '<div>'
        + '  <ui-select value="{=value=}">'
        + '    <ui-selectitem value="1">one</ui-selectitem>'
        + '    <ui-selectitem value="2">two</ui-selectitem>'
        + '    <ui-selectitem value="3">three</ui-selectitem>'
        + '  </ui-select>'
        + '</div>'
});
```

## fire

派发一个自定义事件。San 为组件提供了自定义事件功能，组件开发者可以通过该方法派发事件。事件可以在视图模板中通过 on- 的方式绑定监听，也可以通过组件实例的 on 方法监听

```javascript
var Label = san.defineComponent({
    template: '<template class="ui-label"><a on-click="clicker" title="{{text}}">{{text}}</a></template>',
    clicker: function () {
        this.fire('customclick', this.data.get('text') + ' clicked');
    }
});
var MyComponent = san.defineComponent({
    initData: function () {
        return {name: 'San'};
    },
    components: {
        'ui-label': Label
    },
    template: '<div><ui-label text="{{name}}" on-customclick="labelClicker($event)"></ui-label></div>',
    labelClicker: function (doneMsg) {
        alert(doneMsg);
    }
});
```
