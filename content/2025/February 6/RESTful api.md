## Create Arrow Functions

First step is to remove the function keyword from the function declaration.
```javascript
function sum(a, b) {
  return a + b
}
```
to
```javascript
const sum = (a, b) =>{
  return a + b
}
```
### [Arrow Function Scoping](https://blog.webdevsimplified.com/2020-09/arrow-functions/)
The arrow function has a key characteristic - it inherits the this value from the scope where it was defined, rather than creating its own scope.

```javascript
const user = {
  name: 'John',
  // Regular function syntax
  regularFunction: function() {
    console.log('Regular function this:', this.name); // "John"

    setTimeout(function() {
      // Regular function creates new this binding
      console.log('Callback this:', this.name); // undefined
    }, 1000);
  },

  // Arrow function syntax
  arrowFunction: function() {
    console.log('Regular function this:', this.name); // "John"

    setTimeout(() => {
      // Arrow function inherits outer this
      console.log('Arrow callback this:', this.name); // "John"
    }, 1000);
  }
};

user.regularFunction();
user.arrowFunction();
```

![[images/scope.png]]_Execute in [Javascript-playground](https://playcode.io/javascript)_

This is why arrow functions are commonly used in callbacks, especially when you need to preserve the outer this reference.

Arrow functions have become my go to for creating any new function.

---

## ES6



---

## RESTful API
If you use any modern website, the chances are you have interacted with the website with REST.

**REpresentation State Transfer**
- GET *get data*
- PUT(Full replacement update)/PATCH(Partial update) *update/add data*
- POST *create data*
- DELETE *delete data*
```javascript
// Case 3: Update nested object properties
// Original data
const user = {
    name: "John",
    settings: {
        theme: "dark",
        notifications: true,
        language: "en"
    }
}

// PATCH request
PATCH /users/123
{
    "settings": {
        "theme": "light"
    }
}
```
