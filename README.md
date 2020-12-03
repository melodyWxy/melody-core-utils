# melody-cors-utils

1. react-life-hook

2. cookie-contrulor

3. melody-flux-store

...ohters

### life-hooks

> 现在，我们来定义一种react函数式组件的书写规范, 代码格式如下：

+ init-state
+ life-hooks
+ memo-callback
+ mome-UI


```jsx 
// demo
import React, { useCallback, useMemo } from 'react';
import { useInitState, useDidMount, useWillMount, useWillUpdate, useDidUpdate, useWillUnmount } from 'melody-cors-utils/life-hooks';
import cookies from 'melody-cors-utils/cookie-contrulor';

function App(props){
    // init - state
    const [state, setState] = useInitState({
        a: cookies.get('test-cookie'),
        b: 2,
        c: 3
    })
    // life-hooks
    useDidMount(()=>{
        console.log('did-mount');
        cookies.set('test-cookie', 'a-test-didMount')
        setState({
            c: 1005
        })
    })

    useWillUnmount(()=>{
        console.log('un-mount');
    })

    useWillMount(()=>{
        console.log('willMount');
    })
    useWillUpdate(()=>{
        console.log('will-update');
    })
    useDidUpdate(()=>{
        console.log('did-update');
    })
    
    // memo-callback
    const handleCountBClick = useCallback((b)=>{
        setState({
            b
        })
    }, [])

    // memo-UI
    const showA = useMemo(()=>{
        return  <div >{state.a}</div>;
    },[state.a])

    return (
        <div>
           {showA}
            <div onClick={()=>handleCountBClick(state.b+1)}>{state.b} + {state.a} --- {state.c}</div>
        </div>
    )
}

```


### cookie-contrulor

```js
   import cookies from 'melody-cors-utils/cookie-contrulor';
   cookies.get(key);
   cookies.getAll();
   cookies.set(key, value, day ?= 365, domain ?= '');
   cookies.remove(key);
   cookies.removeAll();
```

### melody-flux-store
> redux? why not event Store => 也许我不需要关心任意框架的组件间通信规则，因为我制定的就是规则。

```tsx
import melodyStore from 'melody-cors-utils/melody-flux-store';
// as Component A
class A extends React.Component{
    //...
    componentDidMount(){
        // 监听
        melodyStore.on('headerColorUpdate', (valueArr: any[]) => {
            this.setState({
                headerColor: value
            })
        })
        // 亦可随时发射
        melodyStore.emit('headerColorUpdate', valueArr);
    }
    componentWillUnmount(){
        // 亦可移除
        melodyStore.removeEvent('headerColorUpdate')
    }
    //...
}

```