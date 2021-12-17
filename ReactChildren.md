# React.Children
children是react封装的用于处理子元素的一系列方法，在入口文件中我们可以看到
>const Children = {
  map,
  forEach,
  count,
  toArray,
  only,
};
我们可以得知children中包含的几种方法
children源文件地址packages/react/src/ReactChildren.js

1. children 的核心代码其实是mapIntoArray 这个函数，由于react dom熟是通过dom.props.children 来指引关系的，其实是一个树状链表，处理起来比较麻烦，因此就需要一个函数讲数据扁平化，mapIntoArray 就是处理这件事的 

```const SEPARATOR = '.'; // 第一次执行时， nameSoFar 为空，使用这个作为key的一部分传入  
const SUBSEPARATOR = ':';

function mapIntoArray( children: ?ReactNodeList, array: Array<React$Node>, escapedPrefix: string, nameSoFar: string, callback: (?React$Node) => ReactNodeList,): number {  
    // 判断传入的children是什么类型， 如果是 `undefined` || 'boolean' 处理为 null  
    const type = typeof children;  
    if (type === 'undefined' || type === 'boolean') {  
    children = null;  
    }  
    // 是否调用callback函数  
    let invokeCallback = false;  
    // 判断children是什么类型，当children为null或者非数组, 直接调用函数。  
    if (children === null) {  
    invokeCallback = true;  
    } else {  
    switch (type) {  
        case 'string':  
        case 'number':  
        invokeCallback = true;  
        break;  
        case 'object':  
        switch ((children: any).?typeof) {  
            case REACT_ELEMENT_TYPE:  
            case REACT_PORTAL_TYPE:  
            invokeCallback = true;  
        }  
    }  
    }  
    if (invokeCallback) {  
    // 调用callback  
    const child = children;  
    let mappedChild = callback(child);  
    // 深度遍历，第一次执行时，nameSoFar为空，SEPARATOR作为一部分传入。第一次生成 key为 `.0`   
    const childKey = nameSoFar === '' ? SEPARATOR + getElementKey(child, 0) : nameSoFar;  
    // 如果执行后的数据是一个数组，则递归调用mapIntoArray，将数组变成一维数组，放在 `array` 中返回，也就是 map函数中的 result 变量  
    if (Array.isArray(mappedChild)) {     
      // 生成child 的 escapedPrefix， 第一次生成 `.0/`  
      let escapedChildKey = '';  
      if (childKey != null) {  
        escapedChildKey = escapeUserProvidedKey(childKey) + '/';   
      }  
      mapIntoArray(mappedChild, array, escapedChildKey, '', c => c);  
    } else if (mappedChild != null) {  
      // 如果是有效的element,使用新的keyclone一个对象，放入array对象中，作为最终map函数返回的结果。  
      if (isValidElement(mappedChild)) {  
        mappedChild =   
        cloneAndReplaceKey(mappedChild, 
            escapedPrefix + (mappedChild.key && (!child || child.key !== mappedChild.key) ?  
             escapeUserProvidedKey('' + mappedChild.key) + '/'  : '') + childKey,);
      }
      array.push(mappedChild);
    }
    return 1;
  }

 // 递归调用mapIntoArray，无论你嵌套多少层数组，都将被展示位一个平铺数组。  
  let child;  
  let nextName;  
  let subtreeCount = 0; // 当前树种有多少个子节点  
  const nextNamePrefix = nameSoFar === '' ? SEPARATOR : nameSoFar + SUBSEPARATOR;  

  if (Array.isArray(children)) {  
    for (let i = 0; i < children.length; i++) {  
      child = children[i];  
      nextName = nextNamePrefix + getElementKey(child, i);  
      subtreeCount += mapIntoArray(  
        child,  
        array,  
        escapedPrefix,  
        nextName,  
        callback,  
      );  
    }  
  } else {  
      // 如果是可迭代的数据还是还是按照 数组方式 进行深度遍历  
      ....  
  }  
  return subtreeCount;  
}
```