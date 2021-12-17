## 文件路径
packages/shared/reactsymbols.js

## 代码结构

>//定义Symbol  
export let REACT_ELEMENT_TYPE = 0xeac7;  
...

>//做了一个判断，使用symbol.for查询或新创建symbol  
>if(typeof Symbol === 'function' && Symbol.for){  
    const symbolFor = Symbol.for;  
  REACT_ELEMENT_TYPE = symbolFor('react.element');  
  ...  
}

>//返回一个获取iterator函数
>export function getIteratorFn(maybeIterable: ?any): ?() => ?Iterator<*> {
  
}

## 总结
Symbol 是一个构造函数

Symbol.for(key) 方法会根据给定的键 key ，来从运行时的 symbol 注册表中找到对应的 symbol，如果找到了，则返回它，否则，新建一个与该键关联的 symbol，并放入 全局 symbol 注册表中。

获取可迭代对象的迭代器函数：

获取可迭代对象的 Symbol.iterator 属性： maybeIterable[Symbol.iterator]
获取可迭代对象的 @@iterator 属性： maybeIterable['@@iterator']

源代码换一种写法更易于理解：
>// 如果没有原生的 Symbol 符号或 polyfill，则使用普通数字进行表示。  
> const hasSymbol = typeof Symbol === 'function' && Symbol.for;  
  export const REACT_ELEMENT_TYPE = hasSymbol ? Symbol.for('react.element') : 0xeac7;

