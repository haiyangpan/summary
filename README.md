# JavaScript部分

## 原生JS

## ES6

1. promise与async/await的区别

   1. promise 采用了回调函数延迟绑定技术，执行resolve函数时，回调函数还没有绑定，推迟回调函数执行。promise管理异步编程的，本身不是异步的。

      [你真的懂promise吗？](https://segmentfault.com/a/1190000022427416)

      promise很好的解决了回调地狱问题，但是语义化不明显，代码不能很好的表示执行流程。

   2. async/await的实现基于promise，返回promise对象，是generator的语法糖。

      ### 优点：

      1. 语法简洁，更像是同步代码，也更符合普通的阅读习惯；
      2. 改进JS中异步操作串行执行的代码组织方式，减少callback的嵌套；
      3. Promise中不能自定义使用try/catch进行错误捕获，但是在Async/await中可以像处理同步代码处理错误。

      

      

2. Generator 函数的使用

3. 宏任务、微任务

   1.  异步任务分为宏任务（macroTask）和微任务（microTask）。
   2.  当满足执行条件时，task和microtask会被放入各自的队列中等待放入执行线程执行，我们把这两个队列称为Task Queue(也叫Macrotask Queue)和Microtask Queue。
       1. task：script中代码、setTimeout、setInterval、I/O、UI render
       2. microtask: promise、Object.observe、MutationObserver。
   
   - 基于微任务的技术有 MutationObserver、Promise 以及以 Promise 为基础开发出来的很多其他的技术，本题中、await fn()都是微任务。
   
   - 不管宏任务是否到达时间，以及放置的先后顺序，每次主线程执行栈为空的时候，引擎会优先处理微任务队列，**处理完微任务队列里的所有任务**，再去处理宏任务。
   
     #### 事件循环：同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入Event Table并注册函数。当指定的事情完成时，Event Table会将这个函数移入Event Queue。主线程内的任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行。上述过程会不断重复，也就是常说的Event Loop(事件循环)
   
     
   
     ![事件循环](https://github.com/haiyangpan/summary/tree/main/image/eventLoop.png)
   
     宏任务一般是：包括整体代码script，setTimeout，setInterval。
   
     微任务：Promise中then，process.nextTick
   
     [宏任务、微任务](https://juejin.im/post/6844903512845860872)
   
     **运行process.nextTick时需要注意node的版本，v11与浏览器趋于相同。**
   
     

# 框架部分

## VUE框架

1. 常用的一些指令和命令修饰符

   https://blog.csdn.net/weixin_43555789/article/details/97023643

2. v-model原理

   https://www.jianshu.com/p/0d089f770ab2

3. vue组件间通信方式（父子、兄弟、跨级）

   https://segmentfault.com/a/1190000019208626

4. vuex单一数据流的理解

   https://www.jianshu.com/p/80a82e3b6e6d

5. slot和scope slot的使用场景

   https://blog.csdn.net/fang562878311/article/details/100579007

   slot分为默认插槽、具名插槽。slot作为承载分发内容的出口。

   **scope slot**为作用域插槽，主要解决父组件不能直接访问子组件定义的data数据。

6. 生命周期钩子函数，每个周期处理的业务逻辑

   https://segmentfault.com/a/1190000008010666

   ![生命周期钩子](https://github.com/haiyangpan/summary/tree/main/image/lifecycle.png)

   | 钩子          | 逻辑                                                         |
   | ------------- | :----------------------------------------------------------- |
   | beforeCreate  | 在数据观测和初始化事件还未开始,data、watcher、methods都还不存在，但是$route已存在，可以根据路由信息进行重定向等操作 |
   | created       | 在实例创建之后被调用，该阶段可以访问data，使用watcher、events、methods，也就是说 数据观测(data observer) 和event/watcher 事件配置 已完成。但是此时dom还没有被挂载。该阶段允许执行http请求操作 |
   | beforeMount   | 将HTML解析生成AST节点，再根据AST节点动态生成渲染函数。相关render函数首次被调用(**重点**) |
   | mounted       | 在挂载完成之后被调用，执行render函数生成虚拟dom，创建真实dom替换虚拟dom，并挂载到实例。可以操作dom，比如事件监听 |
   | beforeUpdate  | *vm*.data更新之后，虚拟*d**o**m*重新渲染之前被调用。在这个钩子可以修改vm.data，并不会触发附加的冲渲染过程 |
   | updated       | 虚拟dom重新渲染后调用，若再次修改$vm.data，会再次触发beforeUpdate、updated，进入死循环 |
   | beforeDestroy | 实例被销毁前调用，也就是说在这个阶段还是可以调用实例的       |
   | destroyed     | 实例被销毁后调用，所有的事件监听器已被移除，子实例被销毁     |

   异步请求在哪个阶段都可以调用，因为会先执行完生命周期的钩子函数之后，才会执行异步函数，但如果考虑用户体验方面的话，在created中调用异步请求最佳，用户就越早感知页面的已加载，毕竟越早获取数据，在mounted实例挂载的时候就越及时

7. mixin filter自定义指令。编写一个自定义指令。

   [mixin](https://www.jianshu.com/p/61317558d11d)  [filter](https://www.cnblogs.com/wujiaofen/p/11236176.html)

   **1、 当有局部和全局两个名称相同的过滤器时候，会以就近原则进行调用，即：局部过滤器优先于全局过滤器被调用！**

   **2、 一个表达式可以使用多个过滤器。过滤器之间需要用管道符“|”隔开。其执行顺序从左往右。**

   [自定义指令](https://www.jianshu.com/p/c5de3aa0c465)

8. 合理使用mixin，有哪些缺点？

   mixin封装一小段想要复用的代码是有用的，但是容易造成滥用（由于不需要传递状态的特性）。此外，**使用Vue mixins时方法和参数是不共享的**。命令冲突。隐含的依赖关系（mixin和使用它的组件之间没有层次关系。组件里的变量名称修改后，mixin里没改）。

   **缺点**：

   	1. 不传递状态的原因，容易造成mixin的滥用；
    	2. 使用mixin时方法和参数不共享，造成命名冲突；
    	3. mixin与组件间的隐式依赖，容易造成变量修改出现错误。

   由于这些缺点，所以有了[composition API](https://blog.csdn.net/duninet/article/details/105716649?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.not_use_machine_learn_pai&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.not_use_machine_learn_pai)（受 React Hooks 的启发）。

9. vue3新的开发模式setup。vue3中的reactive api的使用

   [官方RFC](https://composition-api.vuejs.org/zh/)

10. vue.component和vue.extend区别以及使用场景

    Vue.extend返回的是一个“扩展实例构造器”，预设了部分选项的Vue实例构造器。参数是一个包含组件选项的对象。

    使用vue.component进行实例化、或使用`new extendName().$mount($el)`方式进行实例化。

    ```javascript
    let Hello = Vue.extend({
    	template: `<div>{{name}}</div>`,
    	data () {
    		return {
    			name: 'hello world'
    		}
    	}
    })
    new Hello().$mount('#app')
    ```

    **Vue.component**  用于全局注册组件

    ```javascript
    // 注册组件，传入一个扩展过的构造器
    Vue.component('my-component', Vue.extend({ /* ... */ }))
    // 注册组件，传入一个选项对象 (自动调用 Vue.extend)
    Vue.component('my-component', { /* ... */ })
    // 获取注册的组件 (始终返回构造器)
    var MyComponent = Vue.component('my-component')
    ```

11. computed、watch和methods区别

    **computed属性**：数据依赖其他数据进行变动时，使用computed。可以监控到数组与对象的变化的。

    data中没有相应的属性，也可以自定义computed属性。

    适用场景：一个数据受多个数据影响。computed是有缓存的，如果依赖的数据没有发生变化，会直接从缓存中取值。

    适用场景：![computed适用场景](https://github.com/haiyangpan/summary/tree/main/image/computed.png)

    

    **watch**：用于观察和监听页面上的vue实例。

    数据变化的同时，进行异步操作或者比较大的开销，适合watch。watch为一个对象，键是需要观察的表达式，值是对应回调函数。值也可以是方法名，或者包含选项的对象。

    data和computed中没有相应的属性，不能使用watch。

    适用场景：![watch适用场景](https://github.com/haiyangpan/summary/tree/main/image/watch.png)

    **methods**：方法只要调用，就会执行一次，需要特定的触发条件。不会缓存。

12. template render函数和jsx的使用

13. data、props区别，合理划分两者界限

14. name属性具体应用

15. keep alive作用

16. vue项目实现动画的几种方式

17. key属性作用

18. vue router有几种路由方式

19. 路由鉴权、路由守卫

20. vuex划分模块

21. vue.use使用

22. 如何封装Vue组件

23. vue.config.js的使用

24. axios使用和请求响应拦截器

25. 常用的vue库v-lazy、element、antdesign-vue、vant、iview

26. webpack知识，常用的loader、plugin

27. css预处理器less、scss、stylus

28. ssr服务端渲染和pwa相关知识

29. 单元测试jest mocha+chai vue-test-unit使用

30. vue源码和其他框架的对比

31. MVVM框架，Vue是吗？

32. vue组件初渲染做了哪些事情

33. 响应式的原理，vue2和vue3的原理

34. nextTick的原理

35. mergeOption函数

## React框架

1. 单项数据流
2. redux/react-redux
3. 有状态组件和无状态组件的差别
4. hooks
5. 生命周期钩子
6. react性能优化（如何避免子组件频繁渲染）
7. react-router/react-route
8. react虚拟dom的理解以及工作原理
9. diff算法
10. jsx语法
11. 声明式编程、命令式编程、函数式编程
12. 错误边界
13. context的理解
14. react和vue对比差异、优缺点
15. 在react和vue中如何选择使用项目开发的框架

[React面试知识点](https://segmentfault.com/a/1190000019339210)
