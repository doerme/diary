# call faster than apply

## Function.prototype.apply(thisArg, argArray)
first, judge the func is a function or not, then judge the argArray is object(array) or not, it maybe null or undeined,
if func is not a function or argArray is null/undefined, will throw error.
then to get the argArray's length, and assignment to a variable named len, and len will be the result of argument's length property when we 
call [[Get]], and set a variable named n which is equal to len.
and init a empty list named argList, init the index to 0, then loop the argArray until the index < n, so a variable named indexName(ToString(index))
will produce, and the nextArg become the result of argument with index when we call [[Get]] ( like arguments[index] ), and append the nextArg to argList,
set index to index + 1, and return the result of function[[call]], thisArg for context, argList for arguments object

```
argList = [];

len = argArray.length;

index = 0;

while(index < n) {

  indexName = index.toString();
  argList.push(argArray[indexName]);
  index = index + 1;
}

return func(argList)
``` 

## Function.prototype.call(thisArg[,arg1[, arg2, ...]])
First, call will judge the func is a function or not, then make the argList a empty list(array), 
and then, is the arg is more than 1, argument will become the lastest element in the argList, finally,
return the result of the function[call], thisArg for context, and argList for arguments object.

only 4 steps in call the function.call 

```
if argConut > 1 && argList = [];

while(arg){

  argList.push(arg);
}

return func(argList);
```

link:
[why-is-call-so-much-faster-than-apply](https://stackoverflow.com/questions/23769556/why-is-call-so-much-faster-than-apply)
[知乎 why-is-call-so-much-faster-than-apply](https://zhuanlan.zhihu.com/p/27659836)
