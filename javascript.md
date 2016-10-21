# The common snippets of the javascript :blush:


#### 防反跳 / debounce

```js
const debounce = (action, delay) => {
  let timer
  return function () {
    var context = this
    var args = arguments
    clearTimeout(timer)
    timer = setTimeout(() => { action.apply(context, args) }, delay)
  }
}
```

#### 拉平数组 / flatten

```js
const flatten = arr => {
    let list = []
    const flattenFunc = arg => arg.forEach(item => Array.isArray(item) ? flattenFunc(item) : list.push(item))
    flattenFunc(arr)
    return list
}
```

#### 获取函数的返回值 / value

```js
const value = func => {
  const valueFunc = arg => typeof arg === 'function' ? valueFunc(arg()) : arg
  return valueFunc(func)
}
```

#### 动态加载javascript

```javascript
  function loadScript(url, callback){
    var script = document.createElement('script');
    script.type = 'text/javascript';

    if(script.readyState){ //IE
      script.onreadystatechange = function(){
        if(script.readyState === 'loaded' || script.readyState === 'complete'){
          script.onreadystatechange = null;
          callback()
        }
      }
    }else{ //Others
      script.onload = callback.call(this);
    }

    script.url = url;
    document.getElementsByTagName('head')[0].appendChild(script);
  }
```

