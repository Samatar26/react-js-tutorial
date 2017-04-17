# react-js-tutorial
###Some essential ES6 features
### Class
This is a class declaration
In order to add properties we need to use the constructor method.
The constructor method is run when we create a new instance of a class.

You don't put commas between our methods, you leave it blank.

Inside of our methods, the 'this' keyword binds to the new instance of your class.

```
class Human {
  constructor() {
    this.height = 171;
  }
}

//You can also pass in parameters

class Human {
  constructor(height) {
    this.height = height;
    this.location = {
      x: 0,
      y: 0
    };
  }
    walk(x, y) {
      this.location.x += x;
      this.location.y += y;
  }
}

const samatar = new Human();

```

### extends keyword
We want the warrior class to use the human class as its base. With the extends keyworrd, the warrior class will inherit all of the properties of the human class.

We might want to add some new properties.

We have to call super first, in order to override the existing method or add on to the properties.

It calls the 'super constructor', which is the constructor in the Human class. Applies all of those properties and allows us to apply our new property.

The this keyword is not available in our extended class unless we call super first.

```
class Warrior extends Human {
  constructor(){
    super()
    this.strength = 10;
  }
}
```

### static keyword
When you have a class in order to use a method, you have to instantiate  a new instance of that class. Sometimes you just want to get some data or access the class and get some information.

```
class Human {
  constructor(height) {
    this.height = height;
    this.location = {
      x: 0,
      y: 0
    };
  }
    walk(x, y) {
      this.location.x += x;
      this.location.y += y;
  }
  static sayHello() {
    return 'Hi There';
  }
}

```

We don't have to instantiate a new human object to use the static method.

You can also use the get and set kewyords of objects so instead of calling it, it will always return the result of the function, e.g.:
```
static get sayHello() {
  return 'Hi There';
}

console.log(Human.sayHello);

```

```
console.log(Human.walk()) //this will give us an error
```
