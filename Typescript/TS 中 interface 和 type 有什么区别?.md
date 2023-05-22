# TS 中 interface 和 type 有什么区别?

* 两者都是用来定义类型的
* 定义类型的范围不同
  * interface 只能定义对象类型，或接口当名字的函数类型
  * type 可以定义任意类型，包括基础类型，联合类型，交叉类型，元组等
* interface可以继承一个或多个接口或类型，也可以继承 type，但type没有继承功能
* type 交叉类型：& 可以让类型中的成员合并成一个新的 type 类型，但接口不能交叉合并
* interface 可合并声明：定义两个相同名称的**接口会合并声明**，定义两个同名的type会编译错误

示例：

```ts
interface Person {
  name: string;
  age: number;
}

type Pet = {
  name: string;
  type: 'cat' | 'dog' | 'bird' | 'fish';
}

function printInfo(info: Person | Pet): void {
  console.log(info);
}

class Animal {}
interface Lion extends Animal {
  roar(): void;
}

type WildAnimal = Animal & {
  isPredator: boolean;
};
```

### interface的特点

interface是TypeScript中一个非常重要的概念，它被广泛用于定义对象的类型。interface有以下几个特点：

* **可以被扩展（继承）**

interface可以被继承，它可以被另一个interface继承，这使得我们可以轻松地扩展一组属性。下面是一个示例：

```ts
interface Person {
  name: string;
  age: number;
}

interface Employee extends Person {
  salary: number;
}

const tom: Employee = {
  name: 'Tom',
  age: 25,
  salary: 10000,
};
```

* **声明合并**

当多个接口具有同名属性时，TypeScript自动将它们合并为一个接口。这种现象被称为声明合并。例如：

```ts
interface Person {
  name: string;
  age: number;
}

interface Person {
  gender: string;
}

const tom: Person = {
  name: 'Tom',
  age: 25,
  gender: 'male',
};
```

* **可以定义函数类型**

在interface中，我们可以定义一个函数类型来表示函数的类型注释。下面是一个示例：

```ts
interface AddFunc {
  (num1: number, num2: number): number;
}

const add: AddFunc = (a, b) => a + b;

console.log(add(3, 5)); // 8
```

### type的特点

type是TypeScript中另一个用于定义类型的关键字。它具有以下几个特点：

* **可以定义基本类型**

与interface不同，type可以用于定义原始类型的别名。下面是一个示例：

```ts
type Score = number;

const mathScore: Score = 80;
const englishScore: Score = 90;
```

在上面的例子中，我们用type定义了一个名为Score的类型别名，它等价于number类型。我们随后创建了两个变量，分配了Score类型的值。

* **可以使用交叉类型和联合类型**

type 和 interface 都可以使用交叉类型和联合类型。但是，type 相较于 interface，更加灵活地应用于复杂的类型定义。下面是一个示例：

```ts
type Pet = {
  name: string;
  type: 'cat' | 'dog' | 'bird' | 'fish';
};

type BirdPet = Pet & {
  type: 'bird';
  canFly: boolean;
};

type CatOrDogPet = Pet & {
  type: 'cat' | 'dog';
  hasFur: boolean;
};
```

在上面的例子中，我们用type定义了Pet、BirdPet、CatOrDogPet三个类型。BirdPet类型具有Pet类型的所有属性并添加了canFly属性，而CatOrDogPet类型包含Pet类型、'cat'、'dog'字面量类型和hasFur属性。

* **可以使用泛型**

type也可以使用泛型定义，它提供了强大的抽象能力。下面是一个示例：

```ts
type ArrayType<T> = T[];

const arr1: ArrayType<number> = [1, 2, 3, 4, 5];
const arr2: ArrayType<string> = ['one', 'two', 'three'];
```

在上面的例子中，我们用type定义了一个名为ArrayType的泛型类型别名，该类型别名需要一个类型参数T，返回一个由T类型元素组成的数组。我们使用了具体的类型参数number和string来创建arr1和arr2数组。

## 综合比较

interface和type有着相似的功能，但是它们也有各自独有的特点。我们可以把它们区分为以下几点：

### interface适用于

* 用于声明对象类型的接口
* 需要继承或声明合并的接口
* 声明函数类型的接口
* 需要在类型注释中使用类型的接口

### type适用于

- 声明原始类型的别名
- 定义复杂的类型，如交叉类型、联合类型、元组和映射类型
- 使用泛型的类型别名

我们需要根据自己的实际需求来选择使用interface或type，它们之间并没有绝对的优劣之分。通常情况下，当我们主要处理对象时，我们更倾向于使用interface；而当我们需要更加高级的类型定义时，我们更倾向于使用type。

## 总结

在TypeScript中，interface和type都是用于定义类型的关键字，它们在使用上有一些区别。interface更适用于对象类型的定义，可以被**继承、声明合并、定义函数类型**。而type则适用于**原始类型的别名、复杂类型的定义和泛型类型的别名**。我们需要灵活选择合适的类型定义方式，以满足实际需求。

