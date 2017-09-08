# read underscore3

```
var _ = function(obj){
  if(obj instanceof _) return obj;
  if(!(this instanceof _)) return new _(obj);
  this._wrapped = obj;
}
```
这里是创建了 underscore 对象.这个函数的目的其实就是为了创建 underscore 对象, 如果在 new 命令的时候有对象传进来, 那么就将这个对象传给新建对象的 _wrapped 属性存储起来.

```
if(typeof exports != 'undefined' && !exports.nodeType) {
  if(typeof module != 'undefined' && !module.nodeType && module.exports) {
    exports = module.exports = _;
  }
  exports._ = _;
} else {
  root._ = _;
}
```
这段代码如何理解呢? 这个是前端与 node.js 通用的模块封装模式, 我们知道 exports 和 module.exports 是 node 模块化的显著标志, 第一个先判断 exports 是否存在, 还有就是判断 exports
变量是否有 nodeType 属性, 如果有, 说明 exports 变量是一个 html 元素, 所以不使用 node 方式模块加载, 同理 module 变量也是这样.这里不再细讲 exports 和 module.exports 的区别, 简单总结就是 exports 看作是 module.exports 的一个快照.

```
var optimizeCb = function(func, context, argCount) {
    if (context === void 0) return func;
    switch (argCount) {
      case 1: return function(value) {
        return func.call(context, value);
      };
      // The 2-parameter case has been omitted only because no current consumers
      // made use of it.
      case null:
      case 3: return function(value, index, collection) {
        return func.call(context, value, index, collection);
      };
      case 4: return function(accumulator, value, index, collection) {
        return func.call(context, accumulator, value, index, collection);
      };
    }
    return function() {
      return func.apply(context, arguments);
    };
  };
```
这个函数是什么意思？从整体上看并没有太大的意思，我们知道 this 的指向是动态的，所以在实际开发中肯定免不了对函数的 this 值进行硬绑定的做法，但是 bind 函数会有兼容性问题，
所以会倾向于使用 call 方法和 apply 方法，这两个原生函数在使用的时候区别就在于 call 后面是可以跟随 n 的参数，而 apply 后面是跟随数组形式的参数的，
那为什么 underscore 源码需要将这两种方法区分呢？可以看到 optimizeCb 函数会将传递参数少于或等于 4 个的采用 call 方法来绑定 this， 
而对于多于 4 个参数的方法则会采用 apply 方法来进行绑定，其实这是代码性能优化的手段，apply 方法在执行的时候其实是比 call 方法要慢得多， apply 方法在执行的时候需要对传进来的参数数组
进行深拷贝：apply 内部执行伪代码
```
argList = [];

len = argArray.length;

index = 0;

while(index < len){
  indexName = index.toString();
  argList.push(argArray[indexName]);
  index = index + 1;
}

return func(argList);
```
此外，apply 内部还需要对传进来的参数数组进行判断到底是不是一个数组对象，是否是 null 或者 undefined，而对于 call 则简单许多：
```
argList = [];

if(argCount > 1) {
  while(arg) {
    argList.push(arg);
  }
}

return func(argList);
```
总的来说就是 apply 不管在什么时候，对参数的循环遍历都是必定执行的，而对于 call 来说则是当参数数目大于 1 的时候才会执行循环遍历，而且 apply 比 call 多了一些对参数的检查，
所以 call 比 apply 快，基于这一点，underscore 使用 optimize 函数优化需要硬绑定的代码性能。