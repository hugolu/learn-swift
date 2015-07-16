# ARC - Automatic Reference Counting

- [變數與常數的生命週期](#Life_Cycle)
- [類別解構函式`deinit`](#Deinit)
- [Reference Cycle](#Reference_Cycle)
- [`weak`針對 Optional 的變數、常數](#Weak)
- [`unowned`針對非 Optional 的變數、常數](#Unowned)
- [Capture Lists](#Capture_Lists)
- [更複雜的情況](#Complex_Case)

<a name="Life_Cycle"></a>
## 變數與常數的生命週期

Function 的參數，執行區塊`{}`裡面的變數、常數會隨著執行任務結束而釋放記憶體空間。

```swift
func foo(argument: Int) {
    var variable: Int = 1
    let constant: Int = 2
}

foo(0)
//釋放 arg, variable, constant 記憶體
```

Struct 區塊`{}`裡面的變數、常數會因為 Struct 產生出來的變數不再被參考到而釋放記憶體空間。

```swift
struct Bar {
    var variable: Int = 1
    let constant: Int = 2
}

var bar: Bar? = Bar()
bar = nil   //釋放 variable, constant 記憶體
```

同樣的，Class 區塊`{}`裡面的變數、常數也會因為 Class 產生出來的變數不再被參考到而釋放記憶體空間。

```swift
class Fiz {
    var variable: Int = 1
    let constant: Int = 2
}

var fiz: Fiz? = Fiz()
fiz = nil   //釋放 variable, constant 記憶體
```
<a name="Deinit"></a>
## 類別解構函式`deinit`

`deinit{}`負責處理在類別消除前執行一些動作，我們可以利用這個函式觀察物件的生命週期。

```swift
class Person {
    var name: String

    init(name: String) {
        self.name = name
        println("\(name) is borned")
    }

    deinit {
        println("\(name) is dead")
    }
}

var man: Person? = Person(name: "Hemingway")        //output: Hemingway is borned
man = nil                                           //output: Hemingway is dead
```

<a name="Reference_Cycle"></a>
## Reference Cycle

Swift 透過 ARC 記憶體管理回收沒有被參考到的變數、常數，但有些情形會是物件間相互參考，雖然失去外部參考沒有人可以存取，但記憶體始終不會被釋放。如下範例：

```swift
class Person {
    var name: String
    var lover: Person?

    init(name: String) {
        self.name = name
        println("\(name) is borned")
    }

    deinit {
        println("\(name) is dead")
    }
}

var boy: Person! = Person(name: "Roméo")        //output: Roméo is borned
var girl: Person! = Person(name: "Juliette")    //output: Juliette is borned

boy.lover = girl        //return: {name "Roméo" {{name "Juliette" nil}}}
girl.lover = boy        //return: {name "Juliette" {{name "Roméo" {{…}}}}}

boy = nil
girl.lover              //return: {name "Roméo" {{name "Juliette" {{…}}}}}
girl = nil
```

- 兩人相愛之下`boy.lover = girl`、`girl.lover = boy`，物件形成相互參考。
- 當男孩失去外部參考`boy = nil`，卻因為女孩依然記得他`girl.lover = boy`，男孩沒被系統回收。
- 當女孩失去外部參考`gilr = nil`，卻因為死去的男孩依然也記得她`boy.lover = girl`，女孩沒被系統回收。
- 兩個人就變成~~殭屍~~傳奇，永遠不消滅。

<a name="Weak"></a>
## `weak`針對 Optional 的變數、常數

物件永不消滅，這不科學啊！這是記憶體洩漏 (Memory Leak)，記憶體用不著抓著不放會導致 App 消耗越來越多的記憶體，最後系統因為記憶體資源不足必須刪除進程，佔用大量記憶體的程式就是開刀對象啦。

那麼之前的問題要如何解決呢？答案是將有可能造成 Reference Cycle 的變數宣告為 Optional 並加上`weak`關鍵字，告訴系統這個變數不會增加 ARC 參考數量。

```swift
class Person {
    var name: String
    weak var lover: Person?
    
    init(name: String) {
        self.name = name
        println("\(name) is borned")
    }

    deinit {
        println("\(name) is dead")
    }
}

var boy: Person! = Person(name: "Roméo")        //output: Roméo is borned
var girl: Person! = Person(name: "Juliette")    //output: Juliette is borned

boy.lover = girl        //return: {name "Roméo" {{name "Juliette" nil}}}
girl.lover = boy        //return: {name "Juliette" {{name "Roméo" {{…}}}}}

boy.lover?.name         //return: "Juliette"
girl.lover?.name        //return: "Roméo"

boy = nil               //output: Roméo is dead
girl.lover?.name        //return: nil
girl = nil              //output: Juliette is dead
```

- 雖然兩人相愛`boy.lover = girl`、`girl.lover = boy`，但因為`weak`物件不會形成相互參考。
- 當男孩失去外部參考`boy = nil`，女孩馬上忘記這段感情`girl.lover?.name //return: nil`，男孩被系統回收。
- 當女孩失去外部參考`gilr = nil`，因為世界上沒有人記得她，女孩也被系統回收。
- 兩個人都跑去投胎，記憶體順利被釋放。

<a name="Unowned"></a>
## `unowned`針對非 Optional 的變數、常數

另一種情況是物件所參考的對象在物件生成的時候就存在，直到物件消滅。

以下範例`Slave`一出生就確定跟隨`Master`，而`Master`可以在其生命週期中任意更換`Slave`。

```swift
class Slave {
    let master: Master
    
    init(master: Master) {
        self.master = master
        println("Slave init")
    }
    
    deinit {
        println("Slave deinit")
    }
}

class Master {
    var slave: Slave?
    
    init() {
        slave = Slave(master: self)
        println("Master init")
    }
    
    deinit {
        println("Master deinit")
    }
}

var master: Master! = Master()      //output: Slave init -> Master init
master = nil                        //Slave don't let Master die!!!
```

- 因為`Slave`會記住`Master`，所以就算主人死去`master = nil`，兩個人還是沒有從記憶體中移除。

試試把`Master`中對於`Slave`的參考設為`weak`，看看發生什麼事情。

```swift
class Slave {
    let master: Master
    
    init(master: Master) {
        self.master = master
        println("Slave init")
    }
    
    deinit {
        println("Slave deinit")
    }
}

class Master {
    weak var slave: Slave?
    
    init() {
        slave = Slave(master: self)
        println("Master init")
    }
    
    deinit {
        println("Master deinit")
    }
}

var master: Master! = Master()          //output: Slave init -> Slave deinit -> Master init
master.slave                            //return: nil
master = nil                            //outout: Master deinit
```

- 糟糕！奴隸產生的時候只有主人知道，由於`weak`的關係，所以一出生就被系統回收。
- 這個主人終生沒有奴隸可用`master.slave //return: nil`。

這個情況下，不是`Master`對`Slave`參考保持弱連結，而是`Slave`不允許保有對`Master`的強參考，所以`Slave`對`Master`的關係是永遠忠誠(`let`)卻不擁有(`unowned`)。

```swift
class Slave {
    unowned let master: Master
    
    init(master: Master) {
        self.master = master
        println("Slave init")
    }
    
    deinit {
        println("Slave deinit")
    }
}

class Master {
    var slave: Slave?
    
    init() {
        slave = Slave(master: self)
        println("Master init")
    }
    
    deinit {
        println("Master deinit")
    }
}

var master: Master! = Master()          //output: Slave init -> Master init
master = nil                            //output: Master deinit -> Slave deinit
```

- `Master`誕生前系統趕緊配給`Slave`。
- `Master`死去後系統馬上回收`Slave`。

誰說一個`Master`一輩子只能配一個`Slave`，下面範例展示更換`Slave`的過程。

```swift
class Slave {
    static var count: Int = 0

    unowned let master: Master
    var number: Int
    
    init(master: Master) {
        self.master = master
        self.number = ++Slave.count
        println("Slave\(self.number) init")
    }
    
    deinit {
        println("Slave\(self.number) deinit")
    }
}

class Master {
    var slave: Slave?
    
    init() {
        slave = Slave(master: self)
        println("Master init")
    }
    
    deinit {
        println("Master deinit")
    }
}

var master: Master! = Master()          //output: Slave1 init -> Master init
master.slave = Slave(master: master)    //output: Slave2 init -> Slave1 deinit
master.slave = Slave(master: master)    //output: Slave3 init -> Slave2 deinit
master = nil                            //output: Master deinit -> Slave3 deinit
```

- `Master`誕生前系統趕緊配給`Slave1`。
- `Master`想換個奴隸，系統配給`Slave2`並回收`Slave1`。
- `Master`想換個奴隸，系統配給`Slave3`並回收`Slave2`。
- `Master`死去後系統馬上回收`Slave3`。

<a name="Capture_Lists"></a>
## Capture Lists

還有一種 Reference Cycle 的情況是由 Closure 造成。請見範例。

```swift
class Foo {
    let name = "Foo"
    
    init() {
        println("Foo init")
    }
    
    deinit {
        println("Foo deinit")
    }

    lazy var info: ()->() = {
        println("my name is \(self.name)")
    }
}

var foo: Foo! = Foo()       //output: Foo init
foo.info()                  //output: my name is Foo
foo = nil                   //foo is keeped by closure
```

- `info`是`Foo`屬性，型別為`()->()`，透過`lazy`關鍵字延遲初始化時機，一直到被存取前才完成初始化。
- 不把`info`放在`init()`裡面初始化的理由是：類別必須先完成所有屬性初始化才能存取本身的屬性，但這個 Closure 裡面卻有存取`self.name`的 statement，會造成編譯失敗。
- 呼叫`foo.info()`的時候，Closure`info`保存`self`的參考。
- 當`foo`不再被外部參考，記憶體應該被釋放時，卻因為`self`被`info`參考無法釋放記憶體，造成 Reference Cycle。

解決方式是在 Closure 中將宣告`[unowned self]`，意味雖然 Closure 整個生命週期都可以存取到`self`，但不擁有`self`。範例如下：

```swift
class Foo {
    let name = "Foo"

    init() {
        println("Foo init")
    }
    
    deinit {
        println("Foo deinit")
    }
    
    lazy var info: ()->() = {
        [unowned self] in
        println("my name is \(self.name)")
    }
}

var foo: Foo! = Foo()       //output: Foo init
foo.info()                  //output: my name is Foo
foo = nil                   //output: Foo deinit
```

- 記得`[unowned self]`要放在 Closure 最前面，後面加上`in`。
- 當`foo=nil`失去外部參考，Closure 也不會參考`self`，`foo`的資源會系統釋放。

<a name="Complex_Case"></a>
## 更複雜的情況

最後再看一個更複雜的案例。`doCapture`這個 Closure 捕捉三個物件：

- `self` - 物件本身在 Closure 的生命週期中不會消失，使用`unowned self`。
- `foo` - 在 Closure 產生後可能被設為`nil`，使用`weak foo = self.foo`。
- `bar` - 物件初始化過程擁有的物件參考，在 Closure 的生命週期中不會變更或消失，使用`unowned bar = self.bar`。

```swift
class Foo {
    let name = "Foo"
    deinit { println("Foo is dead") }
}

class Bar {
    let name = "Bar"
    deinit { println("Bar is dead") }
}

class Qiz {
    let name = "Qiz"
    var foo: Foo?
    let bar: Bar
    
    init() {
        foo = Foo()
        bar = Bar()
    }
    
    deinit {
        println("Qiz is dead")
    }
    
    lazy var doCapture: ()->() = {
        [unowned self, weak foo = self.foo, unowned bar = self.bar] in
        println("\(self.name), \(foo?.name), \(bar.name)")
    }
}

var qiz: Qiz! = Qiz()   //return: {name "Qiz" {{name "Foo"}} {name "Bar"} nil}
qiz.doCapture()         //output: Qiz, Optional("Foo"), Bar
qiz = nil               //output: Qiz is dead -> Foo is dead -> Bar is dead
```

以下歸納使用`weak`與`unowned`的規則：

1. use weak capture if the class instance or property is an optional
2. use unowned if the class instance or property is non-optional and can never be set to nil
3. "you must ... use the in keyword, even if you omit the parameter names, parameter types, and return type"

1. 如果參考的物件或屬性為 Optional，使用`weak`。
2. 如果參考的物件或屬性不是 Optional (不可能是`nil`)，使用`unowned`。
3. 定義 Closure 要使用`in`關鍵字，即使忽略參數名稱、參數型別、回傳型別。