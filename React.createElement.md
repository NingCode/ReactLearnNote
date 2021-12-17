# ReactElement解读
reactElement是由JSX编译而来的，通过参考文件：packages/react/ReactElement.js 我们可以学习一下ReactElement有那些特性。本文件没有什么可学之处，只是便于增进对React元素的了解

## 文件解读：

1. 首先，文件中定义了一些校验函数，校验那些内容我们先不用管，这些函数有：
>hasValidRef,hasValidKey,defineKeyPropWarningGetter,defineRefPropWarningGetter,warnIfStringRefCannotBeAutoConverted

2. 定义了ReactElement函数
>const ReactElement = function (type, key, ref, self, source, owner, props){

    const element = {

    // This tag allows us to uniquely identify this as a React Element
    $$typeof: REACT_ELEMENT_TYPE,

    // Built-in properties that belong on the element
    type: type,
    key: key,
    ref: ref,
    props: props,

    // Record the component responsible for creating this element.
    _owner: owner,
  };
    ...
    
}

从函数的参数中我们可以看出来ReactElement的常用属性就是在这里定义的，key，self，owner。其中有个参数$$typeof: REACT_ELEMENT_TYPE标记了该对象是个React Element。

3. 定义了两个jsx相关的函数jsx，jexDEV，我们暂时不关注。

4. 定义了createElement函数

>export function createElement(type, config, children) {
>  let propName;

>  const props = {};

>  let key = null;
>  let ref = null;
>  let self = null;
>  let source = null;

>  if (config != null) {
    // 将 config 处理后赋值给 props
    // ...省略
  }

>  const childrenLength = arguments.length - 2;
>  // 处理 children，会被赋值给props.children
>  // ...省略

>  // 处理 defaultProps
  // ...省略

>  return ReactElement(
    type,
    key,
    ref,
    self,
    source,
    ReactCurrentOwner.current,
    props,
  );
}

React.createElement(component, props, ...childre)​返回一个 react 元素对象，包含：

​$$typeof​：Symbol.for('react.element')（暂时只涉及react element）
Built-in properties that belong on the element

​type​：原生标签类型（h1、p、div等）或者 function（类组件、函数组件等）(还有别的类型，比如React.Fragment、React.Suspense，这里暂不涉及)

​key​：列表中标识元素的唯一性

​ref​：同 react ref 属性

​props​：元素的 props 属性（包含 jsx 中写在元素标签上的属性和 children 属性）

5. 后边还有两个顶级API，cloneElement函数以及isValidElement()函数。

6. 源码实现：
```
    function createElement(type,config,children){
      //处理key
      let key,ref
      if(config){
        key = config.key
        ref = config.ref
        delete config.key
        delete config.ref
      }

      //处理children
      let props = {...config}
      if(config){
        //1 没有children
        //2 有一个儿子  （1）文本  （2）元素
        //3 多个儿子
        if(arguments.length>3){
          //arguments 这里其实是判断children有多少个
          props.children = Array.prototype.slice.call(arguments,2)
          //Array.prototype.slice.call 将类数组的arguments转化为数组

        }else if(arguments.length===3){
          //只有一个儿子
          props.children = children
        }
      }
      return{
        $$typeof:REACT_ELEMENT,
        key,
        ref,
        type,
        props
      }
    }
```