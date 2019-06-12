
1. React 中 keys 的作用是什么？
  * key 是 React 用来追踪列表中哪些元素被修改、被添加或者被移除的辅助标识。
  * 需要保证列表中某个元素的key在同级元素中具有唯一性。React Diff 算法会借助元素的key值来判断该元素是新创建的还是移动来的元素，从而减少不必要的元素重渲染。

2. 调用 setState 之后发生了什么？
  * 调用setState函数之后，React会将传入的参数对象与组件当前的状态合并；然后根据新的状态构建新的 React Dom 树，计算前后两次虚拟DOM的差异，并根据差异重新渲染UI

3. React 生命周期函数
  * 初始渲染阶段
    * getDerivedStateFromProps()
    * render()
    * componentDidMount()

  * 更新阶段
    * getDerivedStateFromProps()
    * shouldComponentUpdate()
    * render()
    * componentDidUpdate()

  * 卸载阶段
    * componentWillMount()

4. shouldComponentUpdate() 是做什么的？
  * 用来判断是否需要调用render方法重新绘制DOM

5. 为什么虚拟DOM会提高性能？
  * 虚拟DOM是用来描述真实DOM的一个JS对象
  * 相当于在 js 和真实DOM中间加了一个缓存，在前后两个虚拟DOM上比对差异比直接比对真实DOM的差异性能更好; 只将虚拟DOM的差异应用到真实DOM上，减少了大量不必要的渲染

6. react diff 算法原理
  * 把树形结构按照层级分解，只比较同级元素
  * 给列表结构的每个单元添加唯一的 key 属性，方便比较
  * React 只会匹配相同class的component (class 指的是组件的名字)
  * 合并操作，调用component的setState方法的时候，react 会将该组件标记为dirty，到一个事件循环结束，React会检查所有标记为dirty的component重新绘制
  * 可以重写shouldComponentUpdate() 提到diff的性能

7. React 中 refs 的作用是什么？
  * 通过refs，我们可以灵活访问某个DOM元素

8. 展示组件(UI组件)和容器组件之间有何不同？
  * 展示组件专门通过props接收数据和回调，并且几乎不会有自身的状态，即使有状态，通常也只是UI相关而不是数据相关的状态
  * 容器组件则会为展示组件或其他容器组件提供数据和行为。容器组件经常是有状态的，因为它们是(其他组件的)数据源
  * 展示组件负责渲染，容器组件负责管理数据和逻辑

9. 类组件和函数式组件之间有何不同？
  * 类组件不仅允许使用更多额外的功能，比如组件自身的状态和生命周期钩子，也能使组件直接访问store并维持状态
  * 当组件仅是接收props，并将组件自身渲染到页面时，该组件就是一个无状态组件，可以使用一个纯函数来创建这样的组件。这种组件也被称为哑组件或展示组件

10. 组件的状态 (state) 和 属性 (props) 之间有何不同？
  * state 是一种数据结构，用于组件挂载时所需数据的默认值。state可能会随着时间的推移而发生变化，但多数时候是作为用户时间行为的结果。
  * props 则是组件的配置。props 由父组件传递给子组件，并且就子组件而言，props是不可变的。组件不能改变自身的props，但是可以把其子组件的props统一管理。props也不仅仅是数据，回调函数也可以通过props传递

11. 何为受控组件？
  * 针对表单元素，state 是其"唯一数据源" (即表单元素的值只能由state控制，state的更新通过onChange事件触发)

12. 何为高阶组件？
  * 高阶组件是一个以组件为参数并返回一个新组件的函数。比如 Redux 的 connect 函数。高阶组件的优点在于复用组件逻辑

13. 为什么建议传递给setState 的参数是一个callback而不是一个对象？
  * 因为 setState 导致的state更新可能是异步的，此时不能依赖它们的值去计算下一个state

14. 在 constructor中调用 super(props) 的目的是什么？
  * 在 super() 被调用之前，子类是不能使用this的，在ES2015中，子类必须在constructor中调用super()。传递props给super()的原因则是便于在子类中能在constructor访问this.props

15. 发送网络请求应该在哪一阶段？
  * componentDidMount(); 保证数据的正确加载，比如此时可以正确调用setState

16. 事件在React中的处理方式
  * react没有将事件附加到子节点本身，而是采用合成事件，用单个事件监听器监听顶层的所有事件，提供一个统一的API接口，解决了不同浏览器间的兼容性问题

17. React 中三种构建组件的方式
  * React.createClass() / ES6 class / 函数式组件

18. Redux 
  * redux 是一个应用数据流框架，主要是解决了组件间状态共享的问题，原理是集中式管理，主要有三个核心方法：action / store / reducer ; 
  * 工作流程：view(视图) 调用store的dispatch接收action，传入store, reducer进行state更新，view通过store提供的getState获取最新的数据