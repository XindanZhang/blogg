### What is A Promise?

> A promise is just some set of code that says you are waiting for some action to be taken (Jim finishing work) and then once that action is complete you will get some result (Jim paying you back). Sometimes, though, a promise will be unfulfilled (Jim breaking his leg) and you will not get the result you expect and instead will receive a failure/error.

Watched the tutorial [Promises in 10 Mintues](https://youtu.be/DHvZLI7Db8E?si=PR3l_h5nfZk5T08-) and learned to write Promise function.

> [!Implementation]
> Inplement all the code in the free online [website](https://codepen.io/pen/).

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

With Callback functions:
```javascript
function handleJimWork(successCallback, errorCallback) {
  // Slow method that runs in the background
  const success = doJimWork()
  if (success) {
    successCallback()
  } else {
    errorCallback()
  }
}

handleJimWork(
  () => {
    console.log("Success")
  },
  () => {
    console.error("Error")
  }
)
```

Change to Promise:
```javascript
function handleJimWork() {
  return new Promise((resolve, reject) =>{
    const success = doJimWork()
    if (success) {
      resolve(200)
    } else {
      reject("Break his leg.")
    }
  })
}

handleJimWork()
  .then(amount => {
    console.log(`Jim paid you ${amount} dollars`)
  })
  .catch(reason => {
    console.error(`Error: ${reason}`)
  })
  ```

### Callback Hell

```javascript
// 1. Here 'callback' is a function parameter name, which can be replaced with any name
function one(callback) {
  doSomething()
  callback()  // execute the passed-in function
}

// 2. Here 'callback' is also a function parameter name
function two(callback) {
  doSomethingElse()
  callback()  // execute the passed-in function
}

// 3. Here 'callback' is also a function parameter name
function three(callback) {
  doAnotherThing()
  callback()  // execute the passed-in function
}

// 4. These are the actual callback functions being passed
one(() => {                    // anonymous function passed to one
  two(() => {                  // anonymous function passed to two
    three(() => {              // anonymous function passed to three
      console.log("We did them all")
    })
  })
})
```

### Promise Chaining
```javascript
function one() {
  return new Promise(resolve => {
    doSomething()
    resolve()
  })
}

function two() {
  return new Promise(resolve => {
    doSomethingElse()
    resolve()
  })
}

function three() {
  return new Promise(resolve => {
    doAnotherThing()
    resolve()
  })
}

Promise.all([one(), two(), three()])
  .then(messages => {
    console.log(messages)
    // ["From One", "From Two", "From Three"]
  })
  .catch(error => {
    // First error if any error
  })

```

`Promise.race` can also be used.
> But it only waits until one promise either fails or succeeds unlike Promise.any which only cares about the first success. Promise.race will wait until the first promise fails or succeeds and then call .then or .catch accordingly.
