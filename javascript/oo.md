#**JavaScript OO**

###  Class Object Instance

* Class

>A class is a collection of object. It defines the characteristics of the object. It is a template definition of variables and methods of an object.

>类是对象的集合。它定义了对象的特性。
它是一种定义对象变量和方法的模板。


```javascript
function Animal(name) { this.name = name }
// 或
var Animal = function(name) { this.name = name };
```

* Object

>The term of "Object" is used when referring to some data structure representing an object. For example, if you want to develop a racing game, you may need a Car object or a Map object that are describing cars and maps. See Object Oriented Programming (OOP) for a way of programming that mainly uses objects.

>"Object"被用作当指向一些数据结构从而最为一个对象。
例如，如果你想要开发一款赛车游戏，你需要一个车或者地图
地图对象，对它们进行描述。面向对象编程是使用对象主要的一种表现形式。

```javascript
function Animal() { }
var animal1 = new Animal();
var animal2 = new Animal();
```

* Instance

>In object-oriented programming (OOP), an instance is a specific realization of any object. Formally "instance" is synonymous with "object", as they are each a particular value (realization), and these may be called an instance object; "instance" emphasizes the distinct identity of the object.

>在面向对象编程中，一个实例就是任一对象具体的实现。
正式的实例等同于对象，他们实现了对象中特定的值，也可以称
只为对象实例。实例强调了对象的不同身份。

```javascript
var Cat = new Animal('miaomiao');
```

* Relationship

>类：创建对象的模板

>对象：类的实例的集合

>实例：对象具体的实现

![Relationship](/source/img/javascript/class.png)

### JavaScript中定义class

```javascript
function Animal(name) { this.name = name }
// 或
var Animal = function(name) { this.name = name };
```

### JavaScript定义构造函数
``` javascript
function Person(name, age, sex) {
  this.name = name;
  this.age = age;
  this.sex = sex;
}
```

### JavaScript定义属性、方法（类方法、实例方法）

* Property

```javascript
function Animal(name , age, type) {
  this.name = name;
  this.age = age;
  this.type = type;
}

var dog = new Animal( 'wangwang', 2, 'Dog');
```

* Method

>静态方法

>>定义

```javascript
Animal.prototype.run = function() {
  console.log("Animal is running!");
};
```
>>引用

```javascript
dog.run();
```
>类方法

>>定义

```javascript
Animal.eating = function() {
  console.log("Animal is eating!");
};
```
>>引用

```javascript
Animal.eating();
```

### Inheritance

>Inheritance is getting or inheriting the property from its parent class.A class which inheriths the property from its parent class is called child class.

>继承是从父类获取或继承属性。一个继承父类属性的类称为子类。

```javascript
function Animal (name, age, type) {
  this.name = name;
  this.sex = age;
  this.type = type;
}

function Dog (name, age) {
  Animal.call(this, name, age, 'Dog');
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

var dog = new Dog('WangWang', 2);
```

### Encapsulation

>Encapsulation is the packing of data and functions into a single component. The features of encapsulation are supported using classes in most object-oriented programming languages, although other alternatives also exist. It allows selective hiding of properties and methods in an object by building an impenetrable wall to protect the code from accidental corruption.

>封装是包装数据和函数到一个单一的组件中。封装的特性
在大多数面向对象语言中都被支持使用，虽然有一些语言不
支持。它允许在类中选择隐藏属性和方法通过建立一堵墙去
保护代码。

```javascript
function Animal (name, age, type) {
  this.name = name;
  this.sex = age;
  this.type = type;
}

function Dog (name, age) {
  Animal.call(this, name, age, 'Dog');
};

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

function Car (animal) {
  this.crew = animal;
}

Car.prototype.getCrewName = function() {
  return this.crew.name;
};

var dog = new Dog('dog', 2);

var car = new Car(dog);

//console.log(car.crew.name); （破坏封装特性）
console.log(car.getCrewName());
```

### Polymorphism

>子类的实例可以赋值给父类的引用。

>多态指同一个实体同时具有多种形式。它是面向对象程序设计（OOP）的一个重要特征。如果一个语言只支持类而不支持多态，只能说明它是基于对象的，而不是面向对象的。

```javascript
function Animal (name, age, type) {
  this.name = name;
  this.sex = age;
  this.type = type;
}

function Dog (name, age) {
  Animal.call(this, name, age, 'Dog');
}
function Cat (name, age) {
  Animal.call(this, name, age, 'Cat');
}

var dog = new Dog('WangWang', 4);
var cat = new Cat('MiaoMiao', 3);

var animals = [ dog , cat ];
for(var i = 0; i < animals.length; i++) {
  console.log(animals[i].name);
  console.log(animals[i].age);
  console.log(animals[i].sex);
}
```
**[ More about [javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) ]**
