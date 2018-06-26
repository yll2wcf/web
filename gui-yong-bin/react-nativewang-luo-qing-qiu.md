react-native网络请求

react-native 使用fetch进行网络请求：


```js
 

 var REQUEST_URL = 'http://gank.io/api/search/query/listview/category/Android/count/10/page/1';
  fetch(REQUEST_URL , {
  method: 'GET',
  headers: {
    'Accept': 'application/json',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    firstParam: 'yourValue',
    secondParam: 'yourOtherValue',
  })
})

```
但是fetch并不支持请求超时，网络请求支持超时需要用到promise


#ES6语法里的promise
官网地址：https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise

The Promise object represents the eventual completion (or failure) of an asynchronous operation, and its resulting value.

Promise对象最终给出一个成功或者失败的异步操作做完他的结果。
用promise封装的超时请求以post请求为例
如果是get请求，只需要拼接url即可，不需要请求体
new Promise((resolve, reject) resolve，reject对应成功的回调和失败的回调

```js

/***
url:请求url；
timeOut:超时时间
body:请求参数

*/

const fetchData = (url,timeout=20000,body) => {
  const request = new Promise((resolve, reject) => {
    fetch(url,{
        method: 'POST',
        headers: {
        
            'Content-Type': 'application/json'
        },
        body:JSON.stringify(body)
    })
    // 请求状态成功，解析请求数据
    .then(res => {
      if (res.status >= 200 && res.status < 300) {
        //resolve(res);
        resolve(res.json())
      }
      reject(`${res.status}`);//这里是失败的回调
    })
    // 返回请求的数据
    .then(responseJson=>{
      resolve(responseJson)
    })
    // 返回错误
    .catch(e => reject(e.message));
  });

// 定义一个延时函数
  const timeoutRequest = new Promise((resolve, reject) => {
    setTimeout(reject, timeout, 'Request timed out');
  });

// 竞时函数，谁先完成调用谁
  return Promise
    .race([request, timeoutRequest])
    .then(res => {
      return res
    }, m => {
      throw new Error(m);
    });
};

export default fetchData

```

使用的时候传入url，请求超时时间，请求体，
```js

timeOutTest(){

        let url =  config.currentURL+config.api.logIn;
        fetchData(url,10000,{
            user_name:this.userName,
            password:this.passWord
        })
            .then( response =>{
              // 请求成功 do...
            })
            .catch( error => {
                if (error.message == '401'){
                   
                }else{
                    
                    console.log(error.message);
                  
                }

            })
    }
    


```








