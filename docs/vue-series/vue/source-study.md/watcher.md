# watcher

## 原理解析

### 深度监听 deep: true

只要`watcher`某一个依赖的子孙属性发生变化，就会触发`watcher`重新计算表达式，并执行监听的回调函数。

### 哪些情况下 watcher 的回调函数会执行？

通过学习源码，我们发现在以下三种情况下会执行回调函数：

- 所监听的表达式的返回值`value`改变了（原始值改变，或者是对象的引用改变）
- 所监听的表达式的返回值`value`是对象或数组（即使`value`的引用没改变）
- 深度监听表达式时。

```js
export default class Watcher {
  /**
   * Scheduler job interface.
   * Will be called by the scheduler.
   * 依赖改变时，最终会调用该函数
   */
  run () {
    if (this.active) {
      const value = this.get()
      if (
        // 以下三种情况需要调用监听回调函数：
        // 1、表达式的返回值 value 改变了（原始值，或者是对象的引用）
        // 2、表达式的返回值 value 是对象或数组（即使 value 的引用没改变）
        // 3、深度监听的，不管最终的返回值是否改变，都要执行回调。（因为 watcher 依赖的 dep 的子孙属性改变了）
        value !== this.value ||
        // Deep watchers and watchers on Object/Arrays should fire even
        // when the value is the same, because the value may
        // have mutated.
        isObject(value) ||
        this.deep
      ) {
        // set new value
        const oldValue = this.value
        this.value = value
        if (this.user) {
          try {
            this.cb.call(this.vm, value, oldValue)
          } catch (e) {
            handleError(e, this.vm, `callback for watcher "${this.expression}"`)
          }
        } else {
          this.cb.call(this.vm, value, oldValue)
        }
      }
    }
  }
  // ...
}
```

针对于第一点，我们比较好理解，毕竟返回的值都改变了，肯定是要执行回调函数的。

但是针对第二点，表达式的返回值时对象或数组时，即使引用没改变，为什么要执行回调函数呢？按照源码的英文注释，即使返回的是同一引用，但是引用的子孙属性可能发生变化了。我猜想，因为返回的对象不一定是响应式的，我们无法知道（或者是比较难知道？）引用的子孙属性发生变化，因此统一处理成执行回调函数。我们来验证一下。

```js
const OBJECT = {}
export default {
  name: 'App',
  data: function () {
    return {
      a: 1
    }
  },
  created () {
    this.$watch(() => {
      const a = this.a
      return OBJECT
    }, function () {
      console.log('$watch 回调执行啦，尽管 OBJECT 没改变！')
    })
    this.a = 2
  }
}
```

结果打印：`$watch 回调执行啦，尽管 OBJECT 没改变！`

我们可以看到，监听的表达式的返回值时一常量对象`OBJECT`，我们并没有改变它，但是当我们改变表达式的依赖`data.a`时，回调函数居然执行了！（其实上，表达式并没有实际依赖`data.a`，只是在计算表达式的值时，会获取`data.a`的值，导致`data.a`成了表达式的依赖项了）