```


```

people类

身份证ID, name, 户籍地址，吃饭的方法， 睡觉的方法，

```javascript
function People(Id, name) {
  this.name = name;
  this.Id = Id;
  this.eat = () => {
    console.log('我在吃饭');
  };
  this.sleep = () => {
    console.log('我在睡觉');
  };
};
```



面向对象的三大宗旨：封装，继承，多态。

本篇文章将带各位了解一下JS中的继承。以及各个继承方法的优缺点。

JS在ES5之前，相比于Java等语言，并没有用class关键字来定义类，也没有用extends来继承。在ES5之前，要想实现继承有以下几种方法：

1. 原型链继承
2. 构造函数继承
3. 组合式继承
4. 原型式继承
5. 寄生式继承
6. 寄生组合式继承

ES6实现继承：
`class`与`extends`。本质与寄生组合式继承类似。

> 原型链继承

```javascript
function parent() {
};
parent.prototype.name = 'hellow word';
parent.prototype.color = [];
parent.prototype.getName = function() {
  console.log(this.name);
};
parent.prototype.setName = function(name) {
  this.name = name;
}
parent.prototype.setColor = function(color) {
  this.color.push(color)
};
parent.prototype.getColor = function() {
  console.log(this.color);
}
function child() {};
child.prototype = new parent();
const c1 = new child();
const c2 = new child();
c1.getName(); // hellow word
c2.setName('这是我更改后的名字');
c2.getName(); // 这是我更改后的名字
c1.getName(); // hellow word
c2.setColor('red'); 
c1.getColor(); // [ 'red' ]
c2.getColor(); // [ 'red' ]
```

优点：

原型链上的方法和父类自身属性或方法都能够被所有子类继承。

缺点：

1. 由于所有子类都共享原型上的方法，当更改的属性或方法是一个引用类型时，所有子类实例的属性或方法都会被改变。因为子类实例上存储的是栈上指向堆内的地址，更改的时候，是改的堆里面的数据。所以就发生了所有子类实例的都被改变。
2. 不能向构造函数传参
3. 继承比较单一，一个子类只能继承一个父类。

> 构造函数继承

```javascript
function parent1(name, colors) {
  this.name = name;
  this.color = [...colors];
  this.sayName = function() {
    console.log(this.name);
  };
};
function parent2(age) {
  this.age = age;
};
function child(name, age, colors) {
  	parent1.call(this, name, colors);
  	parent2.call(this, age);
};
child.prototype.setColor = function(color) {
  this.color.push(color);
};
const c1 = new child('byeL', 24, ['black']);
const c2 = new child('wxl', 27, ['white']);
c1.setColor('red');
console.log(c1); // { name: 'byeL', color: [ 'black', 'red' ], age: 24 }
console.log(c2); // { name: 'wxl', color: [ 'white' ], age: 27 }
```

优点：

1. 通过`call`或则`apply`的方法，将父类的属性或方法拷贝到自身，实现私有化。
2. 可以对父类构造函数传参。
3. 可以继承多个父类构造函数

缺点：

1. 每次创建子类实例的时候，假如父类中有函数，因为函数的功能是相对固定的，比如上述代码的`sayName`，基本上不会像`name`或者`age`那样自定义太多。假设新建了1000个子类实例，那么你每个子类实例里面都有一个`sayName`方法。完全可以将它放到原型上去，所以这也就是为什么说`方法都在构造函数中定义，无法复用`
2. 只能继承父类中的属性或者方法，不能继承父类原型链上的方法。

> 组合继承

```javascript
function parent(name, colors) {
  this.color = [];
  this.name = name;
};
function child(name, colors) {
  console.log(colors);
  parent.call(this, name, colors);
};
parent.prototype.addColor = function(color) {
  this.color.push(color);
};
child.prototype = new parent();
const c1 = new child('byeL', ['black']);
const c2 = new child('wxl', ['white']);
c1.addColor('red');
console.log(c1); // { color: [ 'red' ], name: 'byeL' }
console.log(c2); // { color: [], name: 'wxl' }
```

优点：

1. 可以向构造函数传参
2. 每个新实例的所存储的属性或方法数私有的
3. 可以共享一些公共方法。

缺点：

调用了两次父类构造函数，第一次是执行`child.prototype = new parent();`这个时候，会在原型上有一个color属性和name属性。第二次是在`parent.call(this, name, colors);`给`child`构造函数内部定义了color和name属性，屏蔽原型上的方法。这样原型上的部分属性和方法就是冗余的。

