# 通用型別 (Generic Types)

- [使用 Generic 的理由](#Why_Generic)
- [型別限制](#Type_Constraint)
- [擴充功能克服型別限制](#Extension)
- [Generic Struct](#Generic_Struct)
- [Generic Function](#Generic_Function)
- [Generic Class](#Generic_Class)

<a name="Why_Generic"></a>
## 使用 Generic 的理由

先來看看如果沒有 Generic Type，程式寫起來有多累人。下面範例中兩個 Function 功能相同，只是一個針對`Int`、一個針對`String`，如果將來還出現其他型別的應用，複製貼上的方式實在不夠聰明啊！

```swift
func swapTwoInts(a: Int, b: Int) -> (Int, Int) {
    return (b, a)
}
swapTwoInts(1, 2)               //return: (.0 2, .1 1)

func swapTwoStrings(a: String, b: String) -> (String, String) {
    return (b, a)
}
swapTwoStrings("foo", "bar")    //return: (.0 "bar", .1 "foo")
```

改成 Generic Function 不但型別通吃，更能避免複製貼上衍生的問題。

- 函式名稱後使用`<T>`宣告Generic Type用法。
- 原先函式中有關型別的關鍵字通通替換成`T`。

```swift
func swapTwoValues<T>(a: T, b: T) -> (T, T) {
    return (b, a)
}

swapTwoValues(1, 2)             //return: (.0 2, .1 1)
swapTwoValues(3.14, 2.71)       //return: (.0 2.71, .1 3.14)
swapTwoValues("foo", "bar")     //return: (.0 "bar", .1 "foo")
```

<a name="Type_Constraint"></a>
## 型別限制 (Type Constraint)

雖然 Generic 很強大，BUT! 有時候卻沒那麼神奇。下面範例試圖將`compareInt()`與`compareString()`改造成通用型的`compare()`，編譯器卻發出錯誤訊息。

```swift
func compareInt(a: Int, b: Int) -> Bool {
    return a > b
}

func compareString(a: String, b: String) -> Bool {
    return a > b
}

func compare<T>(a: T, b: T) -> Bool {
    return a > b    //error: error: binary operator '>' cannot be applied to two T
}
```

型別要能夠相互比較，必須繼承`Comparable`的協定。

```swift
func compare<T: Comparable>(a: T, b: T) -> Bool {
    return a > b
}

compare(1, 2)           //return: false
compare(3.14, 2.71)     //return: true
compare("foo", "bar")   //return: true
```

<a name="Extension"></a>
## 擴充功能克服型別限制

看起來只要繼承特定協定就能符合 Generic 的要求，但是下面範例編譯器又抱怨`Person`不支援`Comparable`。

```swift
func compare<T: Comparable>(a: T, b: T) -> Bool {
    return a > b
}

class Person {
    var age: Int
    init(age: Int) {
        self.age = age
    }
}

compare(Person(age: 35), Person(age: 20))   //error: error: cannot invoke 'compare' with an argument list of type '(Person, Person)'
```

沒有`Comparable`功能就來實作吧，只要補上能夠對`Person`做比較的運算子即可。

- 使用`extension`擴充`Person`的有關`Comparable`功能。
- 實作`==(a: Person, b: Person)`。
- 實作`<(a: Person, b: Person)`。
- 只要有`==`與`<`，就能延伸出其他比較運算子`>`、`<=`、`>=`。
- 實作全域性比較運算子的理由請參考[說明][1]：Note that we define these as global functions, not as methods on our Car class. Remember that function dispatch is based on the type-signature of the function arguments.
[1]: http://books.aidanf.net/learn-swift/protocols

```swift
func compare<T: Comparable>(a: T, b: T) -> Bool {
    return a > b
}

class Person {
    var age: Int
    init(age: Int) {
        self.age = age
    }
}

extension Person: Comparable {}

func ==(a: Person, b: Person) -> Bool {
    return a.age == b.age
}

func <(a: Person, b: Person) -> Bool {
    return a.age < b.age
}

var p1 = Person(age: 10)
var p2 = Person(age: 20)
compare(p1, p2)
```

能夠比較`Person`好像沒什麼，但當我們想排序陣列，`Comparable`就幫上很大的忙，讓程式簡潔又邏輯清楚，是不是很神奇呢？

```swift
var numbers = [1, 3, 2, 4]
numbers.sort(<)     //return: [1, 2, 3, 4]

var persons = [Person(age: 15), Person(age: 44), Person(age: 37)]
persons.sort(<)     //return: [{age 15}, {age 37}, {age 44}]
```

<a name="Generic_Struct"></a>
## Generic Struct

再看一次沒有 Generic Type 的世界。

```swift
struct IntQueue {
    var items = [Int]()
    
    mutating func enqueue(item: Int) {
        items.append(item)
    }
    mutating func dequeue() -> Int? {
        return items.count > 0 ? items.removeAtIndex(0) : nil
    }
}

var iq = IntQueue()
iq.enqueue(1)       //return: {[1]}
iq.enqueue(2)       //return: {[1, 2]}
iq.enqueue(3)       //return: {[1, 2, 3]}
iq.dequeue()        //return: 1
iq.dequeue()        //return: 2
iq.dequeue()        //retrun: 3
iq.dequeue()        //return: nil


struct StringQueue {
    var items = [String]()
    
    mutating func enqueue(item: String) {
        items.append(item)
    }
    mutating func dequeue() -> String? {
        return items.count > 0 ? items.removeAtIndex(0) : nil
    }
}

var sq = StringQueue()
sq.enqueue("foo")   //return: {["foo"]}
sq.enqueue("bar")   //return: {["foo", "bar"]}
sq.enqueue("qiz")   //return: {["foo", "bar", "qiz"]}
sq.dequeue()        //return: "foo"
sq.dequeue()        //return: "bar"
sq.dequeue()        //return: "qiz"
sq.dequeue()        //return: nil
```

套用 Generic Type 的`struct`人生變成彩色的。

```swift
struct Queue<T> {
    var items = [T]()
    
    mutating func enqueue(item: T) {
        items.append(item)
    }
    mutating func dequeue() -> T? {
        return items.count > 0 ? items.removeAtIndex(0) : nil
    }
}

var iq = Queue<Int>()
iq.enqueue(1)       //return: {[1]}
iq.enqueue(2)       //return: {[1, 2]}
iq.enqueue(3)       //return: {[1, 2, 3]}
iq.dequeue()        //return: 1
iq.dequeue()        //return: 2
iq.dequeue()        //retrun: 3
iq.dequeue()        //return: nil

var sq = Queue<String>()
sq.enqueue("foo")   //return: {["foo"]}
sq.enqueue("bar")   //return: {["foo", "bar"]}
sq.enqueue("qiz")   //return: {["foo", "bar", "qiz"]}
sq.dequeue()        //return: "foo"
sq.dequeue()        //return: "bar"
sq.dequeue()        //return: "qiz"
sq.dequeue()        //return: nil
```

<a name="Generic_Function"></a>
## Generic Function

定義 Function 接受特定類型的物件，並可搭配`where`額外限目標制物件必須符合哪些`protocol`。

`eat<T: Stuff where T: Eatable>(food: T)`要求食物要是個東西`Stuff`，不管好吃`Delicious`或難吃`Disgusting`只要能吃`Eatable`就可以。`Stone`沒有繼承`Eatable`，呼叫`eat()`會發現東西啃不下去。

```swift
protocol Eatable {}
protocol Delicious {}
protocol Disgusting {}

class Stuff {}
class Stone: Stuff {}
class Food: Stuff, Eatable {}
class DeliciousFood: Food, Delicious {}
class DisgustingFood: Food, Disgusting {}

func eat<T: Stuff where T: Eatable>(food: T) {}

eat(Food())
eat(DeliciousFood())
eat(DisgustingFood())
eat(Stone())    //error: cannot invoke 'eat' with an argument list of type '(Stone)'
```

<a name="Generic_Class"></a>
## Generic Class

Generic Class 實作方式與 Struct 大同小異，就差在繼承機制而已。

下面範例，雖然`ColorBall`繼承與實作了`Colorful`協定，但因為`Queue<T: Box where T: Colorful>`只接受`Box`物件，所以`ColorBall`就被擋在門口囉。

```swift
protocol Colorful {
    var color: String { get }
}

class Box {}
class ColorBox: Box, Colorful {
    var color: String
    init(color: String) {
        self.color = color
    }
}

class Ball {}
class ColorBall: Ball, Colorful {
    var color: String
    init(color: String) {
        self.color = color
    }
}

struct Queue<T: Box where T: Colorful> {
    var items = [T]()

    mutating func enqueue(item: T) {
        items.append(item)
    }
    mutating func dequeue() -> T? {
        return items.count > 0 ? items.removeAtIndex(0) : nil
    }
    func countColorItems(color: String) -> Int {
        return items.reduce(0) { $1.color == color ? $0 + 1 : $0 }
    }
}

var queue = Queue<ColorBox>()
queue.enqueue(ColorBox(color: "red"))
queue.enqueue(ColorBox(color: "blue"))
queue.enqueue(ColorBox(color: "red"))
queue.enqueue(ColorBox(color: "blue"))
queue.countColorItems("red")  //return: 2

queue.enqueue(ColorBall(color: "yellow"))   // error: cannot invoke 'enqueue' with an argument list of type '(ColorBall)'
```

> `reduce()`神奇用法請參考[連結][2]。
[2]: https://www.weheartswift.com/higher-order-functions-map-filter-reduce-and-more/