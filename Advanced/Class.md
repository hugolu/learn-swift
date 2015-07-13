## Class

- [物件與類別](#object_and_class)
- [物件初始化](#object_initialization)
- [物件的屬性（property）](#object_property)
- [物件的方法（method）](#object_method)
- [optional 物件變數](#optional_object_variable)
- [類別的繼承](#class_inheritance)
- [初始化過程](#initialization_process)
- [方法與屬性的覆寫](#overriding)
- [類別的方法與屬性](#class_method_property)
- [權限管理](#access_control)

<a name="object_and_class"></a>
### 物件與類別

Swift 使用`class`關鍵字宣告類別 (Class)，使用類別名稱加上參數列表`()`產生物件 (Object)。

```swift
class Rect {        //宣告類別
    var width: Int = 10
    var height: Int = 10
}

var rect = Rect()   //產生物件
rect.width          //return: 10
```

> `width`與`height`為物件屬性，專屬於特定物件的變數或常數，存取時在物件名稱後加上`.`與屬性名稱。

<a name="object_initialization"></a>
### 物件初始化

基本上，物件初始化 (*Initialization*) 就是使用初始化函式 (initializer) 設定類別屬性的過程。 初始化函式使用`init`關鍵字當作函式名稱，除了不用`func`關鍵字與回傳值，定義方式與 function 類似。若類別沒有宣告初始化函式，則物件使用預設初始化函式`init(){}`。

物件屬性沒在初始化過程中賦值將導致編譯錯誤。

```swift
class Rect {        //error: class 'Rect' has no initializers
    var width: Int  //note: stored property 'width' without initial value prevents synthesized initializers
    var height: Int //note: stored property 'height' without initial value prevents synthesized initializers
}
```

物件初始化有三種方式：

- 宣告變數(常數)給初始值
- 宣告變數(常數)為 optional
- 在初始化函式中設定初始值

初始化方式一：宣告變數(常數)給初始值。

```swift
class Rect {
    var width: Int = 10
    var height: Int = 10
}

var rect = Rect()   //return: {width 10 height 10}
```

初始化方式二：宣告變數(常數)為 optional。

```swift
class Rect {
    var width: Int?
    var height: Int?
}

var rect = Rect()   //return: {nil nil}
rect.width = 10     //return: {{Some 10} nil}
rect.height = 10    //return: {{Some 10} {Some 10}}
```

初始化方式三：在初始化函式中設定初始值，初始值可以是預設值或透過參數傳遞。

```swift
class Rect {
    var width: Int
    var height: Int

    init() {
        width = 10
        height = 10
    }
}

var rect = Rect()   //return: {width 10 height 10}
```

```swift
class Rect {
    var width: Int
    var height: Int

    init(width: Int, height: Int) {
        self.width = width
        self.height = height
    }
}

var rect1 = Rect(width: 10, height: 10) //return: {width 10 height 10}
```

一旦自行宣告初始化函式，則預設初始化函式`init(){}`會失效。

```swift
class Rect {
    var width: Int
    var height: Int

    init(width: Int, height: Int) {
        self.width = width
        self.height = height
    }
}

var rect1 = Rect(width: 10, height: 10) //return: {width 10 height 10}
var rect2 = Rect()                      //error: missing argument for parameter 'width' in call
```

可宣告多個初始化函式。

```swift
class Rect {
    var width: Int
    var height: Int

    init(width: Int, height: Int) {
        self.width = width
        self.height = height
    }

    init() {
        width = 10
        height = 10
    }
}

var rect1 = Rect(width: 10, height: 10) //return: {width 10 height 10}
var rect2 = Rect()                      //return: {width 10 height 10}
```

物件初始化不一定都能成功，若有可能失敗則使用初始化函式`init?`回傳`nil`。

```swift
class Rect {
    var width: Int      //must be larger than 0
    var height: Int     //must be larger than 0

    init?(width: Int, height: Int) {
        self.width = width
        self.height = height

        if width <= 0 || height <= 0 {
            return nil
        }
    }
}

var rect1 = Rect(width: 10, height: 10)     //return: {width 10 height 10}
var rect2 = Rect(width: 10, height: 0)      //return: nil
```

<a name="object_property"></a>
### 物件的屬性（property）

物件屬性一般為 **stored property**，設定與讀取直接作用在屬性本身。

```swift
class Rect {
    var width: Int = 10
    var height: Int = 10
}

var rect = Rect()   //return: {width 10 height 10}
rect.width = 20     //return: {width 20 height 10}
rect.width          //return: 20
```

物件屬性也可以透過計算得到，稱之為 **computed property**。宣告方式如下範例，使用`get{}`定義讀取的方式，使用`set{}`定義設定方式。`set{}`可不定義，若沒有設定則變數唯讀，而`get{}`必須定義。computed property 不可宣告為常數。

```swift
class Rect {
    var width: Int = 10
    var height: Int = 10
    var x: Int = 0
    var y: Int = 0
    
    var centerX: Int {
        get {
            return x + width/2
        }
        set {
            x = newValue - width/2
        }
    }
    
    var centerY: Int {
        get {
            return x + height/2
        }
        set {
            y = newValue - height/2
        }
    }
}

var rect = Rect()   //return: {width 10 height 10 x 0 y 0}
rect.centerX        //return: 5
rect.centerY        //return: 5
rect.centerX = 10   //return: {width 10 height 10 x 5 y 0}
rect.centerY = 10   //return: {width 10 height 10 x 5 y 5}
```

除了 stored property 與 computed property ，使用 **property observer** 可以監控變數的存取狀況，使用`willSet{}`配合`newValue`得知變數將被設定的新值，使用`didSet{}`配合`oldValue`得知被取代的變數舊值。
例如下例，按照常理`rect`長寬不可小於等於零，若`width`被設為`0`則提出警告並放棄當次設定。

```swift
class Rect {
    var width: Int = 10 {   //larger than 0
        willSet {
            if newValue <= 0 {
                println("illegal value")
            }
        }
        didSet {
            if width <= 0 {
                width = oldValue
            }
        }
    }

    var height: Int = 10
}

var rect = Rect()   //return: {width 10 height 10}
rect.width = -1     //return: {width 10 height 10}, output: illegal value
rect.width = 20     //return: {width 20 height 10}
```

物件屬性使用`lazy`關鍵字延後屬性初始化的時間。以下範例，呼叫`Rect()`並不會對`position`賦值，而是直到存取變數的時候才會真的初始化`Point()`。

```swift
class Point {
    var x: Int = 0
    var y: Int = 0
    init() {
        println("Point.init()")
    }
}

class Rect {
    var width: Int = 10
    var height: Int = 10
    lazy var position = Point()
    init() {
        println("Rect.init()")
    }
}

var rect = Rect()   //return: {width 10 height 10 nil}
                    //output: Rect.init()
rect.position       //return: {x 0 y 0}
                    //output: Point.init()
rect                //return: {width 10 height 10 {{x 0 y 0}}}
```

<a name="object_method"></a>
### 物件的方法（method）

物件方法定義方式與 function 一樣。呼叫方式則是物件名稱後加上`.`連接方法名稱。

```swift
class Rect {
    var width: Int = 10
    var height: Int = 10
    
    func info() -> String {
        return "\(width)x\(height)"
    }
}

var rect = Rect()
rect.info()     //return: "10x10"
```

<a name="optional_object_variable"></a>
### optional 物件變數

物件變數可以宣告為 optional，定義時類別名稱後面加上`?`，取值時透過`!`強制解開包裝。

``` swift
class Rect {
    var width: Int = 10
    var height: Int = 10
    
    func info() -> String {
        return "\(width)x\(height)"
    }
}

var rect: Rect? = Rect()
rect!.info()    //return: "10x10"

rect = nil
rect!.info()    //fatal error: unexpectedly found nil while unwrapping an Optional value
```

使用`!`解開包裝取值需要判斷 optaional 是否為`nil`，改用`?`取值過程變成 **optional chaining**，只要取值過程遇到`nil`，則回傳`nil`，最終取得的值型別為 optional。

```swift
class Rect {
    var width: Int = 10
    var height: Int = 10

    func info() -> String {
        return "\(width)x\(height)"
    }
}

var rect: Rect? = Rect()
rect?.info()    //return: "10x10" (type: String?)

rect = nil
rect?.info()    //return: nil (do nothing)
```

<a neme="class_inheritance"></a>
### 類別的繼承

繼承像是什麼事都不做，就能平白享用父母留下來的財產。繼承方式為類別名稱加上`:`，然後接著要繼承類別的名稱。

```swift
class Rect {
    var width: Int = 10
    var height: Int = 10
    
    func info() -> String {
        return "\(width)x\(height)"
    }
}

class CoolRect: Rect {
    // do nothing
}

var rect = CoolRect()
rect.info()     //return: "10x10"
```

Swift 不允許多重繼承。

```swift
class BaseA { }
class BaseB { }

class Subclass: BaseA, BaseB {  //error: multiple inheritance from classes 'BaseA' and 'BaseB'
    
}
```

除了繼承父類別的屬性與方法，子類別也可以建立自己的。

```swift
class Rect {
    var width: Int = 10
    var height: Int = 10

    func info() -> String {
        return "\(width)x\(height)"
    }
}

class CoolRect: Rect {
    var color: String = "red"
    
    func desc() -> String {
        return "I'm \(color)"
    }
}

var rect = CoolRect()
rect.info()     //return: "10x10"
rect.desc()     //"I'm red"
```

<a name="initialization_process"></a>
### 初始化過程

初始化函式有兩種：

- Designated Initializer: 類別主要初始化函式。必須讓屬於類別本身所有的屬性完成初始化，然後呼叫父類別的初始化函式繼續完成初始化工作。
- Convenience Initialize: 方便使用者設定的初始化函式。使用`convenience`關鍵字並將某些參數定為預設值，然後呼叫同層的 designaed initializer 或 convenience initializer。

```swift
class Person {
    var name: String
    var age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
    
    convenience init(name: String) {
        self.init(name: name, age: 1)
    }
}

var father = Person(name: "Jack", age: 30)  //return: {name "Jack" age 30}
var baby = Person(name: "Jason")            //return: {name "Jason" age 1}
```

Designated Initializer 與 Convenience Initializer 的關係有三個規則。

- 規則 1：A designated initializer must call a designated initializer from its immediate superclass.
- 規則 2：A convenience initializer must call another initializer from the same class.
- 規則 3：A convenience initializer must ultimately call a designated initializer.

![relationships between designated and convenience initializers](https://developer.apple.com/library/prerelease/mac/documentation/Swift/Conceptual/Swift_Programming_Language/Art/initializerDelegation01_2x.png)

Designated initializer 的繼承。

```swift
class Base {
    var foo: Bool
    init(foo: Bool) {
        self.foo = foo
    }
}

class Sub: Base  {

}

var obj = Sub(foo: true)
```

Designated initializer 的覆寫。

```swift
class Base {
    var foo: Bool
    init(foo: Bool) {
        self.foo = foo
    }
}

class Sub: Base {
    var bar: Bool
    init(bar: Bool) {
        self.bar = bar
        super.init(foo: true)
    }
}

var x = Sub(bar: true)
var y = Sub(foo: true) // error: incorrect argument label in call (have 'foo:', expected 'bar:')
```

> 一旦子類別宣告 designated initializer，就失去繼承父類別 designated initializer 的能力。

Convenience initializer 的繼承。

```swift
class Base {
    var foo: Bool
    var bar: Bool

    init(foo: Bool, bar: Bool) {
        self.foo = foo
        self.bar = bar
    }
    init(foo: Bool) {
        self.foo = foo
        self.bar = true
    }
    convenience init() {
        self.init(foo: true, bar: true)
    }
}

class Sub: Base {
    
}

var x = Sub()                       //return: {{foo true bar true}}
var y = Sub(foo: true)              //return: {{foo true bar true}}
var z = Sub(foo: true, bar: true)   //return: {{foo true bar true}}
```

必須覆寫所有父類別所有 Designated Initializer 才能繼承 Convenience Initializer。

```swift
class Base {
    var foo: Bool
    var bar: Bool
    
    init(foo: Bool, bar: Bool) {
        self.foo = foo
        self.bar = bar
    }
    init(foo: Bool) {
        self.foo = foo
        self.bar = true
    }
    convenience init() {
        self.init(foo: true, bar: true)
    }
}

class Sub: Base {
    override init(foo: Bool, bar: Bool) {
        super.init(foo: !foo, bar: !bar)
    }
    override init(foo: Bool) {
        super.init(foo: !foo)
    }
}

var x = Sub()                       //return: {{foo false bar false}}
var y = Sub(foo: true)              //return: {{foo false bar true}}
var z = Sub(foo: true, bar: true)   //return: {{foo false bar false}}
```

不完全覆寫父類別所有 Designated Initializer，無法繼承 Convenience Initializer。

```swift
class Base {
    var foo: Bool
    var bar: Bool
    
    init(foo: Bool, bar: Bool) {
        self.foo = foo
        self.bar = bar
    }
    init(foo: Bool) {
        self.foo = foo
        self.bar = true
    }
    convenience init() {
        self.init(foo: true, bar: true)
    }
}

class Sub: Base {
    override init(foo: Bool, bar: Bool) {
        super.init(foo: !foo, bar: !bar)
    }
}

var x = Sub()                       //error: missing argument for parameter 'foo' in call
var y = Sub(foo: true)              //error: missing argument for parameter 'bar' in call
var z = Sub(foo: true, bar: true)   //return: {{foo false bar false}}
```

透過`required`關鍵字強制子類別需實作特定 initializer。

```swift
class Base {
    var foo: Bool
    
    init() {
        self.foo = true
    }
}

class Sub: Base {
    init(foo: Bool) {
        super.init()
        self.foo = foo
    }
}

var x = Sub(foo: true)
var y = Sub()   //error: missing argument for parameter 'foo' in call
```

```swift
class Base {
    var foo: Bool
    
    required init() {
        self.foo = true
    }
}

class Sub: Base {
    init(foo: Bool) {
        super.init()
        self.foo = foo
    }
} //error: 'required' initializer 'init()' must be provided by subclass of 'Base'
```

```swift
class Base {
    var foo: Bool

    required init() {
        self.foo = true
    }
}

class Sub: Base {
    init(foo: Bool) {
        super.init()
        self.foo = !foo
    }
    required init() {
        super.init()
        self.foo = !self.foo
    }
}

var x = Sub()           //return: {{foo false}}
var y = Sub(foo: true)  //return: {{foo false}}
```

<a name="overriding"></a>
### 方法與屬性的覆寫

如果想擴充繼承而來的方法，要用`override`明確宣告覆寫父類別方法。

```swift
class Rect {
    var width: Int = 10
    var height: Int = 10

    func info() -> String {
        return "\(width)x\(height)"
    }
}

class CoolRect: Rect {
    var color: String = "Red"
    
    override func info() -> String {
        return "\(color) " + super.info()
    }
}

var rect = CoolRect()
rect.info()     //return: "Red 10x10"
```

如果想擴充繼承而來的屬性，要用`override`明確宣告覆寫父類別屬性，覆蓋的屬性為 computed property。

```swift
class Rect {
    var width: Int = 10
    var height: Int = 10
}

class Square: Rect {
    var length: Int {
        get {
            return super.width
        }
        set {
            super.width = newValue
            super.height = newValue
        }
    }
    
    override var width: Int {
        get { return length }
        set { length = newValue }
    }
    
    override var height: Int {
        get { return length }
        set { length = newValue }
    }

}

var square = Square()   //return: {{width 10 height 10}}
square.width = 20       //return: {{width 20 height 20}}
square.height = 30      //return: {{width 30 height 30}}
square.length = 40      //return: {{width 40 height 40}}
```

property observer 也可覆寫，覆寫後呼叫順序為 subclass willSet, base class willSet, base class didSet, subclass didSet。

```swift
class Rect {
    var width: Int = 10 {
        willSet { println("父類別 willSet") }
        didSet { println("父類別 didSet") }
    }
    var height: Int = 10
}

class CoolRect: Rect {
    override var width: Int {
        willSet { println("子類別 willSet") }
        didSet { println("子類別 didSet") }
    }
}

var rect = CoolRect()   //return: {{width 10 height 10}}
rect.width = 20         //return: {{width 20 height 10}}
                        //output: 子類別 willSet
                        //output: 父類別 willSet
                        //output: 父類別 didSet
                        //output: 子類別 didSet
```

以下為更複雜的覆寫範例。

```swift
import Darwin

class Square {
    var width: Double = 10
    var area: Double {
        get { println("Base get"); return width*width }
        set { println("Base set"); width = sqrt(newValue) }
    }
}

var square = Square()
square.width        //return: 10
square.area         //return: 100
square.area = 225   //return {width 15}
square.width        //return: 15

class TalkingSquare: Square {
    override var area: Double {
        willSet { println("Sub willSet") }
        didSet { println("Sub didSet") }
    }
}

square = TalkingSquare()
square.area = 16  // trigger sequence: Sub willSet -> Base set -> Sub didSet
square.width

class FakedSquare: TalkingSquare {
    override var area: Double {           // 完全取代附類別的area
        get { return M_PI*width*width }
        set { super.width = sqrt(newValue/M_PI) }
    }
}

var circle = FakedSquare()
circle.width        //return: 10
circle.area         //return: 314.1592653589793
circle.area = 100   //return: {{{width 5.641895835477563}}}
circle.width        //return: 5.641895835477563
```

- `Square.area`是一個 computed property，透過`width`計算`area`的值。
- `TalkingSquare.area`覆寫`Square.area`，是一個 property observer，監控父類別屬性的變化。所以當 `TalkingSquare.area`變化時，除了觸動自己的 property observer，還會觸動父類別的 computed property。
- `FakedSquare.area`是一個 computed property，覆寫祖父類別 computed property。所以當 `circle.area`變化時，不會觸動父類別`TalkingSquare`的 property observer。

使用`final`修飾屬性或方法，子類別則無法進一步改寫。

```swift
class Base {
    final var foo: Int = 0
}

class Sub {
    override var foo: Int { //error: property does not override any property from its superclass
        willSet {}
        didSet {}
    }
}
```

```swift
class Base {
    func foo() {}
}

class Sub {
    override func foo() {}  //error: method does not override any method from its superclass

}
```

使用`final`修飾整個類別，子類別無法繼續繼承。

```swift
final class Base {
    
}

class Sub: Base {   //error: inheritance from a final class 'Base'
    
}
```

<a name="class_method_property"></a>
### 類別的方法與屬性

使用`static`定義類別方法與屬性。

```swift
class Base {
    static let max = 3
    static var cnt = 0
    static func usage() { println("\(Base.cnt)/\(Base.max)") }

    init?() {
        if Base.cnt >= Base.max {
            return nil
        }
        Base.cnt += 1
    }
}

var a = Base()  //return: __lldb_expr_965.Base
var b = Base()  //return: __lldb_expr_965.Base
var c = Base()  //return: __lldb_expr_965.Base
var d = Base()  //return: nil

Base.usage()     //output: 3/3
```

<a name="access_control"></a>
### 權限管理

Swift 提供三種存取權限：

- `internal`: for module or app only
- `public`: for anyone
- `private`: for the same file

```swift
class Girl {
    var name: String
    private var weight: Int
    
    init(name: String, weight: Int) {
        self.name = name
        self.weight = weight
    }
}

var mary = Girl(name: "Mary", weight: 50)
mary.weight //return: 50
```

> 在 Swift 中就算屬性宣告成`private`，還是可以透過`.`取得資訊。