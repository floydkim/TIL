---
title: 'JavaScript의 상속 구현: Psedoclassical vs. Prototypal'
date: 2019-03-16 19:59:12
category: development
---


[이 글](http://theoryapp.com/javascript-inheritance-pseudoclassical-vs-prototypal/)의 러프한 번역입니다.

  

# 자바스크립트 상속: Psedoclassical vs. Prototypal

  

JS는  프로토타입  오리엔티드  랭귀지임. 이것은  다른  오브젝트로부터  직접  상속이 가능하다는 뜻이다.

자바처럼 클래식한 언어들은 서브클래스가 수퍼클래스와 클래스의 인스턴스 오브젝트로부터 상속받는것과 대조적이다.

  

## pseudoclassical Inheritance

(주: pseudo-class-ical == class를 흉내낸다)

constructor함수를 사용하며, new 키워드로 새 오브젝트를 만든다. .prototype 속성을 이용해 상속 체인을 형성한다. 컨스트럭터 함수는 prototype 속성을 가지며 이 속성은 모든 인스턴스에 상속된다.

  

```javascript
function Point(x, y) {
  this.x = x || 0;
  this.y = y || 0;
}

Point.prototype.add = function() {
  return this.x + this.y;
};

var p = new Point(3, 4);

console.log(p instanceof Point);  // true
console.log(p.add()); // 7
```

`new Point(3, 4)` 호출하면 다음과 같은 일이 일어난다.

1. Point.prototype 을 상속받는 새 오브젝트를 만든다
2. Point()를 새 오브젝트의 컨텍스트에서 호출한다. 이것은 새 오브젝트의 속성을 initialize한다.

  

constructor함수 Point는 그 자신이 함수 오브젝트인 것에 주의하길 바란다.

즉 `Function`의 인스턴스이다.

  

```javascript
console.log(Point instanceof  Function);  //true
```

  

이제 우리는 Point를 상속받는 다른 pseudoclass를 만들 수 있다.

```javascript
function Point3D(x, y, z) {
  Point.call(this, x, y);
  this.z = z || 0;
}

Point3D.prototype = new Point();
Point3D.prototype.add_3d = function() {
  return this.x + this.y + this.z;
};

var q = new Point3D(3, 4, 5);

console.log(q instanceof Point);  // true
console.log(q instanceof Point3D);  // true
console.log(q.add()); // 7
console.log(q.add_3d());  // 12
```

  

constructor Point3D 안에, Point 컨스트럭터를 적용하기 위해 `Point.call`메서드를 사용했다.

`call`메서드는 `Function.prototype`으로부터 상속받은 것이다.

(주: Point 컨스트럭터를 this에 바인딩해 호출하므로, Point 안의 this.x 선언이 새로 만들어지는 인스턴스를 가리키게 되고, 결국 인스턴스에 x 라는 속성이 등록되는것임)

또한, Point3D의 .prototype 속성을 Point 인스턴스로 바꿔주었다.

  

새 오브젝트를 만들기 위해 `new Point3D(3, 4, 5)`를 호출할 때, 이런 일이 일어난다.

  

1. 새 오브젝트를 만들고 Point3D.prototype로부터 상속받는데 이것은 곧 Point 오브젝트(인스턴스)다.
2. 새 오브젝트의 컨텍스트에서 Point3D()를 호출한다.

  

## Prototypal Inheritance

  

prototypal 상속에서는 **어떠한 class에 대한 개념 없이, 존재하는 오브젝트로부터 직접적으로 새 오브젝트를 만든다.**

여기서는 새 오브젝트 생성을 위해 `Object.create`를 사용하며, 이 메서드는 새 오브젝트의 prototype이 될 오브젝트를 인자로 받는다.

  

```javascript
var point = {
  x: 0,
  y: 0,
  add: function() {
    return this.x + this.y;
  }
};

var p = Object.create(point);

p.x = 3;
p.y = 4;
console.log(p.add()); // 7


var point_3d = Object.create(point);

point_3d.z = 0;
point_3d.add_3d = function() {
  return this.x + this.y + this.z;
};

var q = Object.create(point_3d);

q.x = 3;
q.y = 4;
q.z = 5;
console.log(q.add()); // 7
console.log(q.add_3d());  // 12
```

  

`p`와 `point_3d`는 같은 방법으로 만들어졌음에 주목하자. 그러나 우리는 `point_3d`를 추가적인 속성과 메서드로 확장한다.

  

`Object.create` 메서드는 다음 방식으로 정의되어있다.

```javascript
Object.create = function (o) {
  function F() {}
  F.prototype = o;
  return new F();
};
```
