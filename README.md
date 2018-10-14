# vue-question

### 1、vue五千条以上数据分页怎么做？甚至上万数据的时候？（你获取后台返回的数据渲染到页面上分页）

### 2、active-class是什么?是哪个组件的属性？
active-class是vue-router模块的router-link组件中的属性，用来做选中样式的切换。当 <router-link> 对应的路由匹配成功，将自动设置 class 属性值 .router-link-active。
默认值: "router-link-active"，设置链接激活时使用的 CSS 类名。
更改默认值方法：
1、可以通过路由的构造选项 linkActiveClass 来全局配置：
```
export default new Router({
  linkActiveClass: 'active',
})
```
2、在router-link中写入active-class
```
<router-link to="/home" class="menu-home" active-class="active">首页</router-link>
```

### 3、嵌套路由怎么定义

在实际项目中，应用界面通常由多层嵌套的组件组合而成，要实现嵌套路由需要在 VueRouter 的参数中使用 children 配置，这样就可以很好的实现路由嵌套。

example：
```
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User,
      children: [
        {
          // 当 /user/:id/profile 匹配成功，
          // UserProfile 会被渲染在 User 的 <router-view> 中
          path: 'profile',
          component: UserProfile
        },
        {
          // 当 /user/:id/posts 匹配成功
          // UserPosts 会被渲染在 User 的 <router-view> 中
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})
```
要注意，以 / 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。children 配置就是像 routes 配置一样的路由配置数组。
所以，可以嵌套多层路由。

基于上面的配置，当你访问 /user/foo 时，User 的出口是不会渲染任何东西，这是因为没有匹配到合适的子路由。如果你想要渲染点什么，可以提供一个 空的 子路由：
```
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id', component: User,
      children: [
        // 当 /user/:id 匹配成功，
        // UserHome 会被渲染在 User 的 <router-view> 中
        { path: '', component: UserHome },

        // ...其他子路由
      ]
    }
  ]
})
```

### 4、怎么定义vue-router的动态路由？怎么获取传过来的动态参数？

在 vue-router 的路由路径中使用“动态路径参数”(dynamic segment) 定义动态路由。

example:
```
const User = {
  template: '<div>User</div>'
}

const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头后面跟一个自定义变量，通过这个变量可以使用$route.params.id(自定义变量名，这里就是id)来获取传递过来的动态参数
    { path: '/user/:id', component: User }
  ]
})
```

### 5、this.$router 和 this.$route

通过注入路由器，可以在任何组件内通过 this.$router 访问路由器，也可以通过 this.$route 访问当前路由。
```
// Home.vue
export default {
  computed: {
    username () {
      // 我们很快就会看到 `params` 是什么
      return this.$route.params.username
    }
  },
  methods: {
    goBack () {
      window.history.length > 1
        ? this.$router.go(-1)
        : this.$router.push('/')
    }
  }
}
```

### 6、怎么把Vue Router 添加进项目
用 Vue.js + Vue Router 创建单页应用，是非常简单的。使用 Vue.js ，已经可以通过组合组件来组成应用程序，当你要把 Vue Router 添加进来，需要做的是，将组件 (components) 映射到路由 (routes)，然后告诉 Vue Router 在哪里渲染它们。

HTML:

```
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- 使用 router-link 组件来导航. -->
    <!-- 通过传入 `to` 属性指定链接. -->
    <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
```

JavaScript:
```
// 0. 如果使用模块化机制编程，导入Vue和VueRouter，要调用 Vue.use(VueRouter)

// 1. 定义 (路由) 组件。
// 可以从其他文件 import 进来
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }

// 2. 定义路由
// 每个路由应该映射一个组件。 其中"component" 可以是
// 通过 Vue.extend() 创建的组件构造器，
// 或者，只是一个组件配置对象。
// 我们晚点再讨论嵌套路由。
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]

// 3. 创建 router 实例，然后传 `routes` 配置
// 你还可以传别的配置参数, 不过先这么简单着吧。
const router = new VueRouter({
  routes // (缩写) 相当于 routes: routes
})

// 4. 创建和挂载根实例。
// 记得要通过 router 配置参数注入路由，
// 从而让整个应用都有路由功能
const app = new Vue({
  router
}).$mount('#app')

// 现在，应用已经启动了！
```

### 7、什么是编程式的导航

`<router-link :to="...">`是声明式的导航。除了使用 <router-link> 创建 a 标签来定义导航链接，还可以借助 router 的实例方法，通过编写代码来实现，即编程式的导航。

  在 Vue 实例内部，可以通过 $router 访问路由实例。此你可以调用 this.$router.push。

  想要导航到不同的 URL，则使用 router.push 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

当点击 <router-link> 时，这个方法会在内部调用，所以说，点击 <router-link :to="..."> 等同于调用 router.push(...)。

