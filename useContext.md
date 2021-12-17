# useContext

父组件代码

>//创建context对象，并暴露出去  
import React,{useContext} from 'react'
import UcChild form './ucChild'

>export const Context = React.createContext({name:'',age:18})  
export default function UseContext(){  
    const ctx = useContext(Context)  
    console.log(ctx)  
    return(  
        < div>  
            < UcChild>< /UcChild>  
        < /div>
    )  
}

子组件代码

>import React,{useContext} from 'react'  
import {Context} from './index'//把父组件暴露出来的数据引入  
export default function UseContext(){  
    const ctx = useContext(Context)  
    return(  
        < div>  
            {ctx.name}{ctx.age}  
        < /div>
    )  
}