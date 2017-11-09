# 完整Doc地址
[https://ecomfe.github.io/san/](https://ecomfe.github.io/san/)

# 安装/包下载

> 1. [https://github.com/ecomfe/san/releases](https://github.com/ecomfe/san/releases)
>
> * npm install san

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

3.获取数据

例7
```javascript
san.defineComponent({
    alert: function () {
      alert(this.data.get('user'))
    }
});
```

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

5.pop
```javascript
//在数组末尾弹出一条数据
san.defineComponent({
    rmLast: function () {
        this.data.pop('users');
    }
});
```

6.unshift
```javascript
//在数组开始插入一条数据。
san.defineComponent({
    addUser: function (name) {
        this.data.unshift('users', {name: name});
    }
});
```

7.shift
```javascript
//在数组开始弹出一条数据。
san.defineComponent({
    rmFirst: function () {
        this.data.shift('users');
    }
});
```

7.remove
```javascript
//移除一条数据。只有当数组项与传入项完全相等(===)时，数组项才会被移除。
san.defineComponent({
    rm: function (user) {
        this.data.remove('users', user);
    }
});
```

8.removeAt
```javascript
//通过数据项的索引移除一条数据。
san.defineComponent({
    rmAt: function (index) {
        this.data.removeAt('users', index);
    }
});
```

9.splice
```javascript
//向数组中添加或删除项目。
san.defineComponent({
    rm: function (index, deleteCount) {
        this.data.splice('users', [index, deleteCount]);
    }
});
```

## 数据校验

关于数据校验，用法与react中非常类似，但是实际尝试后发现应用起来效果并不好，其中主要原因就是错误信息的抛出方式是console error，因此除了组件调用时的参数验证使用这种方式，正常情况下的数据验证还是自己进行比较好。具体例子可查看官方文档[https://ecomfe.github.io/san/tutorial/data-checking/](https://ecomfe.github.io/san/tutorial/data-checking/)

# 表单

具体例子参考 ./examples/example-3.html

#生命周期

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