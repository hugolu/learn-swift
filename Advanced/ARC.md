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

<a name="Capture_Lists">
## Capture Lists

<a name="Complex_Case"></a>
## 更複雜的情況