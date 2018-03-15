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
  
  Person.prototype = Name.prototype; // Person.prototype = new Name("Raymond");
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
      obj.reduce(function(a, c, index, d) {
          if(d.indexOf(c) !== index) d.splice(index, 1);
          return d;
      });
  }
  unique(array); // outputs: [0, 1, 4, 6, 3]
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
