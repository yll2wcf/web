##react-native 里的this

>>java里的this和js里的this.

  java里的this 指的的当前的这个类。
  js 里的this 只有在调用的时候才能确定指的是谁，默认情况下指向的是window.(this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁，实际上this的最终指向的是那个调用它的对象)
  
```js
function a(){
    var user = "追梦子";
    console.log(this.user); //undefined
    console.log(this); //Window
}
a();

```
这里没有人明确的调用a(),或者说调用a()的是当前的这个界面，那么this就是指当前这个界面，而当前界面里没有user,所以是undefined.

```js
  function a(){
       var user = "追梦子";
       console.log(this.user); //undefined
       console.log(this);　　//Window
 }
window.a();

```