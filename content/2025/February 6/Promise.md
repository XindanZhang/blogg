### What is A Promise?

> A promise is just some set of code that says you are waiting for some action to be taken (Jim finishing work) and then once that action is complete you will get some result (Jim paying you back). Sometimes, though, a promise will be unfulfilled (Jim breaking his leg) and you will not get the result you expect and instead will receive a failure/error.

Watched the tutorial [Promises in 10 Mintues](https://youtu.be/DHvZLI7Db8E?si=PR3l_h5nfZk5T08-) and learned to write Promise function.

```javascript
let p = new Promise((resolve, reject) => {
  let a = 1 + 1
  if (a == 2){
    resolve('success')
  }else{
    reject('failed')
  }
})

p.then(message =>{
  console.log(`This is in the then ${message}`)
}).catch(error => {
  console.log(`This is in the catch ${error}`)
})
```

Inplement all the code in the free online [website](https://codepen.io/pen/).