> From:[The best way to load external JavaScript](https://www.nczonline.net/blog/2009/07/28/the-best-way-to-load-external-javascript/)

#### 移动端页面初始化


```javascript
  ;(function(factory){
    window.factory = factory(window, document);

    return window.factory;
  })(function(win, doc){
    var context = {
      init:function(scale){
        var html = document.documentElement;
        var width = html.clientWidth > scale ? scale : html.clientWidth;
        var dpr = window.devicePixelRatio >= 1.5 ? 2 : window.devicePixelRatio;

        //设置html的dpr和fontsize
        html.setAttribute('data-dpr', dpr);
        html.style.fontSize = width * 100 / scale + 'px';

        //设置body的宽度--可以解决微信内置浏览器里面的一个小bug
        this.setBody();
      },
      view:function(){
        return {
          w:document.documentElement.clientWidth,
          h:document.documentElement.clientHeight
        }
      },
      setBody:function(){
        var body = document.body;
        body.style.width = this.view().w + 'px';
        body.style.height = this.view().h + 'px';
      }
    }
  })
```

#### 获取随机数

```javascript
  function getRandom(min, max){
    //左开右闭
    return Math.ceil(Math.random() * (max - min + 1) + min);
  }
```

```javascript
  function getRandom(min, max){
    //左闭右开
    return Math.floor(Math.random() * (max - min + 1) + min);
  }
```

```javascript
  function getRandom(min, max){
    //闭区间
    return Math.round(Math.random() * (max - min + 1) + min, 10);
  }
```
#### 获取元素的计算之后的样式

```javascript
  function getStyle(obj, attr){
    return obj.currentStyle ? obj.currentStyle[attr] : getComputedStyle(obj, null)[attr];
  }
```
#### 判断对象的数据类型

```javascript
  function type(obj){
    return Object.prototype.toString.call(obj).match(/\s(\w+)/)[1].toLowerCase();
  }
```

#### 对象复制

```javascript
  function copy(obj, deep){
    var result = '';
    if(type(obj) !== 'object') return;
    //利用JSON的方法进行深复制和浅复制
    return result = deep ? JSON.parse(JSON.stringify(obj)) : obj;
  }
```
#### 判断网页是否在微信浏览器打开

```javascript
  function isWeChat(){
    return navigator.userAgent.toLowerCase().match(/MicroMessenger/i) === "micromessenger";
  }
```

#### 判断手机系统

```javascript
  function isApple(){
    return /ip(hone|ad|od)/i.test(navigator.userAgent.toLowerCase());
  }
```
```javascript
  function isAndroid(){
    return /android/i.test(navigator.userAgent.toLowerCase());
  }
```
#### 获取查询字符串的值

```javascript
    function getQuery(url){
      var arr = [];
      var query = {};

      url = url && url.charAt(0) === '?' ? url.slice(1) : url || location.search.slice(1);
      arr = url.split('&');

      arr.forEach(function(v){
        var a = v.split('=');
        query[a[0]] = encodeURIComponent(a[1]);
      })

      return query;
    }
```
#### 获取元素在页面中的位置

```javascript
  function getPosition(ele){
    var position = {
      left:ele.offsetLeft,
      top:ele.offsetTop
    };
    var currentParent = ele.offsetParent;

    while (currentParent !== null) {
      position.left += currentParent.offsetLeft;
      position.top += currentParent.offsetTop;
      currentParent = currentParent.offsetParent;
    }

    return position;
  }
```

```javascript
  function getPosition(ele){
    return ele.getBoundingClientRect();
  }
```
#### js仿照md5

```javascript
  function md5(){
    var str = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/';
    return str.split('').sort(function(v1,v2){
      return Math.random() > 0.5;
    }).join('').slice(0, 32);
  }
```
#### 获取星期几

```javascript
  function getDay(){
    var arr = ['星期天', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
    return arr[new Date().getDay()];
  }
```

#### requestAnimationFrame的兼容性处理

```javascript
  ;(function(){
    var lastTime = 0;
    var vendors = ['ms', 'moz', 'webkit', 'o'];

    for(var i = 0; i < vendors.length && !window.requestAnimationFrame; ++i){
      window.requestAnimationFrame = window[vendors[i] + 'RequestAnimationFrame'];
      window.cancelAnimationFrame = window[vendors[i] + 'CancelAnimationFrame'] ||  window[vendors[x] + 'CancelRequestAnimationFrame'];
    }

    if (!window.requestAnimationFrame) window.requestAnimationFrame = function(callback, element) {
        var currTime = new Date().getTime();
        var timeToCall = Math.max(0, 16 - (currTime - lastTime));
        var id = window.setTimeout(function() {
            callback(currTime + timeToCall);
        }, timeToCall);
        lastTime = currTime + timeToCall;
        return id;
    };
    if (!window.cancelAnimationFrame) window.cancelAnimationFrame = function(id) {
        clearTimeout(id);
    };
  })()
```
#### 数组去重

```javascript
Array.prototype.unique = function() {
  return Array.from(new Set(this));
}
```

```javascript
Array.prototype.unique = function() {
  var sortArr = this.sort();
  return sortArr.filter(function(v,i,context){
    return v != context[i+1];
  })
}
```

```javascript
Array.prototype.unique = function() {
  var result = [];
  this.forEach(function(v){
    if(result.indexOf(v) === -1){
      result.push(v);
    }
  })
  return result;
}
```

```javascript
Array.prototype.unique = function() {
  var json = {};
  var result = [];
  this.forEach(function(value){
    var type = Object.prototype.toString.call(value).match(/\s(\w+)/)[1].toLowerCase();
    if(!((type + '-'+value) in json)){
      json[type + '-'+value] = true;
      result.push(value);
    }
  })
  return result;
}
```

```javascript
Array.prototype.unique = function() {
  var sortArr = this.sort(),
    i = 0,
    len = sortArr.length;
  for(; i < len; i++){
    if(sortArr[i] === sortArr[i++]){
      sortArr.splice(i,1);
      i--;
    }
  }
  return sortArr;
}
```

```javascript
Array.prototype.unique = function() {
  var sortArr = this.sort(), result = [];
  sortArr.reduce((v1,v2) => {
    if(v1 !== v2){
      result.push(v1);
    }
    return v2;
  })
  result.push(sortArr[sortArr.length - 1]);
  return result;
}
```

#### 操作className

```javascript
//操作className时候的辅助函数
function toArray(argu){
  var splitText = typeof argu === 'string' ? (argu.indexOf(' ') !== -1 ? ' ' : ',') : ' ';
  return Array.isArray(argu) ? argu : argu.split(splitText);
}
```
>说明：以下四种方法的参数的形式可以为以下几种

> * 数组：['classname1', 'classname2']
> * string
>  * 单字符串:'classname1'
>  * 空格分隔: 'classname1 classname2'
>  * 逗号分隔: 'classname1, classname2'

```javascript
HTMLElement.prototype.hasClass = function (classname) {
  let selfClass = this.className.split(' ');
      classname = toArray(classname);
  if(this.classList){
    for(let v of classname){
      if(!this.classList.contains(v)){
        return false;
      }
    }
  }else{
    for(let v of classname){
      if(selfClass.indexOf(v) === -1){
        return false;
      }
    }
  }
  return true;
}
```

```javascript
HTMLElement.prototype.addClass = function (classname) {
  return this.className = Array.from(new Set((toArray(classname).join(' ') + ' ' + this.className).split(' '))).join(' ');
}
```

```javascript
HTMLElement.prototype.addClass = function (classname) {
  classname = toArray(classname);
  classname.forEach(function(v){
    if(!this.hasClass(v)){
      this.className += ' ' + v;
    }
  }.bind(this))
  return this;
}
```

```javascript
HTMLElement.prototype.removeClass = function (classname) {
  var selfClass = Array.from(new Set(this.className.split(' ')));
      classname = toArray(classname);
  classname.forEach(function(v){
    if(selfClass.indexOf(v) !== -1){
      selfClass.splice(selfClass.indexOf(v), 1);
    }
  })
  this.className = selfClass.join(' ');
  return this;
}
```

```javascript
HTMLElement.prototype.toggleClass = function (classname) {
  classname = toArray(classname);
  classname.forEach(function(v){
    if(this.hasClass(v)){
      this.removeClass(v)
    }else{
      this.addClass(v);
    }
  }.bind(this))
}
```
