# libra

## Question 1
v-if和v-for哪个优先级高？如果两个同时出现，应该怎么优化得到更好的性能？

### Answer: 
1、v-for比v-if具有更高的优先;

2、v-for会先被执行生成虚拟节点，后再执行v-if判断渲染与否，例如经过如下运算：
```
this.list.map(function (item) {
  if (item.isActive) {
    return item.content
  }
})
```

3、在使用上存在两种情况：
- 为了过滤一个列表中的项目
- 为了避免渲染本应该被隐藏的列表

4、针对以上使用存在两种情况：
- 使用计算属性过滤出要渲染的列表的项目，生成一个新列表数组；
- 使用外层包裹元素进行v-if判断整个列表渲染；

## Question 2 
Vue组件data选项为什么必须是个函数而Vue的根实例则没有此限制？

### Answer: 
1、首先，Javascipt里Object是引用数据类型，如果不用function 返回，每个组件的data 都是内存的同一个地址，一个数据改变了其他也改变了;

2、其二，在Vue中，一个组件的 data 选项必须是一个函数，以保持每个实例可以维护一份被返回对象的独立的拷贝，因为组件可能被用来创建多个实例;

3、假设 data 是一个纯粹的对象，则所有的实例将共享引用同一个数据对象；

4、data作为函数在每次创建一个新实例后，能够调用 data 函数，从而返回初始数据的一个全新副本数据对象；

5、最后，使用Vue的根实例时，本身就只有一个实例，是定义在实例中的data属性，所以在Vue的根实例上可以直接使用对象的方式；
```
new Vue({
  el: '#app',
  data: {

  }
})
```

6、补充，Vue.extend() 中 data 也必须是函数；

## Question 3
你知道vue中key的作用和工作原理吗？说说你对它的理解。

### Answer: 
1、作用：
    key的作用主要是为了高效的更新虚拟DOM

2、工作原理：
    在渲染真实dom之前都会生成虚拟dom，当某个节点数据改变时，对比新旧虚拟dom的节点；
    按照深度优先，同层比较的原则，在patch 函数中执行，判断两个节点是否相同可直接，如果设置了key，就会用key进行比较。

## Question 4
你怎么理解vue中的diff算法？

### Answer: 
1、首先，什么是diff算法。在vue数据响应中每一次数据更新要直接操作真实dom开销很大，为了解决和优化这个问题，会先生成虚拟dom，每次数据改变对比新旧虚拟dom再根据对比结果操作真实dom，而diff算法就是对比新旧两个虚拟dom是否相等；

2、使用diff算法可以优化性能减少开销，跨平台，兼容性等优势；

3、diff算法在vue源码中的patch函数实现，对比新旧虚拟dom的vnode节点；

4、diff算法的比较原则是深度优先，同级比较。

## Question 5
谈一谈对vue组件化的理解？

### Answer: 
1、它提供一种抽象，使之可以使用独立可复用的组件来构建大型应用；

2、组件化是vue的核心思想，就是把页面拆分成多个组件，每个组件资源独立、可复用，并且组件与组件之间可以嵌套；

3、组件也分为页面组件、基础组件以及功能组件；

4、组件包含prop属性、自定义事件、slot扩展插槽等三要素；

5、一个组件的生成主要为：
    创建组件构造函数，继承于Vue
    挂载组件钩子函数，在patch流程中触发
    实例化组件vnode

6、组件化优势，在于能提高开发效率，方便重复使用，简化调试步骤，提升项目可维护 性，便于多人协同开发。

## Question 6
谈一谈对vue的设计原则的理解？

### Answer:
1、用组件化的思想构建应用；

2、合理细分颗粒化；

3、场景化，按照不同状态封装组件；


## Question 7
vue为什么要求组件模板只能有一个根元素？

### Answer:
1、组件的template最终会转换成VNode对象，一个组件对应一个根元素对应一个VNode对象

2、从效率上，如果多个根，那么就会产生多个入口（遍历、查找）从效率上来说都不方便

3、同时也取决于diff算法，单个匹配，深度优先，同级比较；

## Question 8
谈谈你对MVC、MVP和MVVM的理解？

### Answer:
