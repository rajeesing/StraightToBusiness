# About Javascript
It's dynamic programming language used to control multimedia content on your web page. This can be execute using javascript engine with browser (ex. V8 of Chrome browser) or node library. Javascript engine uses two type of data structure for execution.
1. Stack
2. Queue
   
Stack hold the execution block based on order of calls and keep poping the execution block once done it's execution context. Any callback will holds by queue and after all execution, callbacks executes. Event loop continuously checks for any new message for execution else exit. 

## Callback
Callback is any function which you are passing as argument to another function. This can be store in any variable like other normal data types. For ex.

Callback Example 1:
```
setTimeout(function(){
   console.log("This line will run after a second");
}, 1000);
```

Same function you can write as:
```
var callback = function(){
   console.log("This line will run after a second");
};
setTimeout(callback, 1000);
```
or

```
setTimeout(() => {
      console.log("This line will run after a second");
}, 1000);
```
Excercise 1: Print a table of 2 on console using callback.

Solution:
```
function PrintTable(sequence){
    console.log(sequence * 2);
 }
 
var sequence = [1,2,3,4,5,6,7,8,9,10];
sequence.forEach(PrintTable);
```
