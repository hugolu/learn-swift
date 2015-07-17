# Function

- [Function 的格式](#Format)
- [Function 不限數量的參數](#VariadicParameters)
- [Function 型別](#FunctionType)
- [Function 多載](#Overloading)
- [Function 當作參數傳遞](#FunctionAsParameter)
- [Function 當作回傳值](#FunctionAsReturn)
- [巢狀 Function](#NestedFunction)
- [Function 與 Optional](#FunctionAndOptional)
- [Curried Function](#CurriedFunction)

<a name="Format"></a>
## Function 的格式

使用`func`關鍵字定義 Function，接著宣告 Function 名稱，然後使用`()`宣告參數列表，如果有回傳值則使用`->`宣告回傳值的型別。

最簡單的 Function - 沒有參數，沒有回傳值。

```swift
func hi() {
    println("hello")
}

hi()    //output: hello
```

接受參數的 Function。

```swift
func hi(name: String) {
    println("Hello, \(name).")
}

hi("John")    //output: Hello, John.
```

雙重名稱的參數，內部、外部使用。

```swift
func hi(friendName name: String) {
    println("Hello, \(name).")
}

hi(friendName: "John")    //output: Hello, John.
```

參數前加`#`自動產生內外同名的參數。

```swift
func hi(#name: String) {
    println("Hello, \(name).")
}

hi(name: "John")    //output: Hello, John.
```

Function 參數預設值 - 沒有傳入參數自動使用預設值。參數預設值定義後自動產生內外同名的參數，呼叫時必須加上外部參數名。

```swift
func hi(name: String = "World") {
    println("Hello, \(name).")
}

hi(name: "John")    //output: Hello, John.
hi()                //output: Hello, world.
```

Function 傳入參數為常數，若要修改要另外用`var`宣告。

```swift
func hi(name: String) {
    name = "World"      // error: cannot assign to 'let' value 'name'
    println("Hello, \(name).")
}
```

```swift
func hi(var name: String) {
    name = "World"
    println("Hello, \(name).")
}

hi("John")    //output: Hello, World.
```

Function 內部改變參數值不影響外部。

```swift
func add(var num: Int) {
    num = num + 1
}

var number = 10
add(number)
println(number) //output: 10
```

Function 用`inout`修飾參數，函式內部改變參數值同時作用到傳入的變數，呼叫函式`inout`參數前要加`&`。

```swift
func add(inout num: Int) {
    num = num + 1
}

var number = 10
add(&number)
println(number) //output: 11
```

Function 可以有回傳值，透過`->`告知回傳值型別。

```swift
func hi() -> String {
    return "Hello, World."
}

hi()    //return: "Hello, World."
```

Function 沒有回傳值，以下三種方式意義相同。

```swift
func foo() {}
func bar() -> () {}
func baz() -> Void {}
```

<a name="VariadicParameters"></a>
## Function 不限數量的參數（*Variadic Parameters*）

- 不限數量參數必須放在參數列最後
- 最多只能有一個參數是不限數量參數

```swift
func hi(adjective: String, pets: String...) {
    for pet in pets {
        print("\(adjective)\(pet), ")
    }
}

hi("可愛的", "小狗", "小貓", "小兔"); //output: 可愛的小狗, 可愛的小貓, 可愛的小兔,
```

<a name="FunctionType"></a>
## Function 型別

Function 型別由參數型別與回傳型別共同定義。Function 型別也是型別的一種，也可以使用變數（常數）指向Function。

```swift
func add5(num: Int) -> Int {
    return num + 5
}

var add: (Int)->Int = add5
add(5)  //return: 10
```

> `add`變數型別為`(Int)->Int`，呼叫時必須加上`()`，括號內填上符合 Function 定義的參數。

<a name="Overloading"></a>
## Function 多載

Function Signature（簽名）包含函式名稱、參數名稱、參數型別、回傳型別。同名函式簽名不同可以同時存在。

### 同名函式，回傳型別不同 

```swift
func hi() {
    println("hello world")
}

func hi() -> String {
    return "hello world"
}

var hi_1: ()->() = hi
var hi_2: ()->String = hi

hi_1()  //output: hello world
hi_2()  //return: "hello world"
```

### 同名函式，參數型別不同

```swift
func hi(something: String) {
    println("hello \(something)")
}

func hi(something: Int) {
    println("hello \(something)")
}

hi(123)     //output: hello 123
hi("world") //output: hello world
```

### 同名函式，參數外部名稱不同

```swift
func hi(name something: String) {
    println("hello \(something)")
}

func hi(food something: String) {
    println("hello \(something)")
}

hi(name: "Hugo")    //output: hello Hugo
hi(food: "Banana")  //output: hello Banana
```

### 同名函式，參數數量不同

```swift
func hi(someone: String) -> String {
    return "Hello \(someone)"
}

func hi(someone: String, other: String) -> String {
    return "Hello \(someone) & \(other)"
}

hi("World")             //return: "Hello World"
hi("George", "Marry")   //return: "Hello George & Marry"
```

<a name="FunctionAsParameter"></a>
## Function 當作為參數傳遞

```swift
func foo() {
    print("foo")
}

func bar() {
    print("bar")
}

func repeat(count: Int, action: ()->()) {
    for _ in 1...count {
        action()
    }
}

repeat(3, foo)  //output: foofoofoo
repeat(3, bar)  //output: barbarbar
```

- `repeat`裡面for-in的計數器的值不重要，用底線`_`取代。
- `action`型別為`()->()`，沒有傳入參數，也沒有回傳值。
- 傳入`repeat`的第二個的參數必須合乎`()->()`的條件。

<a name="FunctionAsReturn"></a>
## Function 當作回傳值

```swift
func foo() -> String {
    return "foo"
}

func bar() -> String {
    return "bar"
}

func murmur(mood: Int) -> ()->String {
    return mood % 2 == 0 ? foo : bar
}

var doStuff: ()->String = murmur(5)
doStuff()   //return: "bar"
```

- `murmur()`回傳型別為 Function，定義為`()->String`，沒有傳入值，回傳值為`String`。
- `murmur()`回傳值的儲存變數型別定義為`()->String`。

<a name="NestedFunction"></a>
## 巢狀 Function

Function 包含 Function 的用法叫做 Nested Function。

```swift
func murmur(mood: Int) -> ()->String {
    func foo() -> String {
        return "foo"
    }
    
    func bar() -> String {
        return "bar"
    }
    
    return mood % 2 == 0 ? foo : bar
}

var doStuff: ()->String = murmur(5)
doStuff()   //return: "bar"
```

理論上外層 Function 執行完他擁有的 Context 就會消失，但 Nested Funtion 有個神奇的能力 - 回傳的 Function 可以存取以下資料：

1. 自己宣告的變數（常數）
2. 傳給自己的參數
3. 外層 Function 宣告的變數（常數）
4. 傳給外層 Function 的參數
5. 最外層的全域變數（常數）

```swift
let gloVar = "GV"
let gloCon = "GC"

func extFunc(extArg: String) -> (String)->String {
    var extVar = "EV"
    let extCon = "EC"

    func intFunc(intArg: String) -> String {
        var intVar = "IV"
        let intCon = "IC"
        
        return "access to \(intVar), \(intCon), \(intArg), \(extVar), \(extCon), \(extArg), \(gloVar), \(gloCon)"
    }
    
    return intFunc
}

var magicFunc = extFunc("EA")
magicFunc("IA") //return: "access to IV, IC, IA, EV, EC, EA, GV, GC"
```

<a name="FunctionAndOptional"></a>
## Function 與 Optional

Function 的參數與回傳值可以是 Optional。

```swift
func hi(name: String!) -> String? {
    return name != nil ? "hi, \(name)" : nil
}

hi("hugo")  //return: "hi, hugo"
hi(nil)     //return: nil
```

<a name="CurriedFunction"></a>
## Curried Function

Curried Function 就是直接呼叫 Nested Function 回傳的 Function，執行時會像以下範例呼叫`repeat`時串接兩個`()`，雖簡潔卻犧牲可讀性。

```swift
func repeat(times: Int) -> (String)->() {
    func say(message: String) {
        for _ in 1...times {
            print(message)
        }
    }
    return say
}

repeat(2)("Hello!")             //output: Hello!Hello!
```

另一種 Curried Function 定義方式如下，簡單來說就是把內外兩層 Function 混在一起做撒尿牛丸。呼叫`repeat`的方式和前一個範例有一點點不同，第二個`()`裡面要包含參數名稱，否則會編譯錯誤。

```swift
func repeat(times: Int)(message: String) -> () {
    for _ in 1...times {
        print(message)
    }
}

repeat(2)(message: "Hello!")    //output: Hello!Hello!
```

> 方便簡潔是把雙面刃，永遠記住 "Write Code for Humans not Machines"。
