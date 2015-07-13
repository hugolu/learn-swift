## Struct

- [與 Class 相仿的 Struct](#class_vs_struct)
- [Value Type vs. Reference Type](#value_vs_reference)
- [改變 struct 屬性的方法](#mutating_function)

<a name="class_vs_struct"></a>
### 與 Class 相仿的 Struct

struct 與 class 用法相近，但 struct 不具繼承功能。

```swift
struct SPoint {
    var x: Int = 0
    var y: Int = 0
}
var sp = SPoint()   //return: {x 0, y 0}

class CPoint {
    var x: Int = 0
    var y: Int = 0
}
var cp = CPoint()   //return: {x 0, y 0}
```

struct 預設有 memberwise initializer，呼叫時要定義外部參數名稱。

```swift
struct SPoint {
    var x: Int
    var y: Int
}
var sp = SPoint(x: 10, y: 10) //return: {x 10, y 10}

class CPoint {   //error: Class 'Point' has no initializers
    var x: Int
    var y: Int
}
```

struct 一旦自行宣告 initializer，就失去預設的 memberwise initializer。

```swift
struct Point {
    var x: Int
    var y: Int

    init() {
        self.x = 0
        self.y = 0
    }
}

var p1 = Point()                //return: {x 0, y 0}
var p2 = Point(x: 10, y: 10)    //error: Extra argument 'x' in call
```

<a name="value_vs_reference"></a>
### Value Type vs. Reference Type

struct 是 value type，class 是 reference type。

value type 的型別有`Int`, `Float`, `Bool`, `String`, `struct`...

```swift
var i: Int          //struct Int {...
var f: Float        //struct Float {...
var b: Bool         //struct Bool {...
var s: String       //struct String {...
```

reference type 的型別有`class`, `func`...

```swift
class Foo {}
var o: Foo          //class Foo {...
var f: ()->()       //()->()
```

value type 保存變數的值；reference type 保存變數的位置。

- 當`struct`賦值給一個常數，struct property無法變更。
- 當`class`賦值給一個常數，雖然常數值不能改變，但若指向的class property若為變數，則可以變更。

```swift
struct Foo {
    var x: Int = 0
}

let foo = Foo()
foo.x = 10    //error: Cannot assign to 'x' in 'foo'
```

```swift
class Foo {
    var x: Int = 0
}

let foo = Foo()
foo.x = 10    //return: {x 10}
```

value type 的變數賦值是複製整個內容；reference type 的變數賦值是複製記憶體的參考。

```swift
struct Foo {
    var x: Int = 0
}

var f1 = Foo()
var f2 = f1
f2.x = 10
f1          //return: {x 0}
f2          //return: {x 10}
```

```swift
class Foo {
    var x: Int = 0
}

var f1 = Foo()
var f2 = f1
f2.x = 10
f1          //return: {x 10}
f2          //return: {x 10}
```

<a name="mutating_function"></a>
### 改變 struct 屬性的方法

struct function 要修改自己的屬性要加上`mutating`關鍵字。

```swift
struct Foo {
    var flag: Bool = true
    func flip() {
        flag = !flag //error: Cannot assign to 'flag' in 'self'
    }
}
```

```swift
struct Foo {
    var flag: Bool = true
    mutating func flip() {
        flag = !flag //error: Cannot assign to 'flag' in 'self'
    }
}

var foo = Foo()
foo.flip()  //return: {flag false}
foo.flip()  //return: {flag true}
```