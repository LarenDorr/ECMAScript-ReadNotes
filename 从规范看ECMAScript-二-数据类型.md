本篇来讲一下ES里的数据类型。

## 数据类型

ES里的数据类型分两种。

一种是**语言类型**，就是我们平常写的`12,'xccurate',undefined`等等类型。

一种是**规范类型**，是用来描述ECMAScript语言结构和ECMAScript语言类型的值，例如引用`Reference`，列表`List`等等。

先来看看基本的语言类型吧。

### 语言类型

ES的语言类型有七种：

- `Undefined`类型：只有一个值：`undefined`。
- `Null`类型：只有一个值：`null`。
- `Boolean`类型：有两个值：`true`和`false`。
- `String`类型：是零个或多个16位无符号整数值的所有有序序列的集合，最大长度为2^53 -1个元素。它的严格定义就不深究了，想了解的看[🔗](https://www.ecma-international.org/ecma-262/9.0/index.html#sec-ecmascript-language-types-string-type)。
- `Symbol`类型：每个值都是独一无二的，例如`Symbol('s1mple')`。规范中定义了一些内置的Symbol值[🔗](https://www.ecma-international.org/ecma-262/9.0/index.html#table-1)。
- `Number`类型：一共18437736874454810627个值，包括`NaN`、`+Infinity`、`-Infinity`。详细定义看[🔗](http://zhoushengfe.com/es6/es6-ch.html#sec-ecmascript-language-types-number-type)。
- `Object`类型：例如：`let a = {}` 这就得好好说说了，见下方。

#### Object类型

对象是属性`property`的集合。

属性使用键值对来标识，属性的键的值可以为`String`类型（可以为空`''`）或`Symbol`类型。

属性分为两种：

- 数据属性`data property`：将键值与一个**ES语言值**和一组**布尔属性**相关联。
- 访问器属性`accessor property`：将键值与一个或两个**访问器函数**和一组**布尔属性**相关联。访问器函数用于存储或检索与属性关联的ECMAScript语言值。

所有对象都是属性的逻辑集合。但是有很多种对象在访问和操作其属性的语义上有所不同。

**普通对象**(Ordinary objects)是最常见的对象形式，具有默认对象语义。 

**怪异对象**(exotic object)是其属性语义与默认语义不同的任何形式的对象。

***属性的特性***

特性`attribute`用来定义和解释对象属性的状态。

其中数据属性的特性有：`[[Value]]`、`[[Writable]]`、`[[Enumerable]]`、`[[Configurable]]`。

访问器属性的特性有：`[[Get]]`、`[[Set]]`、`[[Enumerable]]`、`[[Configurable]]`。

它们的意义也不多说了，随便一查都有，不是重点。

***对象的内部方法与内部插槽***

内部方法 `Object Internal`：其算法制定了对象的实际语义。不是ES语言的一部分，由规范定义，仅用于说明。内部方法是多态的，不同的对象可能会执行不同的算法。

内部插槽 `Internal Slots`：是给算法使用的内部状态。不是对象属性，不继承。其值可能为规范类型或者语言类型。（为了方便理解，其实可以把它看作内部属性，不对开发者可见。而且内部插槽这个名字实在太别扭了😂，下文就叫做内部属性了。）

这两个都是规范定义的用来描述ES对象行为的东西，可以简单的理解为对象的函数和属性。

**内部方法和内部属性均用双方括号标识：`[[xxxxx]]**`

这个表格[🔗](https://www.ecma-international.org/ecma-262/9.0/index.html#table-5)总结了对象的必要的内部方法，每个对象都必须有其算法，然而却不一定相同。

简略说一下有什么：

`[[GetPrototypeOf]]`、`[[SetPrototypeOf]]`：获取和设置对象的原型`prototype`。

`[[IsExtensible]]`：决定是否可以向此对象添加其他属性。

`[[PreventExtensions]]`：决定是否可以将新属性添加到此对象。// 这个跟上面那个有什么区别还不太清楚

`[[GetOwnProperty]]`、`[[DefineOwnProperty]]`：获取和设置对象（名称为参数的）属性的属性描述符。

`[[HasProperty]]`：判断对象是否有自己的或继承的（名称为参数的）属性。

`[[Get]]`、`[[Set]]`：获取和设置（名称为参数的）属性的值。

`[[Delete]]`：删除对象（名称为参数的）属性。

`[[OwnPropertyKeys]]`：返回对象自身的属性的列表。

上篇说过，函数是可调用对象。所以其具有额外的内部方法：`[[Call]]`。而构造函数具有额外的内部方法：`[[Construct]]`。

也就是说函数执行时最终调用的就是函数对象的`[[Call]]`方法，构造函数构造对象时，执行的是构造函数的`[[Construct]]`方法。

它们的的实际语义到时候再仔细分析。

接下来一小节[🔗](https://www.ecma-international.org/ecma-262/9.0/index.html#sec-invariants-of-the-essential-internal-methods)讲的是这些基本内部方法不变的行为。例如返回类型的规定，等等。这些规则是所有对象必须遵守的，尽管其算法有可能不同。这些边边角角不用开发者操心，就不详述了。

***内在对象***

内在对象 `Intrinsic Objects`是规范中的算法要引用到的内置对象。像`Array`、`String`、`Object`及其原型等等。

详细见表[🔗](https://www.ecma-international.org/ecma-262/9.0/index.html#table-7)，就先不列举了，等到用到的时候再说。

多说一句，这组内置对象是每个领域`Realm`中有一组。什么是领域呢，例如一个页面中的`iframe`中的`Array`对象与该页面中的`Array`对象就不一样，它们就是两个领域的。

### 规范类型

规范类型是在规范中使用的，用来描述ES语义的、无法在规范外使用类型。

#### List和Record类型

**List类型**用来解释函数参数列表的执行。List类型的值是包含单个值的列表元素的简单有序序列。

其实可以简单理解为ES的数组，但是写规范的时候还不存在ES的数组类型，只好用List来描述像数组这样的类型。例如，«1,2»定义了一个List值，它具有两个元素，每个元素被初始化为一个特定的值。 一个新的空列表可以表示为«»。

（在上篇提到的ES解释器engine262中List实现就是用数组来实现的。）

**Record类型**用于描述规范算法中的数据聚合。Record类型值由一个或多个命名字段组成。 每个字段的值都是ECMAScript值或由与Record类型相关联的名称表示的抽象值。 字段名称始终用双括号括起来，例如`[[value]]`。

这个就像是ES里的对象了，使用也很像：`R.[[Field2]]`是名为`[[Field2]]`的R的字段的缩写。

（在engine262里Record就是用对象来实现的。）

#### Set 和 Relation类型

**Set类型**用于解释内存模型中使用的无序元素集合，其中没有元素出现超过一次。元素可以添加到集合中，也可以从集合中移除。集合可以合并、交叉或从彼此中减去。

**Relation类型**用于解释Set上的约束。

// 这两个我还没遇到过，不太懂就不说了😂。

#### Completion Record 类型

Completion Record类型用来解释值和控制流的运行时传播。

ECMAScript规范中的每个运行时语义都显式或隐式返回一个报告其结果的完成Completion Record。

（这个定义读起来就很拗口，不过不是很重要，简单理解一下就行。）

Completion Record是一个Record，所以就可以用对象来描述它，它有三个字段：

```js
Completion Record = {
  [[type]] // Completion的类型，有 normal, break, continue, return, throw 5种类型
  [[value]] // 返回的值为ES语言值或空，仅当当[[type]] 为 normal,return, throw时有值
  [[target]] // 定向控制转移的目标label，为string或空，仅当[[type]] 为break, continue时有值
}
```

`[[type]]`为`normal`时返回的Completion Record称作**normal completion**。其余的都称为**abrupt completion**。

在规范的各种抽象操作（也叫算法）中会有这样的返回 `Return Infinity`，这就是隐式返回了一个Completion Record，相当于 `Return NormalCompletion("Infinity")`。

规范在算法中经常有 `? Call(exoticToPrim, input, « hint »)`这样以 `?`开头的语句，它的意思如下：

让`res`为执行`Call(exoticToPrim, input, « hint »)`抽象操作的结果。

上面说过，每个运行时语义都显示或隐式的返回一个Completion Record，所以

如果`res`为`abrupt completion`，返回`res`。

如果`res`为`Completion Record`， `res = res.[[value]]`。

你还会看到 `! ToString(key)`这样前缀为 `!`的语句，它的意思如下：

让res为ToString(key)

断言res不是`abrupt completion`

如果`res`为`Completion Record`，  `res.[[value]] = res`。

这两个不一样哦，注意看最后的赋值语句。

在阅读规范时，可暂时忽略其实际意义。

#### Reference类型

Reference类型用来解释诸如` delete `，` typeof `，赋值运算符，` super `关键字和其他语言特征等运算符的行为。

（在我看来，就是LHS，为了解决谁在哪里这样的问题）

一个Reference是解析的名称或属性绑定。Reference由三个组件组成：

```js
{
  BaseValue // 值为：undefined,Object,Boolean,String,Symbol,Number, Environment Record. 
  ReferencedName // String或Symbol值。
  StrictReference // 布尔值，标识是否为严格模式
}
```

`base`值为`undefined`时表示无法解析该引用。

还有Super Reference， 用于表示使用super关键字表达的名称绑定。有个额外的 `thisValue`组件，其`base`值永远不会为`Environment Record`。

举个栗子：

```js
let a = {
  b: 'test'
}
```

其中，查找b时就会得到一个Reference类型，其值为：

```js
{
  BaseValue: a,
  ReferencedName: b,
  StrictReference: false
}
```



规范还规定了对于它的一些抽象操作，例如`GetBase`、`GetReferencedName`、`IsStrictReference`等等。

简单说下吧：

- `GetBase ( V )`：V是一个Reference。获取V的`base`组件。

- `GetReferencedName ( V )`：V是一个Reference。获取V的`name`组件。

- `IsStrictReference ( V )`：V是一个Reference。获取V的`strict`组件。

- `HasPrimitiveBase ( V )`：V是一个Reference。用来判断V的base组件的值是否是这几个原始值`Boolean,String,Symbol,Number`。

- `IsPropertyReference ( V )`：V是一个Reference。用来判断V是否是属性Reference，即其base值为Object或者`HasPrimitiveBase ( V )`为true。

- `IsUnresolvableReference ( V )`：V是一个Reference。用来判断是否是无法解析的Reference，即`base`值为`undefined`。

- `IsSuperReference ( V )`：V是一个Reference。就是判断V是不是Super Reference，即有没有`thisValue` 组件。

- `GetValue ( V )`：

  如果V不是Reference，就直接返回V

  是Reference，

  ​	是无法解析的Reference，就抛出ReferenceError 错误。（就是平常遇到的变量未定义错误啦 `Uncaught ReferenceError: a is not defined`）

  ​	是属性Reference，

  ​		如果是base原始值`Boolean,String,Symbol,Number`，

  ​			就让base = ToObject(base)。⭕1（以后有红圈的地方就是有批注的地方，圈起来，重点😂）

  ​		返回 `base.[[Get]](GetReferencedName(V), GetThisValue(V))`。

  ​	那么Reference一定为 `Environment Record`，

  ​		返回 `base.GetBindingValue(GetReferencedName(V), IsStrictReference(V))`。

  ⭕1：这个地方就是为什么`string、number`不是对象却能够使用 `String、Number`对象上的方法的原因了。

  比如使用 `'SOMEBODY'.toLocaleLowerCase()`时要先进行LHS查询找到 `'SOMEBODY'.toLocaleLowerCase`是什么，才能执行函数。此时的Reference就是：

  ```js
  {
    BaseValue: 'SOMEBODY',
    ReferencedName: toLocaleLowerCase,
    StrictReference: false
  }
  ```

  判断 `'SOMEBODY'`是原始值，就进行`ToObject`，这个操作会将其变成一个String对象，再获取`toLocaleLowerCase`方法。

  当Reference为 `Environment Record`时，由于`Environment Record`还没讲，不说太多，大概就是获取Environment Record绑定值。

- `PutValue ( V, W )`：

  如果V不是Reference，抛出ReferenceError 。

  如果无法解析Reference，

  ​	如果是严格模式，抛出ReferenceError 。

  ​	否则，⭕2

  ​		获取全局对象`GlobalObject`，

  ​		执行 `Set(globalObj, GetReferencedName(V), W, false)`操作，返回其结果。

  如果是属性Reference，

  ​	如果是`base`原始值`Boolean,String,Symbol,Number`，就`ToObjet`。

  ​	执行 `base.[[Set]](GetReferencedName(V), W, GetThisValue(V))`操作，

  ​		如果返回值为`false`，且为严格模式，抛出ReferenceError 。

  ​	那么一定是Environment Record，

  ​		执行 `base.SetMutableBinding(GetReferencedName(V), W, IsStrictReference(V))`。返回其结果。

  ⭕2：这个地方就是平常我们没声明变量直接进行赋值操作 `a = 1`时的算法，会在全局对象上新建一个属性，并赋给其值。

  当Reference为 Environment Record时，大概进行的操作就是在类型为Environment Record 的`base`上创建一个可变绑定，并赋值。

- `GetThisValue ( V )`：当为SuperReference时返回`thisValue` 组件的值。

- `InitializeReferencedBinding ( V, W )`：此时`base`为Environment Record，执行 `InitializeBinding(GetReferencedName(V), W)`操作，就是初始化绑定。

总算讲完了，Reference类型算是比较重要的一种规范类型了，理解ES运行时语义绕不开它，所以讲了很多，不像Completion Record类型，没讲太多，一方面对于理解语义无关紧要，一方面是我也不太懂😂。

#### Property Descriptor类型

属性描述符类型，用来解释对象属性的特性的操作，其值为Record类型，分为数据属性描述符和访问器属性描述符。

它的抽象操作我就不多说了，自己了解下就行了[🔗](https://www.ecma-international.org/ecma-262/9.0/index.html#sec-property-descriptor-specification-type)。

#### Lexical Environment 类型 Environment Record类型

这两个是大部头，按下不表，下面有一节专门介绍，主要是用来描述ES里作用域，标识符绑定等等。

额外说一句，我看到现在很多教程里都还在用活动对象（Activation Object）来解释作用域、闭包时，还纳闷怎么没在规范里看到，后来一搜才发现，AO、VO是ES3规范里的东西，打ES5后就再也没这俩词了。。

#### Data Blocks

用来描述字节大小（8位）数值的不同且可变的序列。暂时我也不懂，就先不说了😂。

------

这篇就先结束到这吧，本来打算把下章抽象操作也讲完的，一写下来发现有点长，就下篇讲吧。下篇主要是类型测试、类型转换，还有对象上的基本操作。

最后如果对大家有用的话，求关注、star，github 仓库[🔗](https://github.com/LarenDorr/ECMAScript-ReadNotes)。🙏🙏