- 1 <!doctype html>是为了防止浏览器兼容性问题所规定的，不加doctype可能会引发未知的兼容性问题;
- 2 <meta http-equiv="Content-Security-Policy" content="script-src 'self'>
    SRI Subresource Integrity 
```xml
    <script crossorigin="anonymous" integrity="sha256-xxxx" src="http://cdn.example.com/js/jquery.js"></script>
```
- 3 Object.create() 实现继承，还需要执行被继承者的constructor，以防止取不到被继承者的属性
```javascript
  function Name(name) {
      this.name = name;
  }
  Name.prototype.getName = function() {
      console.log(this.name);
  }
  function Person(name) {
      Name.apply(this, arguments); // super constructor.. in the second way, should remove current line
  }
  
  Person.prototype = Object.create(Name.prototype); // Person.prototype = new Name("Raymond");
  var name = new Name("Raymond")
  var person = new Person("Raymond");
  person.getName();
  // output: Raymond
  person instanceof Person //true
  person instanceof Name //true
  name instanceof Person //false
```
- 3 CSS 块级元素 margin引发的问题，可以设置块级为行内元素可消除margin-top，margin-bottom共同存在时引发的问题
- 4 利用Array.prototype.reduceRight去重复数据
```javascript
  var array = [0, 1, 4, 6, 3, 0, 6];
  function unique(obj) {
    return obj.reduceRight(function(a, c, index, d) {
        if (a == c || (f = d.lastIndexOf(c)) !== index)
        d.splice(index || f, 1);
        if (!index) return d
    });
  }
  unique(array); // outputs: [0, 1, 4, 6, 3]
  
  //others
  
  function uniq(array) {
    var n = 0;
    var p = 0;
    for (var i = 1; i < array.length;) {
        if (array[i] === array[n]) {
            array.splice(i, 1);
        } else {
            n++;
            i++;
        }
    }
    return array;
}

function uniq(array) {
    for (var i = 0; i < array.length;) {
        if ((c = array.lastIndexOf(array[i])) !== i) {
            array.splice(c, 1)
        } else {
            i++
        }
    }
    return array
}

```
- 5 利用Array创建特定长度数组
```javascript
  Array.apply(this, { length: 100 }).map(Function.call, fn) // fn .. Number .. function(i) { return i; }
```
- 6 正则表达式
-- /i (忽略大小写)
-- /g (全文查找出现的所有匹配字符)
-- /m (多行查找)

- 7 CSS 是阻塞渲染的资源。需要将它尽早、尽快地下载到客户端，以便缩短首次渲染的时间。
```HTML
<link href="style.css" rel="stylesheet"> <!--始终阻塞渲染-->
<link href="print.css" rel="stylesheet" media="print"> <!--打印内容时适用,或想重新安排布局、更改字体适用-->
<link href="other.css" rel="stylesheet" media="(min-width: 40em)"> <!--媒体查询符合条件时阻塞渲染-->
```
- 8 浏览器渲染逻辑
1. 处理 HTML 标记并构建 DOM 树。
2. 处理 CSS 标记并构建 CSSOM 树。
3. 将 DOM 与 CSSOM 合并成一个渲染树。
4. 根据渲染树来布局，以计算每个节点的几何信息。
5. 将各个节点绘制到屏幕上。
- 9 使用requestAnimationFrame
```javascript
var taskList = breakBigTaskIntoMicroTasks(monsterTaskList);
requestAnimationFrame(processTaskList);

function processTaskList(taskStartTime) {
  var taskFinishTime;

  do {
    // Assume the next task is pushed onto a stack.
    var nextTask = taskList.pop();

    // Process nextTask.
    processTask(nextTask);

    // Go again if there’s enough time to do the next task.
    taskFinishTime = window.performance.now();
  } while (taskFinishTime - taskStartTime < 3);

  if (taskList.length > 0)
    requestAnimationFrame(processTaskList);

}
```
- 10  柯里化
```javascript
function currying(fn) {
  var args = [];
  return function() {
    if (arguments.length === 0) {
      return fn.apply(this, args);
    }
    console.log(1, arguments)
    Array.prototype.push.apply(args, arguments)
    return arguments.callee;
  }
}

var pure = function() {
  console.log(2, arguments);
  if (arguments.length === 1) return arguments[0]
  return Array.from(arguments).reduce((a, b) => a + b)
}

let a = currying(pure)

a(1)
a(2)()

```
- 11 快速排序算法
```javascript
function sort(array) {
    if (array.length <= 1) { return array; }
    var v = array.splice(0, 1)[0]
    var left = [], right = [];
    for ( var i = 0; i < array.length; i++) {
        if (array[i] < v) left.push(array[i])
        else right.push(array[i])
    }
    console.log(left, right, array, v)
    return sort(left).concat([v], sort(right))
}
```
- 12 深度优先遍历算法实现
```javascript
function traverse(obj, callback) {
    var p = [];
    function _traverse(_obj) {
        if (typeof _obj && _obj.length) {
            for (var j = 0; j < _obj.length; j++) {
                _traverse(_obj[j])
            }
        } else {
            p.push(_obj)
            if (callback) callback(_obj)
        }
    }
    _traverse(obj);
    return p;
}
```

- 12 广度优先遍历

```javascript
function bfs(array) {
  var result = [];
  (function traverse(a) {
    let t = []
    for (var i = 0; i < a.length; i++) {
      if (typeof a[i] === 'object') {
        t = t.concat(a[i])
      } else {
        result.push(a[i])
      }
    }
    if (t.length > 0) {
      traverse(t)
    }
    return result
  })(array)
  return result
}
```

- 13 Promise.resolve(thennable)

```javascript
const promise1 = Promise.resolve('test')
Promise.resolve(promise1).then(console.log) // output: test, not a promise object
```

- 14 跨域附加
```
1. WebSocket
2. iframe http get
```
