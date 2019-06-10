# Classes
[MDN : Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)

**hoisting**
class 선언은 function선언과 달리 hoisting 되지 않는다!

### class expressions
function expression처럼 class expression 선언이 가능함
```js
let Rectangle = class {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
```

## Class body and method definitions
The body of a class is the part that is in curly brackets  `{}`. This is where you define class members, such as methods or constructor.

### Strict mode
>The body of a class is executed in  [strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode), i.e., code written here is subject to stricter syntax for increased performance, some otherwise silent errors will be thrown, and certain keywords are reserved for future versions of ECMAScript.

class의 body는 strict mode로 실행됩니다. 달리 말하면, 여기에 쓰인 코드는 퍼포먼스 향상을 위한 더 엄격한 문법이 적용됩니다. 블라블라..

### Constructor
>The  [`constructor`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor)  method is a special method for creating and initializing an object created with a  `class`. There can only be one special method with the name "constructor" in a class. A  [`SyntaxError`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError "The SyntaxError object represents an error when trying to interpret syntactically invalid code.")  will be thrown if the class contains more than one occurrence of a  `constructor`  method.

>A constructor can use the  `super`  keyword to call the constructor of the super class.

`constructor` 메서드는 `class`로 만들어진 오브젝트를 생성하거나 초기화하는 특별한 메서드입니다. 클래스 선언 안에 "constructor"라는 이름의 메서드는 하나만 존재할 수 있습니다. `constructor`메서드가 두번 이상 나타나면 Syntax Error가 발생합니다.

### Prototype methods

See also  [method definitions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Method_definitions).

```js
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
  // Getter
  get area() {
    return this.calcArea();
  }
  // Method
  calcArea() {
    return this.height * this.width;
  }
}

const square = new Rectangle(10, 10);

console.log(square.area); // 100
```

### Static methods
> The  [`static`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static)  keyword defines a static method for a class. Static methods are called without  [`instantiating`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Introduction_to_Object-Oriented_JavaScript) their class and  **cannot** be called through a class instance. Static methods are often used to create utility functions for an application.

static 키워드는 static 메서드를 정의함. static 메서드들은 인스턴시에이션 없이 호출되며,  인스턴스를 통해서는 호출될 수 없다. static 메서드들은 종종 애플리케이션의 유틸리티 펑션을 만드는데 사용된다.
```js
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  static distance(a, b) {
    const dx = a.x - b.x;
    const dy = a.y - b.y;

    return Math.hypot(dx, dy);
  }
}

const p1 = new Point(5, 5);
const p2 = new Point(10, 10);

console.log(Point.distance(p1, p2)); // 7.0710678118654755
```


## Sub classing with `extends`

`extends` 키워드는 class 선언(혹은 class 표현식) 안에 사용되며 다른 클래스의 자식 클래스를 생성합니다.

```javascript
class Animal { 
  constructor(name) { // 인스턴스 생성시 인자를 받아 .name 속성에 등록
    this.name = name;
  }
  
  speak() {
    console.log(this.name + ' makes a noise.');
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name); // 수퍼클래스의 constructor를 호출하며, name을 인자로 전달합니다.
  }

  speak() { // Animal class의 speak 메서드를 override함.
    console.log(this.name + ' barks.');
  }
}

let d = new Dog('Mitzie');
d.speak(); // Mitzie barks.
```
만약 서브클래스에 constructor가 존재하면, "this"를 사용하기 전에 super()를 호출해야 합니다.

전통적인 function-based "classes" 역시 extend할 수 있습니다. :
```js
function Animal (name) {
  this.name = name;  
}

Animal.prototype.speak = function () {
  console.log(this.name + ' makes a noise.');
}

// constructor 함수와 prototype 속성에 메서드를 넣어둠.
// 이렇게 만든것도 class 선언시 extends로 가져?올수있다.

class Dog extends Animal {
  speak() { // 기존 메서드 override
    console.log(this.name + ' barks.');
  }
}

let d = new Dog('Mitzie');
d.speak(); // Mitzie barks.
```



## Super class calls with `super`

`super` 키워드는 수퍼클래스의 관련 메서드를 호출하는데 사용됩니다. 프로토타입 기반의 상속 방식에 비해 장점이 되는 부분입니다.

```js
class Cat { 
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

class Lion extends Cat {
  speak() { // speak 메서드를 override
    super.speak(); // 수퍼클래스의 speak메서드를 가져와 호출
    console.log(`${this.name} roars.`);
  }
}

let l = new Lion('Fuzzy');
l.speak(); 
// Fuzzy makes a noise.
// Fuzzy roars.
```

# Super

[MDN : Super] (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super)

**super** 키워드는 부모 오브젝트의 함수에 액세스하고 호출하기 위해 사용됩니다.

## Description
constructor 안에서 사용될 때, super 키워드는 하나만 존재해야 하며 반드시 `this`키워드가 사용되기 전에 사용되어야 합니다. super 키워드는 또한 부모 오브젝트의 함수를 호출하기 위해서도 사용될 수 있습니다.

## Example
### Using `super` in classes
```js
class Square extends Rectangle {
  constructor(length) {
    this.height; // ReferenceError, super는 처음에 실행되어야 합니다!

    // 부모 클래스의 constructor를 lengths를 인자로 호출합니다.
    // 이것은 정사각형(Rectangle)의 너비와 높이로 사용됩니다.
    super(length, length);

    // Note: 파생된 클래스들에서, super()는 `this`를 사용하기 전에 사용해야만 합니다.
    // 그냥 두면 reference error를 유발하게 됩니다.
    this.name = 'Square';
  }
}
```

### Using  `super.prop`  in object literals

```js
var obj1 = {
  method1() {
    console.log('method 1');
  }
}

var obj2 = {
  method2() {
    super.method1();
  }
}

Object.setPrototypeOf(obj2, obj1); // 이런게 있네? obj2의 prototype을 obj1으로 만드는듯.
// 즉 obj2의 상위클래스가 obj1이 되어서, super로 method1()을 가져와 호출.
obj2.method2(); // logs "method 1"
```

[MDN setPrototypeOf](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/setPrototypeOf) 보니까 [[Prototype]]을 직접 건드는거라 비추천한다고함.