![image-20210415202732634](https://gitee.com/ByeL/blogimg/raw/master/img/20210415202732.png)

> 原型式继承

```javascript
function createObj(obj) {
  function f() {};
  f.prototype = obj;
  return new f();
}
const _proto_ = {
  name: '123',
  color: ['red'],
};
const p1 = createObj(_proto_); // 创建一个实例出来
p1.color.push('black'); // 给实例的color属性'black';
// 可以看出，引用类型会收到影响
console.log(_proto_); // { name: '123', color: [ 'red', 'black' ] }
```

优点：

工厂模式，不需要关心具体的创建过程。只需要知道结果。可扩展性高，当想创建不一样的实例对象的时候，只需要传入不同的原型即可。

缺点：

和原型链继承一样，所有的实例会共享原型，如果是引用类型的话，当一个实例更改了原型后，其余的实例的相应属性也会跟着变化。

>  寄生式继承

```javascript
function createObj(obj) {
  function f() {};
  f.prototype = obj;
  return new f();
};
function createChild(prototype) {
  const instance = createObj(prototype);
  instance.sayName = function() {
    console.log('Helloween');
  };
  return instance;
};
createChild({ name: 'byeL', age: 24 });
```

可以看做是原型式继承的增强版，可以添加一些方法或属性。

> 组合寄生式继承（完美继承）

```javascript
function bindPrototype(subType, superType) {
  const prototype = Object.create(superType.prototype);
  subType.prototype = prototype;
  prototype.constructor = subType; // 将父类的构造函数的原型的constructor指向子类，因为实例后的对象是子类的。
};
function parent(name) {
  this.name = name;
};
parent.prototype.sayName = function() {
  console.log(`我的名字是${this.name}`);
};
function child(name, age) {
  parent.call(this, name);
  this.age = age;
};
bindPrototype(child, parent);
const c1 = new child('byeL', 24);
c1.sayName();
```

优点：

1. 可以向父类构造函数传参
2. 存在私有变量
3. 父类构造函数只调用了一次，不会出现冗余的属性或方法
4. 可以共享原型上的方法或属性
5. 可以继承多个父类

> ES6中的继承

在ES6中，实现了通过`extends`来继承相应的`class`类。

```javascript
//转义前
class parent {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}
class child extends parent {
 constructor(name, age, color) {
 	super(name, age);
   this.color = color;
 }
}
// 文章的最后是转义后的ES5代码
```

我们可以从ES6转义后的代码可以看出，ES6的`class`以及`extends`其实是寄生组合式继承的的语法糖。

ES6的继承：

子类在构造函数中，必须调用super()方法，这个super指的是父类的构造函数。从而先获取父类实例。对应的ES5的代码是`_createSuper`。与ES5组合继承一样，父类需要将this指向子类的实例。我们都知道在`ES5`中我们用的是:

`parent.call(this, ...arguments)`。

在上面那个代码块中，parent就指的是父类的构造函数。

那么在`ES6`中，父类的构造函数指的是`class`中的`constructor`。他在子类的`class`中的构造函数中，用`super`表示，这也就是说为什么需要在子类构造函数中，需要首先调用`super`获取一个上下文this，然后在在这个this上面添加子类的属性或方法。对应的转义后的代码块是：

```javascript
_this = _super.call(this, name, age);
_this.color = color;
return _this;
```



```javascript


"use strict";

function _inherits(subClass, superClass) {  // 将父类的原型赋值给子类原型，并改写constructor
	if (typeof superClass !== "function" && superClass !== null) {
		throw new TypeError("Super expression must either be null or a function");
	}
	subClass.prototype = Object.create(superClass && superClass.prototype, {
		constructor: {
			value: subClass,
			writable: true,
			configurable: true
		}
	});
	if (superClass) _setPrototypeOf(subClass, superClass); // 设置原型链
}

function _setPrototypeOf(o, p) { // 设置
	_setPrototypeOf = Object.setPrototypeOf ||
	function _setPrototypeOf(o, p) {
		o.__proto__ = p;
		return o;
	};
	return _setPrototypeOf(o, p);
}

function _createSuper(Derived) {
	var hasNativeReflectConstruct = _isNativeReflectConstruct();
	return function _createSuperInternal() {
		var Super = _getPrototypeOf(Derived),
		result;
		if (hasNativeReflectConstruct) {
			var NewTarget = _getPrototypeOf(this).constructor;
			result = Reflect.construct(Super, arguments, NewTarget);
		} else {
			result = Super.apply(this, arguments);
		}
		return _possibleConstructorReturn(this, result);
	};
}

function _possibleConstructorReturn(self, call) {
	if (call && (typeof call === "object" || typeof call === "function")) {
		return call;
	}
	return _assertThisInitialized(self);
}

function _assertThisInitialized(self) {
	if (self === void 0) {
		throw new ReferenceError("this hasn't been initialised - super() hasn't been called");
	}
	return self;
}

function _isNativeReflectConstruct() {
	if (typeof Reflect === "undefined" || !Reflect.construct) return false;
	if (Reflect.construct.sham) return false;
	if (typeof Proxy === "function") return true;
	try {
		Boolean.prototype.valueOf.call(Reflect.construct(Boolean, [],
		function() {}));
		return true;
	} catch(e) {
		return false;
	}
}

function _getPrototypeOf(o) {
	_getPrototypeOf = Object.setPrototypeOf ? Object.getPrototypeOf: function _getPrototypeOf(o) {
		return o.__proto__ || Object.getPrototypeOf(o);
	};
	return _getPrototypeOf(o);
}

function _classCallCheck(instance, Constructor) {
	if (! (instance instanceof Constructor)) {
		throw new TypeError("Cannot call a class as a function");
	}
}

var parent = function parent(name, age) {
	_classCallCheck(this, parent);

	this.name = name;
	this.age = age;
};

var child =
/*#__PURE__*/
function(_parent) {
	_inherits(child, _parent); // 这一行是将父类的原型赋值给子类，并且绑定原型链

	var _super = _createSuper(child); // 返回的是一个函数。这个函数里会实例化一个父类出来。

	function child(name, age) {
		var _this;

		_classCallCheck(this, child);

		_this = _super.call(this, name, age); // 将父类的this指向子类。
		_this.color = color;
		return _this;
	}

	return child;
} (parent);
```

