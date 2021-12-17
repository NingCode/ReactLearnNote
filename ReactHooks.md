# Hooks

前言：Hooks改变了react依赖class类开发的生态，赋予了函数组件状态，逐渐成为了react开发的主流写法。过去函数组件只能作为pureComponent。

```function useState(initialState) {  
  let hook;  
  //检测是否是第一次加载  
  if (isMount) {  
    hook = {  
      queue: {  
        pending: null  
      },  
      memoizedState: initialState,  
      next: null  
    }  
    if (!fiber.memoizedState) {  
      fiber.memoizedState = hook;  
    } else {  
      workInProgressHook.next = hook;  
    }  
    workInProgressHook = hook;  
  } else {  
    hook = workInProgressHook;  
    workInProgressHook = workInProgressHook.next;  
  }  
  let baseState = hook.memoizedState;  
  if (hook.queue.pending) {  
    let firstUpdate = hook.queue.pending.next;  
    do {  
      const action = firstUpdate.action;  
      baseState = action(baseState);  
      firstUpdate = firstUpdate.next;  
    } while (firstUpdate !== hook.queue.pending.next)  
    hook.queue.pending = null;  
  }  
  hook.memoizedState = baseState;  
  return [baseState, dispatchAction.bind(null, hook.queue)];  
}
```