该方法的参数可以是一个字符串路径，或者一个描述地址的对象。例如：
```
// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: 123 }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```
注意：如果提供了 path，params 会被忽略，上述例子中的 query 并不属于这种情况。取而代之的是下面例子的做法，你需要提供路由的 name 或手写完整的带有参数的 path：
```
const userId = 123
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```
同样的规则也适用于 router-link 组件的 to 属性。

### 8、请详细说下你对vue生命周期的理解？
Vue实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模板、挂载Dom、渲染→更新→渲染、销毁等一系列过程，我们称这是Vue的生命周期。

Vue的生命周期简单说就是：就是Vue实例从创建到销毁的过程，就是生命周期。

每一个组件或者实例都会经历一个完整的生命周期。总共分为8个阶段实例初始化后，实例创建完成后，实例载入(挂载)前/后，数据更新，实例销毁前/后。

1、在实例初始化之后：实例、组件通过new Vue() 创建出来之后会初始化生命周期，然后就会执行beforeCreate钩子函数（实例初始化之后执行），此时创建组件、实例的选项还未挂载，因此无法访问methods，data,computed上的方法或数据，只是一个空壳，无法访问到数据和真实的dom，一般不做操作。在beforeCreate阶段，vue实例的挂载元素el和数据对象data都为undefined，值还未初始化。beforeCreate在实例初始化之后，数据观测(data observer) 和 event/watcher 事件配置之前被调用。
```
tip：

此时组件的选项还未挂载，因此无法访问methods，data,computed上的方法或数据
```

2、实例创建完成后：created在实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调（即初始化数据data，绑定事件：methods里面定义的方法）。然而，挂载阶段还没开始，$el 属性目前不可见。这个时候已经可以使用到数据，也可以更改数据,在这里更改数据不会触发updated函数，在这里可以在渲染前倒数第二次更改数据的机会，不会触发其他的钩子函数，一般可以在这里做初始数据的获取。这是一个常用的生命周期，因为你可以调用methods中的方法、改变data中的数据，并且修改可以通过vue的响应式绑定体现在页面上、获取computed中的计算属性等等。
```
tip:

通常我们可以在这里对实例进行预处理。
也有一些童鞋喜欢在这里发ajax请求，值得注意的是，这个周期中是没有什么方法来对实例化过程进行拦截的。
因此假如有某些数据必须获取才允许进入页面的话，并不适合在这个页面发请求。
建议在组件路由勾子beforeRouteEnter中来完成。
```

3、实例挂在前：接下来开始找实例或者组件对应的模板，编译模板为虚拟dom放入到render函数中准备渲染，然后执行beforeMount钩子函数，在这个函数中虚拟dom已经创建完成，马上就要渲染,在这里也可以更改数据，不会触发updated，在这里可以在渲染前最后一次更改数据的机会，不会触发其他的钩子函数，一般可以在这里做初始数据的获取。
```
tip：

beforeMount在挂载开始之前被调用：相关的 render 函数首次被调用。
```

4、实例挂在后：接下来开始render，渲染出真实dom，然后执行mounted钩子函数，此时，组件已经出现在页面中，数据、真实dom都已经处理好了,事件都已经挂载好了，可以在这里操作真实dom等事情...。
```
tip：

el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用mounted钩子函数。

1.在这个周期内，对data的改变可以生效。但是要进下一轮的dom更新，dom上的数据才会更新。
2.这个周期可以获取 dom。
3.beforeRouteEnter的next的勾子比mounted触发还要靠后。
4.指令的生效在mounted周期之前。
```

5、数据更新之后：当组件或实例的数据更改之后，会立即执行beforeUpdate，然后vue的虚拟dom机制会重新构建虚拟dom与上一次的虚拟dom树利用diff算法进行对比之后重新渲染，一般不做什么事儿。

```
tip：

当组件或实例的数据更改之后时调用，发生在虚拟 DOM 重新渲染和打补丁之前。你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。
```

6、数据当更新完成后：由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用updated钩子函数。当这个钩子被调用时，数据已经更改完成，dom也重新render完成，组件DOM已经更新，所以你现在可以执行依赖于DOM的操作。然而在大多数情况下，你应该避免在此期间更改状态，因为这可能会导致更新无限循环。

7、实例销毁之前：当经过某种途径调用$destroy方法后，立即执行beforeDestroy。在实例销毁之前调用，在这一步，实例仍然完全可用。一般在这里做一些善后工作，例如清除计时器、清除非指令绑定的事件等等。

8、实例销毁后：Vue实例销毁后调用destoryed钩子函数。调用后，Vue实例的所有东西都会被绑定，所有的事件监听器会被移除，所有的子实例也会被销毁，只剩下dom空壳。再重新改变数据的值，vue不再对此动作进行响应了。但是原先生成的dom元素还存在。执行了destroy操作，后续就不再受vue控制了。

