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
   这里没有人明确的调用a(),或者说调用a()的是当前的这个界面，那么this就是指当前这个界面，而当前界面里没有 user,所以是undefined.




```js
function a(){
       var user = "追梦子";
       console.log(this.user); //undefined
       console.log(this);　　//Window
 }
window.a();

```
  显然当前调用a()的是window,那么this就是指的是当前界面。
  
```js
var o = {
    user:"追梦子",
    fn:function(){
        console.log(this.user);  //追梦子
    }
}
o.fn();

```
  O是个对象，并且O调用了fn，this 就是O,O里有user.
  
 
  
   
     
  
```js

var o = {
    a:10,
    b:{
        // a:12,
        fn:function(){
            console.log(this.a); //undefined
        }
    }
}
o.b.fn();


var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //undefined
            console.log(this); //window
        }
    }
}
var j = o.b.fn;
j();
```
这个有点兜圈子。上面那个被执行了，下面这个没有被执行所以它的this指的是window

```js
function Fn(){
this.user = "追梦子";
}
var a = new Fn();
console.log(a.user); //追梦子

```构造函数版this：

　　这里之所以对象a可以点出函数Fn里面的user是因为new关键字可以改变this的指向，将这个this指向对象a，为什么我说a是对象，因为用了new关键字就是创建一个对象实例，理解这句话可以想想我们的例子3，我们这里用变量a创建了一个Fn的实例（相当于复制了一份Fn到对象a里面），此时仅仅只是创建，并没有执行，而调用这个函数Fn的是对象a，那么this指向的自然是对象a，那么为什么对象a中会有user，因为你已经复制了一份Fn函数到对象a中，用了new关键字就等同于复制了一份。




