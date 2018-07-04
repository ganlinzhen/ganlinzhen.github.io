#### 一、Js 原生对象的一些方法：

##### 1.string：

1）charAt（） 返回指定位置的字符串

2）concat（） 连接字符串

3）indexOf（‘searchStr’，startfrom）检测字符串是否含有某个字符串片段：String1.index（‘aaa’， 1）

4）结合正则表达式：match（）、search（）、replace（）

#### 二、js原型与构造函数

1.原型与构造函数的理解

##### 构造函数：

###### 带有实例化一个this对象的函数，并且具有prototype属性指向一个原型对象

##### 原型对象（prototype）:

###### 每个构造函数都会有一个原型对象，所有通过构造函数生成的对象，都共享一个原型对象。并且具有一个constructor属性指向自己的构造函数

##### es6 中的class：

整合了构造函数和原型对象，放在一起声明



构造函数有prototype属性存着自己的原型对象，同时原型对象上有constructor属性指向自己的构造函数

2.实例上有_____prototype_

3.原型的自带方法：

1）hasOwnProperty():判断实例上的属性是否属于自己还是属于原型上的cat1.hasOwnProperty('name')

2)   isPrototypeOf(): 判断实例是否与构造函数的关系Cat.prototype.isPrototypeOf(cat1)

3)  toString() 

4.es5有6种数据类型（number，string，boolean，null，undefind，object）es6加了symbol

5.区分具体的数据类型：

​	typeof 不能识别的类型 null—>object，object，array，但可以判断简单的数据类型和function

​	instanceof 只能用来判断对象的具体类型

​	object.prototype.tostring.call(***)  可以替代以上两种判断所有数据类型但判断起来不是很方便

###### 6.call方法：function.call(obj,param1,param2)

含义是指把function中的this对象指向obj，并执行function

1.var Animal={ 

​    Name:'Animal'

   say：function（）{
​	console.log(this.name)
   }

}

var Cat={

​     name：‘cat’

​    say： function（）{
   		console.log(this.name)

​	}

}

Animal.say.call(Cat)

2.var Cat=function（）{

​	this.name='zhenganlin'

}

Var  a={}

Cat.call(a)

a.name==='zhenganlin'

###### 7.{}+{};{}+[]的值

对于引用类型做加法：首先会进行Valueof；然后toString；

+{}====》''[object Object]''

+[1,2] ===> '1,2'

还有就是{}在最前面会被识别为函数式直接执行后面的

{}+[] ==> 0

#### 三、数组操作

1.类数组的数据类型：（有iterator接口）

- Array
- Map
- Set
- String
- TypedArray
- 函数的 arguments 对象
- NodeList 对象

#### 四、Object.defineProperty(obj, prop, descriptor)

descriptor: 常见属性

`enumerable`，属性是否可枚举，默认 false。

`configurable`，属性是否可以被修改或者删除，默认 false。

`writable`  

`get`，获取属性的方法。

`set`，设置属性的方法

```javascript
Object.defineProperty(obj,"handles", {
                value: {},
                enumerable: false,
                configurable: true,
                writable: true
            })
// 假如obj = {name: '111', age: '2222'}
// 这样 var newobj = Object.assign({},obj)
// 这是handles 属性并不会转移到newobj这个对象上面
```

#### 五、new 操作符做的事情

```javascript
function newOBJECT(Foo) {
  // 1. 新建一个实例对象
  var obj = {}
  // 2. 将实例的_proto_属性指向构造函数的原型对象
  obj._proto_ = Foo.prototype
  obj._proto.constructor = Foo
  // 3. 执行构造函数 并将 构造函数中的this指向新建的实例对象
  Foo.apply(obj,args)
  return obj  
}

```

































