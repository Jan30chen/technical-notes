## JavaScript-节流和防抖

+ 节流：规定时间多次触发，计时器不被更新，只触发第一次操作
+ 防抖：规定时间多次触发，计时器被更新，重新开始计时，触发最后一次操作

```js
//节流函数-定时器实现
const throttle = (fn, delay=1000) =>{
    let flag = false;
    return (...args)=>{
        if(flag) return;
        flag = true;
        setTimeout(()=>{
            fn.apply(this, args);
            flag = false;
        }, delay)
    }
};
//节流函数-时间戳实现
const throttle2 = (fn, delay=1000)=>{
    let previous = 0;
    return (...args)=>{
        let now = Date.now();
        if(now - previous <= delay) return;
        fn.apply(this, args);
        previous = now;
    }
}

//防抖函数-定时器实现
const debounce = (fn, delay=1000) =>{
    let timer = null;
    return (...args)=>{
        clearTimeout(timer);
        timer = setTimeout(()=>{
            fn.apply(this, args);
        }, delay)
    }
};

function fnn(){
    console.log("滚动！")
}
// 添加事件监听
document.addEventListener("scroll", throttle(fnn,1000));
document.addEventListener("scroll", debounce(fnn,1000));
```

