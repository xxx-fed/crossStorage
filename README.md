# crossStorage
具有跨域能力的localStorage

## 使用示例
```javascript
crossStorage.setItem('hero', 'annie')

crossStorage.getItem('hero', function(data){
    // data => 'annie'
})

crossStorage.removeItem('hero')

crossStorage.clear()

crossStorage.change(function(e){    // 模拟 window.onstorage 事件
    if (e.key === 'hero') {
        if (e.oldValue === 'amumu' && e.newValue === 'annie') {
            // that's crazy!!!
        }
    }
})
```

## 原理
* 数据存储在代理页面(http://hd.huya.com/proxy/storage.html)
* 父页面使用iframe内嵌代理页，通过 postMessage 进行存取