---
title: react 入门
date: 2017-06-16 11:33:27
tags: React
---
## React 

## React diff
1. diff 策略
* 策略1：Web UI中DOM节点跨层级的移动操作特别少，可以忽略不计
 对树进行分层比较，两棵树只对同一层级的节点进行比较
 如果发现节点不存在，该节点及其子节点会被完全删除掉。

* 策略2：拥有相同类的两个组件会生成相似的树形结构，拥有不同类的两个组件将生成不同的树形结构
如果是同一类型的组件，按照原策略继续进行比较Virtual DOM
如果不是，则该组件判断为dirtry DOM,从而替换整个组件树下的所有子节点
如果是同一类型的组件，有可能Virtual DOM没有任何变化，如果确切的知道这点，可以节省大量diff的时间。因此，React允许用户通过shouldComponentUpate()来判断用户是否需要运行diff算法分析

策略3：对于同一层级的一组子节点，可以通过唯一的id来区分
当节点处于同一层级时，diff提供了3中节点操作，
React提供优化策略：允许开发者对同一个层次的子节点，添加唯一的key进行区分。


## React 生命周期
| First Render       | Unmount              | Prop change               | StateChange           |
|--------------------|----------------------|---------------------------|-----------------------|
| getDefaultProps    | componentWillUnmount | componentWillReceiveProps | shouldComponentUpdate |
| getInitialState    |                      | shouldComponentUpate      | componentWillUpdate   |
| componentWillMount |                      | componentWillUpdate       | render                |
| render             |                      | render                    | componentDidUpdate    |
| componentDidMount  |                      | componentDidUpdate        |                       |
## React 合成事件

## React setState
    * React 是通过管理状态来实现对组件的管理，React是通过this.state来访问state， 通过
    this.setState方法来更新state，当this.setState调用是，React会重新render渲染UI。
    setState通过一个队列机制实现state更新，当执行setState是，会将需要更新的state合并后放入状态队列，不会立即更新，队列的机制可以高效地批量更新state.
    * React setState(updater, callback)
    callback的执行时机, render后执行
```
componentDidMount() {
    this.setState({
      val: this.state.val +100,
    })

    this.setState({
      val: this.state.val +1000,
    })

    this.setState({
      val: this.state.val +10,
    })

    setTimeout(() => {
      this.setState({
        val: this.state.val +1,
      })
      this.setState({
        val: this.state.val +1,
      })
    })
}
```


* React state和props区别
需要理解的是，props是一个父组件传递给子组件的数据流，这个数据流可以一直传递到子孙组件。而state代表的是一个组件内部自身的状态（可以是父组件、子孙组件）。
改变一个组件自身状态，从语义上来说，就是这个组件内部已经发生变化，有可能需要对此组件以及组件所包含的子孙组件进行重渲染。
而props是父组件传递的参数，可以被用于显示内容，或者用于此组件自身状态的设置（部分props可以用来设置组件的state），不仅仅是组件内部state改变才会导致重渲染，父组件传递的props发生变化，也会执行。
既然两者的变化都有可能导致组件重渲染，所以只有理解pros与state的意义，才能很好地决定到底什么时候用props或state。

官方指导有说，props放初始化数据，一直不变的，state就是放要变的。
State 应该包括那些可能被组件的事件处理器改变并触发用户界面更新的数据,因为组件本身不能修改自己的 props。



