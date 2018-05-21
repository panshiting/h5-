### h5与ios交互
# 使用WebViewJavaScriptBridge与ios进行交互

这里只介绍js端使用方法 
1.将下面代码放入JS中
```
/*这段代码是固定的，必须要放到js中*/
// 设置jsBridge,申明交互
let setupWebViewJavascriptBridge = function (callback) {
  if (window.WebViewJavascriptBridge) {
    return callback(window.WebViewJavascriptBridge)
  }
  if (window.WVJBCallbacks) {
    return window.WVJBCallbacks.push(callback)
  }
  window.WVJBCallbacks = [callback]
  let WVJBIframe = document.createElement('iframe')
  WVJBIframe.style.display = 'none'
  WVJBIframe.src = 'https://__bridge_loaded__'
  document.documentElement.appendChild(WVJBIframe)

  setTimeout(function () {
    document.documentElement.removeChild(WVJBIframe)
  }, 0)
}
```

2、在下面方法体里写相关js代码
```
// 注意:下面的函数体内有任何错误，都不会有打印日志，也不会有任何回调,所以,如果遇到什么也没有输出，说明你写错了

// 传递数据
setupWebViewJavascriptBridge((bridge) => {
  bridge.callHandler('setDatas', JSON.stringify(json))
})

// 获取数据
setupWebViewJavascriptBridge((bridge) => {
  bridge.callHandler('getDatas', '{"type": 2}', (response) => {
    let token = response.slice(4)
  })
})
```

