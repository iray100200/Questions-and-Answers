# 1 <!doctype html>是为了防止浏览器兼容性问题所规定的，不加doctype可能会引发未知的兼容性问题;
# 2 <meta http-equiv="Content-Security-Policy" content="script-src 'self'>
# 3 Object.create() 实现继承，还需要执行被继承者的constructor，以防止取不到被继承者的属性
```javascript
  function Name(name) {
      this.name = name;
  }
  Name.prototype.getName = function() {
      console.log(this.name);
  }
  function Person(name) {
      Name.apply(this, arguments);
  }
  
  Person.prototype = Name.prototype;
  var person = new Person("Raymond");
  person.getName();
  // output: Raymond
```
