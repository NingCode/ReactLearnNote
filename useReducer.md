# useReducer

示例代码：
>import React,{useReducer} from 'react'  
const initState = {count:0}//创建一个初始值    
//这是useReducer()参数里面的reducer，是一个实现方法函数  
>const reducer = (state,action) => {   
    //根据dispatch传入的action去更新state  
    switch(action.type){  
        case 'reset':  
            return initState;  
        case 'add':  
            return {count:state.count+1}  
        case 'red':  
            return {count:state.count-1}  
            //不复合以上类型，返回原来的state  
        default:  
            return state  
    }  
}

>export default function UseRudecerCompt(){  
    //使用useReducer  
    const [state,dispatch] = useReducer(reducer,initState)  
    return(  
        < div>  
            < p>{state.count}< /p>  
            < button onClick={() => dispatch({type:'reset'}  )}>< button/>  
        < /div>  
    )  
}