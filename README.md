# crossStorage

具有跨域能力的localStorage。*（需要浏览器支持 localStorage 和 postMessage）*

*（win7 + IE 11 下 在进行 跨页面localStorage存储 读取时会存在内存泄露的情况，so，如要兼容IE则不推荐使用）*

## API 

  * setItem(key, val)

    存。val可以是 Boolean、Number、String、Object

  * getItem(key, callback)

    取。以获取到的val作为实参调用callback，val存的时候是什么类型的值，获取到的就是什么类型的值

  * removeItem(key)

    删。

  * clear()

    全删。

  * change(callback)

    监听`window.onstorage`事件，`setItem/removeItem/clear`操作会触发`storage`事件，以`{key, oldValue, newValue}`作为实参调用callback。

    *当`removeItem`时，`newValue`为`null`；当`clear`时，`key, oldValue, newValue`均为`null`。*

## Example

```javascript
crossStorage.setItem('hero', 'annie')
crossStorage.getItem('hero', console.log)    // => 'annie'
```


```javascript
/* page A */
crossStorage.setItem('annie', {sex: 0, age: 7, say: 'Did you see my bear?'})

setTimeout(function(){
    crossStorage.setItem('annie', {sex: 0, age: 8, say: 'Did you see my bear?'})
}, 365*24*60*60*1000)

/* page B */
crossStorage.change(function(e){
    if (e.key === 'annie') {
        if (e.newValue.age > e.oldValue.age) {
            console.log('Happy Birthday !')
        }
    }
})
```

## 原理
* 数据使用`localStorage`存储在代理页(www.huya.com/act/proxy/storage.html)
* 父页面通过`iframe`内嵌代理页，通过`postMessage`进行存取
