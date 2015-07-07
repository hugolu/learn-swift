## Function

- [function 的格式](#format)
- [function 不限數量的參數](#variadic_parameters)
- [function 型別](#function_type)
- [function 的多載](#overloading)
- [function 當作參數傳遞](#function_as_parameter)
- [function 當作回傳值](#function_as_return)
- [巢狀 function](#nested_function)
- [function 與 optional](#function_and_optional)
- [curried function](#curried_function)

<a name="format"></a>
### function 的格式

最簡單的 function - 沒有參數，沒有回傳值。
```swift
func hi() {
    println("hello")
}

hi()    //output: hello
```

接受參數的 function。
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

自動產生內外同名的參數。
```swift
func hi(#name: String) {
    println("Hello, \(name).")
}

hi(name: "John")    //output: Hello, John.
```

function 參數預設值 - 沒有傳入參數自動使用預設值。
```swift
func hi(name: String = "World") {
    println("Hello, \(name).")
}

hi(name: "John")    //output: Hello, John.
hi()                //output: Hello, world.
```

function 傳入參數為常數，若要修改要另外用 `var` 宣告。
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

function 內部改變參數值不影響外部。
```swift
func add(var num: Int) {
    num = num + 1
}

var number = 10
add(number)
println(number) //output: 10
```

function 用`inout`宣告變數，函式內部改變參數值會同時作用到傳入的變數，呼叫函式`inout` 參數前要加`&`。
```swift
func add(inout num: Int) {
    num = num + 1
}

var number = 10
add(&number)
println(number) //output: 11
```

function 可以有回傳值，透過`->`告知回傳值的型別。
```swift
func hi() -> String {
    return "Hello, World."
}

hi()    //return: "Hello, World."
```

function 沒有回傳值可以這麼宣告。
```swift
func foo() {
}

func bar() -> Void {
}

func baz() -> () {
}
```

<a name="variadic_parameters"></a>
### function 不限數量的參數（*Variadic Parameters*）

- 不限數量參數必須放在餐數列最後
- 最多只能有一個參數是不限數量參數

```swift
func hi(adjective: String, pets: String...) {
    for pet in pets {
        print("\(adjective)\(pet), ")
    }
}

hi("可愛的", "小狗", "小貓", "小兔"); //output: 可愛的小狗, 可愛的小貓, 可愛的小兔,
```

<a name="function_type"></a>
### function 型別

function 型別由參數型別與回傳型別共同定義。function 型別也是型別的一懂，所以也可以使用變數指向function。
```swift
func add5(num: Int) -> Int {
    return num + 5
}

var add: (Int)->Int = add5
add(5)  //return: 10
```
> `add`變數型別為`(Int)->Int`，呼叫時加上`()`，括號內填上符合 function 定義的參數。

<a name="overloading"></a>
### function 的多載

function signature (簽名) 包含函式名稱、參數名稱、參數型別、回傳值。同名函式簽名不同可以同時存在。
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
```swift
func hi(something: String) -> String {
    return "hello \(something)"
}

func hi(something: String) -> Int {
    return count(something)
}

var str: String = hi("world")   //return: "hello world"
var cnt: Int = hi("world")      //return: 5
```

<a name="function_as_parameter"></a>
### function 當作參數傳遞

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

- `repeat`裡面for-in的計數器不重要，可用底線`_`取代。
- `action`型別為`()->()`，沒有傳入參數，也沒有回傳值。
- 傳入`repeat`的第二個的參數必須合乎`()->()`的條件。

<a name="function_as_return"></a>
### function 當作回傳值

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

- `murmur`回傳型別為 function，定義為`()->String`，沒有傳入值，回傳值為`String`。
- 儲存`murmur`回傳值的變數型別必須定義為`()->String`。

<a name="nested_function"></a>
### 巢狀 function

function 包含 function 的用法叫做 nested function。
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

理論上外層 function 執行完他擁有的 context 就會消失，但 nested funtion 有個神奇的能力 - 回傳的 function 可以存取以下資料：
1. 傳給自己的參數
2. 傳給外層function的參數
3. 自己宣告的變數（常數）
4. 外層function宣告的變數（常數）
5. 最外層的全域變數（常數）

<a name="function_and_optional"></a>
### function 與 optional

<a name="curried_function"></a>
### curried function