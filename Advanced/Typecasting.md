# 型別轉型 (Typecasting)

- [Any & AnyObject](#Any_AnyObject)
- [型別轉換](#Typecasting)
- [as 配合 for、if、switch](#as_for_if_switch")

<a name="Any_AnyObject"></a>
## Any & AnyObject

### `Any`型別變數存放任何東西

```swift
class Foo {}
struct Bar {}

var any: Any
any = 12            //return: 12
any = 3.14          //return: 3.14
any = "hello"       //return: "hello"
any = Foo()         //return: __lldb_expr_882.Foo
any = Bar()         //return: __lldb_expr_885.Bar
```

### `AnyObject`型別變數存放任何**物件**

```swift
class Foo {}
struct Bar {}

var anyobj: AnyObject
anyobj = 123        //error: Cannot assign a value of type 'Int' to a value of type 'AnyObject'
anyobj = 3.14       //error: Cannot assign a value of type 'Double' to a value of type 'AnyObject'
anyobj = "hello"    //error: Cannot assign a value of type 'String' to a value of type 'AnyObject'
anyobj = Foo()
anyobj = Bar()      //error: Cannot assign a value of type 'Bar' to a value ot type 'AnyObject'
```

### 遺失型別

資料儲存在`Any`型別的變數就會遺失原來的型別。

```swift
var foo: Any = "hello"
var str: String = foo   //error: 'Any' is not convertible to 'String'
```

### 判斷型別

使用`is`判斷儲存在`Any`型別的變數是否為特定型別。

```swift
var foo: Any = "hello"

if foo is String {
    println("foo is a string")  //output: foo is a string
}
```

<a name="Typecasting"></a>
## 型別轉換

### `as!`向下轉型`Any`

使用`as!`將`Any`型別強制向下轉型為原來的型別 (force downcast)。

- `Any` 可以儲存 Value Type，如`Int`、`Double`、`Bool`、`String`、`enum`、`struct`。
- `Any` 也可以儲存 Reference Type，如`class`。

```swift
var foo: Any = "123"
var str = foo as! String
```

```swift
class Base {}
class Sub: Base {}

var base: Base = Sub()
var sub: Sub = base as! Sub
```

向下轉型的型別必須是原來的型別，否則系統會崩潰。

```swift
var foo: Any = 123
var str = foo as! String    //強制向下轉型失敗
```

```swift
class Base {}
class Sub: Base {}

var base: Base = Base()
var sub: Sub = base as! Sub //強制向下轉型失敗
```

### `as!`向下轉型`Any?`

當轉型的對象是一個 `Any?` 的 optional，還是使用`as!`向下轉型，接受轉型值得變數可以是：

- Optional
- Forced Unwrapping Optional
- 原來的型別

```swift
var str: Any? = "hello"

var str1 = str as! String?      //return: "hello"   (str1: String?)
var str2 = str as! String!      //return: "hello"   (str2: String!)
var str3 = str as! String       //return: "hello"   (str3: String)
```

```swift
var str: Any? = nil

var str1 = str as! String?      //return: nil       (str1: String?)
var str2 = str as! String!      //return: nil       (str2: String!)
var str3 = str as! String       //error
```

> 試圖將`nil`賦值給非 Optional 變數，會讓系統崩潰。

### `as?`推測後轉型

先用`is`判斷，在用`as!`轉型，太囉唆。

```swift
var any: Any = "hello"

if any is String {
    var str = any as! String    //return: "hello"   (str: String)
}
```

```swift
class Foo {}
var any: AnyObject = Foo()      //return: __lldb_expr_997.Foo (any: AnyObject)

if any is Foo {
    var foo = any as! Foo       //return: __lldb_expr_997.Foo (foo: Foo)
}
```

使用`as?`推測後轉型，若型別相符得到有值得 Optional，型別不符得到`nil`。

```swift
var foo: Any = "hello"

var str = foo as? String    //return: hello (str: String?)
var int = foo as? Int       //return: nil   (int: Int?)
```

```swift
class Foo {}
class Bar {}
var any: AnyObject = Foo()  //return: __lldb_expr_1037.Foo  (any: AnyObject)

var foo = any as? Foo       //return: __lldb_expr_1037.Foo  (foo: Foo?)
var bar = any as? Bar       //return: nil                   (bar: Bar?)
```

<a name="as_for_if_switch"></a>
## as 配合 for、if、switch

### 配合 if

- 使用`is`+`as!`組合技，遜！

```swift
class Foo {
    func bar() {}
}
var obj: AnyObject = Foo()

if obj is Foo {
    (obj as! Foo).bar()
}
```

- 使用`as?`+ Optional Binding 組合技，酷！

```swift
class Foo {
    func bar() {}
}
var obj: AnyObject = Foo()

if let foo = obj as? Foo {
    foo.bar()
}
```

### 配合 for-in

使用`as!`將`[AnyObject]`強制轉型成特定型別的 Array，然後透過`for-in`遍歷每個元素。

```switch
class Foo {
    func bar() {}
}

var array: [AnyObject] = [Foo(), Foo(), Foo()]
for foo in array as! [Foo] {
    foo.bar()
}
```

但如果`[AnyObject]`混入非目標型別的物件，會導致系統崩潰。

```
class Foo {}
class Fiz {}

var array: [AnyObject] = [Foo(), Fiz(), Foo()]
for foo in array as! [Foo] {    //fatal error: Down-casted Array element failed to match the target type
}
```

### `for`+`if`+`as?` 組合技

在`for-in`迴圈內，透過`as?`做 Optional Binding，可以處理`[AnyObject]`混和不同型別物件的問題。

```switch
class Foo { let desc = "Foo" }
class Bar { let desc = "Bar" }

var array: [AnyObject] = [Foo(), Bar()]
for any in array {
    if let foo = any as? Foo {
        foo.desc
    } else if let bar = any as? Bar {
        bar.desc
    }
}
```

### `for`+`switch`+`as` 組合技

與上面解法類似，但更為優雅的方式。

```switch
class Foo { let desc = "Foo" }
class Bar { let desc = "Bar" }

var array: [AnyObject] = [Foo(), Bar()]
for any in array {
    switch any {
    case let foo as Foo:
        foo.desc
    case let bar as Bar:
        bar.desc
    default: break
    }
}
```