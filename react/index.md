# 书籍
- react设计原理 卡颂
# 运行时和编译时的区别 AOT（构建时编译/预编译）和JIT（即时编译）的区别
- 运行时更像是命令式编程，类似vue提供的h函数，让我们直接生成对应的vnode
- 编译时更像是的声明式编程，内部也是调用命令式实现的。类似于我们直接写template，内部会转换为调用h函数的结果
- AOT是预先编译 像vue的template，在运行之前就可以根据模版进行编译，可以进行很多优化，对内容进行分析，区分静态内容和动态内容，而且可以做依赖的收集，这样后续更新只需要找到对应动态节点进行更新
- JIT是即时编译，也就是运行时编译，就像jsx，函数在执行之前永远不知道他返回的是什么东西，所以无法像AOT那样做静态编译的优化，所以react才需要fiber，因为他默认更新机制就是父组件更新，下面的子组件都需要全量更新，但是他也提供了很多方法，让开发者自己做优化，而vue就是在内部帮我们把优化做好了。

# 合成事件 16和17版本的区别
合成事件只是给元素加了onClick属性，实际的事件监听是放在根元素上，当事件冒泡到根元素，逐层去找到各个元素的onClick属性执行。  
 - 16中将合成事件委托到document上，17委托到root元素
 - 16只委托了冒泡阶段 17冒泡和捕获阶段都委托
 - 拿到的event是合成事件的event,e.nativeEvent才是真正的事件对象
 - 16版本问题 主要是如果页面上有多个react版本，事件都委托到document，那么stopPropagation无法阻止其他react中的事件冒泡因为已经都到达了document元素 

#  为什么react需要fiber&时间分片而vue没有
- react组件更新的时候，默认是全量更新，父组件一旦更新，所有子组件都要更新，所以diff阶段会花费很多时间，有大概率会造成页面卡顿，所以需要时间切片
- vue因为使用template模版，可以做静态分析，更新的时候只会更新需要的组件，所以diff的节点会比较少，用到切片的概率很小，作者认为vue没有太大必要引入时间切片，收益不大
- (https://github.com/vuejs/rfcs/issues/89)
#  组件渲染优化
 -  function组件可以使用react.memo进行缓存，如果props没变化组件不会重新渲染，类似于class组件的pureComponent，都是采用object.is来比较新老props
  
# createPortal与vue的teleport相似，可以将节点插入到任意为止

# react为什么要废弃三个生命周期
- componentWillMount componentWillUpdate componentsWillRecieveProps
- 这三个生命周期都是在render阶段执行的，因为fiber架构的出现，导致render的时候是可以中断的，当一个任务执行到一半被打断后，下一次渲染线程抢回主动权时，这个任务被重启的形式是“**重复执行一遍整个任务”而非“接着上次执行到的那行代码往下走**”。这就导致 render 阶段的生命周期都是有可能被重复执行的

# react和vue的异同点
  相同
  - 都使用虚拟dom，并且都支持跨平台开发
  - 都是组件化思想
  - 都是响应式的

不同点
 - react是MVC框架，单向数据流，vue是MVVM框架，双向数据流，实现了双向绑定
 - 事件处理不同，react使用事件委托进行事件监听， vue直接事件绑定
 - react使用jsx语法，vue使用模板语法，因此vue在构建阶段就能做静态分析，优化diff算法部分，而jsx由于得函数执行后才知道返回什么，所以这方面优化程度有限，因此才有了fiber架构
 - vue因为做了依赖收集，所以更新的颗粒度更小，可以限制到某个组件，而react父组件更新，如果不做任何优化的情况下，下面所有子组件都需要全量更新，vue相当于帮我们做了shouldComponentUpdate方法

# react闭包陷阱怎么处理
```js
const FunctionComponent = () => {
  const [value, setValue] = useState(1)

  const log = () => {
    setTimeout(() => {
      alert(value)
    }, 3000);
  }

  return (
    <div>
      <p>FunctionComponent</p>
      <div>value: {value}</div>
      <button onClick={log}>alert</button>
      <button onClick={() => setValue(value + 1)}>add</button>
    </div>
  )
}
```
使用useRef储存，因为ref的赋值是同步的
```js
const FunctionComponent = () => {
  const [value, setValue] = useState(1)
  const countRef = useRef(value)

  const log = () => {
    setTimeout(() => {
      alert(countRef.current)
    }, 3000);
  }

  useEffect(() => {
    countRef.current = value
  }, [value])

  return (
    <div>
      <p>FunctionComponent</p>
      <div>value: {value}</div>
      <button onClick={log}>alert</button>
      <button onClick={() => setValue(value + 1)}>add</button>
    </div>
  )
}

```

# 组件懒加载
- 使用react.lazy，vue使用defineAsyncComponent
  