### 9、路由之间跳转方法？

1、声明式(标签跳转)
```
<router-link :to="index">
```

2、编程式（ js跳转）
```
router.push('index')
```

### 10、什么是vue的路由懒加载？vue如何实现按需加载配合webpack设置?

“懒加载也叫延迟加载，即在需要的时候进行加载，随用随载。在单页应用中，如果没有应用懒加载，运用webpack打包后的文件将会异常的大，造成进入首页时，需要加载的内容过多，延时过长，不利于用户体验，而运用懒加载则可以将页面进行划分，需要的时候加载页面，可以有效的分担首页所承担的加载压力，减少首页加载用时。”

vue项目可以通过三种方式实现按需加载:

1、使用vue异步组件

vue-router配置路由时，使用vue的异步组件，可以实现按需加载。但是，这种情况下一个组件生成一个js文件。
```
{
    path: '/promisedemo',
    name: 'PromiseDemo',
    component: resolve => require(['../components/PromiseDemo'], resolve)
}
```
2、 使用es提案的import()+webpack

vue-router配置路由时，在import组件的时候，把要打包成一个文件的组件指定相同的webpackChunkName
```
// 下面2行代码，没有指定webpackChunkName，每个组件打包成一个js文件。
const ImportFuncDemo1 = () => import('../components/ImportFuncDemo1')
const ImportFuncDemo2 = () => import('../components/ImportFuncDemo2')
// 下面2行代码，指定了相同的webpackChunkName，会合并打包成一个js文件。
// const ImportFuncDemo = () => import(/* webpackChunkName: 'ImportFuncDemo' */ '../components/ImportFuncDemo')
// const ImportFuncDemo2 = () => import(/* webpackChunkName: 'ImportFuncDemo' */ '../components/ImportFuncDemo2')
export default new Router({
    routes: [
        {
            path: '/importfuncdemo1',
            name: 'ImportFuncDemo1',
            component: ImportFuncDemo1
        },
        {
            path: '/importfuncdemo2',
            name: 'ImportFuncDemo2',
            component: ImportFuncDemo2
        }
    ]
})
```

3、使用wenpack提供的require.ensure()

vue-router配置路由时，使用webpack的require.ensure技术，也可以实现按需加载。

这种情况下，多个路由指定相同的chunkName，会合并打包成一个js文件。
```
{
    path: '/promisedemo',
    name: 'PromiseDemo',
    component: resolve => require.ensure([], () => resolve(require('../components/PromiseDemo')), 'demo')
},
{
    path: '/hello',
    name: 'Hello',
    // component: Hello
    component: resolve => require.ensure([], () => resolve(require('../components/Hello')), 'demo')
}
```

### 11、数据驱动

Vue.js 一个核心思想是数据驱动。所谓数据驱动，是指视图是由数据驱动生成的，我们对视图的修改，不会直接操作 DOM，而是通过修改数据。它相比我们传统的前端开发，如使用 jQuery 等前端库直接修改 DOM，大大简化了代码量。特别是当交互复杂的时候，只关心数据的修改会让代码的逻辑变的非常清晰，因为 DOM 变成了数据的映射，我们所有的逻辑都是对数据的修改，而不用碰触 DOM，这样的代码非常利于维护。

### 12、说一下Vue的Virtual DOM

Virtual DOM 它产生的前提是浏览器中的 DOM 被设计的很复杂，有非常多的属性。当我们频繁的去做 DOM 更新，会产生一定的性能问题。

因此虚拟dom应运而生，其实 VNode 是对真实 DOM 的一种抽象描述，只是用来映射到真实 DOM 的渲染，不需要包含操作 DOM 的方法，因此它是非常轻量和简单的。可以对这颗抽象树进行创建节点、删除节点以及修改节点等操作，在这过程中都不需要操作真实DOM，Vue就使用了这样的抽象节点VNode，它是对真实DOM的一层抽象，而不依赖某个平台，它可以是浏览器平台，也可以是weex，甚至是node平台也可以对这样一棵抽象DOM树进行创建删除修改等操作，这也为前后端同构提供了可能。

Virtual DOM 除了它的数据结构的定义，映射到真实的 DOM 实际上要经历 VNode 的 create、diff、patch 等过程。

Vue通过数据绑定来修改视图，当某个数据被修改的时候，会先比较前后两次相应虚拟dom上的数据，如果需要改变，才会将改变映射到真实dom上。

在vue中有一个方法-patch。

patch将新老VNode节点进行比对，然后将根据两者的比较结果进行最小单位地修改视图，而不是将整个视图根据新的VNode重绘。patch的核心在于diff算法，这套算法可以高效地比较virtual DOM的变更，得出变化以修改视图。


### xx、vuex是什么？怎么使用？哪种功能场景使用它？
