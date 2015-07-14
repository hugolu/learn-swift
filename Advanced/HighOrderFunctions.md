# 高階函式

- [`sort()`](#sort)
- [`filter()`](#filter)
- [`map()`](#map)
- [`reduce()`](#reduce)
- [組合技](#chaining)

<a name="sort"></a>
## `sort()`

Swift 針對合乎`Comparable`協定的陣列提供排序能力，相關函式宣告如下：

```swift
func sorted(isOrderedBefore: (T, T) -> Bool) -> Array<T>
mutating func sort(isOrderedBefore: (T, T) -> Bool)
```

- `sorted()`回傳排序陣列結果，不影響原陣列內容。
- `sort()`回傳排序陣列結果，並將結果存回原陣列。

```swift
var numbers = [2, 4, 6, 8, 1, 3, 5, 7]

numbers.sorted(<)   //return: [1, 2, 3, 4, 5, 6, 7, 8]
numbers             //return: [2, 4, 6, 8, 1, 3, 5, 7]

numbers.sort(>)
numbers             //return: [8, 7, 6, 5, 4, 3, 2, 1]
```

只要`class`與`struct`繼承並實作`Comparable`協定，就能透過`<`或`>`決定排序結果為升冪或降冪。

下例示範依照各點與原點的距離做升冪排序。

```swift
struct Point {
    var x: Int
    var y: Int
}

extension Point: Comparable {}

func ==(a: Point, b: Point) -> Bool {
    return (a.x*a.x + a.y*a.y) == (b.x*b.x + b.y*b.y)
}

func <(a: Point, b: Point) -> Bool {
    return (a.x*a.x + a.y*a.y) < (b.x*b.x + b.y*b.y)
}

var p1 = Point(x: 1, y: 5)
var p2 = Point(x: 3, y: 3)
var p3 = Point(x: -4, y: 2)
var points = [p1, p2, p3]       //return: [{x 1, y 5}, {x 3, y 3}, {x -4, y 2}]
points.sort(<)                  //return: [{x 3, y 3}, {x -4, y 2}, {x 1, y 5}]
```

<a name="filter"></a>
## `filter()`

`filter()` 宣告如下：

```swift
func filter(includeElement: (T) -> Bool) -> Array<T>
```

- `includeElement` 表示傳入的 function 或 closure，用來判斷陣列個元素是否符合條件。
- `filter()` 回傳結果為陣列。


以下範例展示透過在 closure 回傳每個元素使否符合判斷式的結果，決定最後 `filter()` 回傳的元素集合。

```swift
var numbers = Array(1...8)

var evenNumbers = numbers.filter { (x) -> Bool in
    x % 2 == 0
}

var oddNumbers = numbers.filter { (x) -> Bool in
    x % 2 != 0
}

numbers         //return: [1, 2, 3, 4, 5, 6, 7, 8]
evenNumbers     //return: [2, 4, 6, 8]
oddNumbers      //return: [1, 3, 5, 7]
```

下例為將一、三象限的點過濾出來的範例。

```swift
struct Point {
    var x: Int
    var y: Int
}

var p1 = Point(x: 1, y: 1)
var p2 = Point(x: -1, y: 1)
var p3 = Point(x: -1, y: -1)
var p4 = Point(x: 1, y: -1)
var points = [p1, p2, p3, p4]

var quadrant_1 = points.filter { (p) -> Bool in
    return p.x > 0 && p.y > 0
}

var quadrant_3 = points.filter { (p) -> Bool in
    return p.x < 0 && p.y < 0
}

points          //return: [{x 1, y 1}, {x -1, y 1}, {x -1, y -1}, {x 1, y -1}]
quadrant_1      //return: [{x 1, y 1}]
quadrant_3      //return: [{x -1, y -1}]
```

<a name="map"></a>
## `map()`

`map()` 宣告如下：

```swift
func map<U>(transform: (T) -> U) -> Array<U>
```

- `transform` 表示傳入的 function 或 closure，用來轉換每個陣列的元素。輸入參數與回傳結果型別可以不同。
- `map()` 回傳結果為陣列。

以下示範如何透過`map()`將整數陣列轉化成兩倍數值與字串的陣列。

```swift
var numbers = Array(1...8)

var doubles = numbers.map { (n) -> Int in
    return n*2
}

var strings = numbers.map { (n) -> String in
    let digital = [0:"零", 1:"壹", 2:"貳", 3:"参", 4:"肆", 5:"伍", 6:"陸", 7:"柒", 8:"捌", 9:"玖"]
    return digital[n] ?? "啥"
}

numbers     //return: [1, 2, 3, 4, 5, 6, 7, 8]
doubles     //return: [2, 4, 6, 8, 10, 12, 14, 16]
strings     //return: ["壹", "貳", "参", "肆", "伍", "陸", "柒", "捌"]
```

接著示範`map()`如何將一組二度空間的點對 X 軸與 Y 軸做鏡射。

```swift
struct Point {
    var x: Int
    var y: Int
}
var points = [Point(x: 1, y: 2), Point(x: -2, y: -1)]

var mirror_x = points.map { (p) -> Point in
    return Point(x: -p.x, y: p.y)
}

var mirror_y = points.map { (p) -> Point in
    return Point(x: p.x, y: -p.y)
}

points      //return: [{x 1, y 2}, {x -2, y -1}]
mirror_x    //return: [{x -1, y 2}, {x 2, y -1}]
mirror_y    //return: [{x 1, y -2}, {x -2, y 1}]
```

<a name="reduce"></a>
## `reduce()`

`reduce()`宣告如下：

```swift
func reduce<U>(initial: U, combine: @noescape (U, T) -> U) -> U
```

- `reduce()`處理一組陣列，得到一個回傳值作為結果。
- `initial`表示`combine`處理函式的初始值。
- `combine`表示傳入的 function 或 closure，用來將上次遞迴的結果`U`與元素`T`結合，產生新的結果`U`將作為下次遞迴函式的輸入值。

以下示範透過`reduce()`將一組整數做總和，`total1`作法中規中矩，`total2`精簡語法拿掉 closure 型別宣告。

```swift
var numbers = Array(1...10)

var total1 = numbers.reduce(0, combine: { (sum, num) -> Int in  //return: 55
    return sum + num
})

var total2 = numbers.reduce(0) { $0 + $1 }                      //return: 55
```

接著示範`reduce()`如何得到一組二度空間的點對原點的距離和，`total1`作法中規中矩，`total2`精簡語法拿掉 closure 型別宣告。

```swift
import UIKit

struct Point {
    var x: Double
    var y: Double
}
var points = [Point(x: 1, y: 5), Point(x: 3, y: 3), Point(x: -4, y: 2)]

var total1 = points.reduce(0.0, combine: { (total, point) -> Double in   //return: 13.81379615571165
    return total + sqrt(point.x*point.x + point.y*point.y)
})

var total2 = points.reduce(0.0) { $0 + sqrt($1.x*$1.x + $1.y*$1.y) }     //return: 13.81379615571165
```

> `sqrt()`為平方根運算 (square root)。

<a name="chaining"></a>
## 組合技

- `sum_1`：將`filter`、`map`、`reduce`結果存成變數再傳遞給另一個函式處理。
- `sum_2`：透過 function chaining 技巧，將`filter`、`map`、`reduce`結果直接傳遞給另一個函式處理。雖然運算式變長，但依舊維持可讀性。

```swift
var numbers = Array(1...10)

var odds = numbers.filter { $0 % 2 != 0 }                                       //return: [1, 3, 5, 7, 9]
var triple = odds.map { $0 * 3 }                                                //return: [3, 9, 15, 21, 27]
var sum_1 = triple.reduce(0) { $0 + $1 }                                        //return: 75

var sum_2 = numbers.filter{ $0 % 2 != 0 }.map{ $0 * 3 }.reduce(0){ $0 + $1 }    //return: 75
```