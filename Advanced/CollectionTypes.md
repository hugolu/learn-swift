# Collection

- [Array](#Array)
- [Dictionary](#Dictionary)
- [Set](#Set)
- [Tuple](#Tuple)

<a name="Array"></a>
## Array

Array 維護一組有序排列的資料，依照次序存取資料具有效能上的優勢。

### Array 簡介

- Array 由一對中括號`[]`組成，裡面定義元素。
- 使用`[]`加上位置對某元素取值。
- `[]`搭配`=`設定(取代)某元素。
- `[]`搭配 Range Operator 定義特定範圍的元素。

```swift
var array = [1, 3, 5]
array[0]                //return: 1
array[0] = 0            //return: 0
array                   //return: [0, 3, 5]
array[1...2] = [1, 2]   //return: [1, 2]
array                   //return: [0, 1, 2]
```

### Array 操作誤區

- Array 型別一旦決定就不能變更，設定不同型別的資料會造成編譯錯誤。
- 存取超出範圍也會造成編譯錯誤。

```swift
var array = [1, 3, 5]
array[0] = "x"  //error: Cannot assign a value of type 'String' to a value of type 'Int'
array[100]      //error: 但 xcode playground 沒有顯示錯誤訊息
```

### Array 型別

- 藉由`=`右邊的值組推測 Array 型別 (Type Inference)。
- 明確在`[`、`]`之間宣告儲存資料的型別。
- 使用 Generic 方式在`<`、`>`之間宣告 Array 元素的型別。

```swift
var array1 = [1, 2, 3]
var array2: [Int] = [1, 2, 3]
var array3: Array<Int> = [1, 2, 3]
```

### Array 宣告

建議使用簡單、清楚的方式，`array1`或`array6`是不錯的選擇。

```swift
var array1 = [Int]()
var array2 = Array<Int>()
var array3: [Int] = Array<Int>()
var array4: [Int] = Array()
var array5: [Int] = [Int]()
var array6: [Int] = []
var array7: Array<Int> = Array<Int>()
var array8: Array<Int> = Array()
var array9: Array<Int> = [Int]()
var array10: Array<Int> = []
```

### Array 操作

- `extend()`在原 Array 後面加上一個 Array，也可以使用`+`或`+=`做到一樣的事。
- `insert()`在 Array 任意位置插入新值。
- `count`表示 Array 內有賦值元素的數目。
- `capacity`表示 Array 內可操作元素的數目，當存取元素位置超過`capacity`範圍，Swfit 會重新產生一個更大的 Array，取代舊的 Array。
- 使用 Range Operator 取得某範圍的元素集合。
- `reverse()` 回傳反向排序元素的 Array，呼叫後不影響原來的 Array。
- `removeAtIndex()` 移除特定位置的元素。
- `removeLast()` 移除 Array 最後一個元素。
- `removeRange()` 移除某範圍的元素。
- 把`[]`賦值給 Array，表示清空全部元素。
- `isEmpty`判斷 Array 是否為空集合。

```swift
var array = [1, 2]          //return: [1, 2]

array.extend([3, 4])        //return: [1, 2, 3, 4]
array = array + [5, 6]      //return: [1, 2, 3, 4, 5, 6]
array += [7, 8]             //return: [1, 2, 3, 4, 5, 6, 7, 8]
array.insert(0, atIndex: 0) //return: [0, 1, 2, 3, 4, 5, 6, 7, 8]
array.append(9)             //return: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
array.count                 //return: 10
array.capacity              //retrun: 12
array[1...3]                //return: [1, 3, 4]
array.reverse()             //return: [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]

array.removeAtIndex(0)      //return: 0
array.removeLast()          //return: 9
array.removeRange(3...4)    //return: [1, 2, 3, 6, 7, 8]

array = []                  //return: 0 elements
array.isEmpty               //return: true
```

### Array 搭配 `for-in`

```swift
var numbers = Array(1...10)  //return: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
var total = 0
for number in numbers {
    total += number
}
total                       //return 55
```

<a name="Dictionary"></a>
## Dictionary

Dictionary 維護一群由<鍵,值>組合的資料，資料不依序儲存，依照<鍵>索引<值>效率高於其他集合型別。

### Dictionary 簡介 

- Dictionary 由一對中括號`[]`構成，裡面的元素以`<key>:<value>`方式存放。
- 使用`[<key>]`對某元素取值。
- `[<key>]`搭配=設定(取代)某元素。

```swift
var animals = ["Dog": 3, "Cat": 5, "Rabbit": 2]

animals["Cat"]      //return: 5
animals["Cat"] = 6  //return: 6
animals             //return: ["Dog": 3, "Cat": 6, "Rabbit": 2]
animals["Bird"]     //return: nil
animals["Bird"] = 1 //return: 1
animals             //return: ["Dog": 3, "Cat": 6, "Rabbit": 2, "Bird": 1]
```

### Dictionary 設定

- 藉由`=`右邊的值組推測 Dictionary 型別 (Type Inference)。
- 明確在`[`、`]`之間宣告儲存資料的型別。
- 使用 Generic 方式在`<`、`>`之間宣告 Dictionary 元素的型別。

```swift
var dict1 = ["Dog": 3, "Cat": 5, "Rabbit": 2]
var dict2: [String:Int] = ["Dog": 3, "Cat": 5, "Rabbit": 2]
var dict3: Dictionary<String, Int> = ["Dog": 3, "Cat": 5, "Rabbit": 2]
```

### Dictionary 宣告

建議使用簡單、清楚的方式，`dict1`或`dict6`是不錯的選擇。

```swift
var dict1 = [String:Int]()
var dict2 = Dictionary<String, Int>()
var dict3: [String:Int] = Dictionary<String, Int>()
var dict4: [String:Int] = Dictionary()
var diec5: [String:Int] = [String:Int]()
var dict6: [String:Int] = [:]
var dict7: Dictionary<String, Int> = Dictionary<String, Int>()
var dict8: Dictionary<String, Int> = Dictionary()
var dict9: Dictionary<String, Int> = [String:Int]()
var dict10: Dictionary<String, Int> = [:]
```

### Dictionary 操作

- 使用`[<key>]`取值，回傳型別為 optional (因為可能找不到對應的值)。
- `updateValue()`更新元素，由回傳值是否為`nil`得知作用對象是否存在。
- `removeValueForKey()`刪除元素，由回傳值是否為`nil`得知作用對象是否存在。
- 使用`[:]`清空 Dictionary。
- `isEmpty`判斷 Dictionary 是否為空集合。
- `count`表示 Dictionary 內有元素的數目。

```swift
var animals = ["Dog": 3, "Cat": 5, "Rabbit": 2]
animals["Dog"]                      //3 (type: Int?)
animals["Bear"]                     //return: nil

var oldValue = animals.updateValue(4, forKey: "Bird")   //return: nil
animals                             //return: ["Dog": 3, "Cat": 5, "Rabbit": 2, "Bird": 4]

animals.removeValueForKey("Cat")    //return: 5
animals                             //return: ["Dog": 3, "Rabbit": 2, "Bird": 4]

animals = [:]                       //0 key/value pairs
animals.isEmpty                     //return: true
animals.count                       //return: 0

```

<a name="Set"></a>
## Set

Set 維護一組不重複的資料，資料不依序儲存。

### Set 簡介

- 將 Array 當成`Set()`的參數，產生一個 Set。
- Set 是一種 generic 的集合資料，型別明確定義在`<>`內，或由 Array 型別推測。

```swift
var set1 = Set<Int>([1, 2, 2, 2, 3])            //return: {2, 3, 1}
var set2 = Set([1, 2, 2, 2, 3])                 //return: {2, 3, 1}
var set3: Set<Int> = Set<Int>([1, 2, 2, 2, 3])  //return: {2, 3, 1}
var set4: Set<Int> = Set([1, 2, 2, 2, 3])       //return: {2, 3, 1}
var set5: Set<Int> = [1, 2, 2, 2, 3]            //return: {2, 3, 1}
```

### Set 宣告

建議使用簡單、清楚的方式，`set1`或`set3`是不錯的選擇。

```swift
var set1 = Set<Int>()
var set2: Set<Int> = Set<Int>()
var set3: Set<Int> = Set()
```

### Set 操作

- `count`表示 Set 內元素數目。
- `isEmpty`判斷 Set 是否空集合。
- `insert()`插入元素，數值重複不影響集合。
- `remove()`移除元素，回傳移除元素，若元素不存在則傳回`nil`。
- `removeFirst()`移除第一個元素。
- `removeAll()`移除所有元素。

```swift
var set = Set([1, 2, 2, 4, 5, 6, 7])    //reutrn: {5, 6, 7, 2, 4, 1}
set.count                               //return: 6
set.isEmpty                             //return: false

set.insert(3)                           //return: {2, 4, 5, 6, 7, 3, 1}
set.remove(9)                           //return: nil
set.removeFirst()                       //return: 2
set                                     //return: {4, 5, 6, 7, 3, 1}
set.removeAll()
set.isEmpty                             //return: true
```
- `intersect()`傳回兩組 Set 交集的元素。
- `union()`傳回兩組 Set 聯集的元素。
- `exclusiveOr()`傳回兩組 Set 不交集的元素。
- `subtract()`傳回`set2`內不含`set1`的元素。

```swift
var set1 = Set([1, 2, 3])   //{2, 3, 1}
var set2 = Set([2, 3, 4])   //{2, 3, 4}
set2.intersect(set1)        //return: {2, 3}
set2.union(set1)            //return: {2, 3, 4, 1}
set2.exclusiveOr(set1)      //return: {4, 1}
set2.subtract(set1)         //retunr: {4}
```

<a name="Tuple"></a>
## Tuple

Tuple 是輕量化的 struct，常用於傳遞個數大於一的值組。

### Tuple 簡介

- 形式為`(<值1>, <值2>, <值3>)`。
- 元素取值依序使用`.0`, `.1`, `.2`。

```swift
var status = (404, "Not Found")                     //return: (.0 404, .1 "Not Found")
println("HTTP \(status.0), \(status.1)")            //return: "HTTP 404, Not Found"
```

- 為增加可讀性，可用`<鍵>:<值>`的方式為 Tuple 元素命名。
- 元素取值使用`.<鍵>`。

```swift
var status = (code: 404, message: "Not Found")      //return: (.0 404, .1 "Not Found")
println("HTTP \(status.code), \(status.message)")   //"HTTP 404, Not Found"
```

### Tuple 與 Dictionary 的應用

```swift
var animals = ["Dog": 3, "Cat": 5, "Rabbit": 2]

for (type, number) in animals {
    print("\(type) x\(number), ")
}
//output: Dog x3, Cat x5, Rabbit x2, 
```

### Tuple 與 Array 的應用

使用`enumerate()`將 Array `轉成 EnumerateSequence<Seq>` 型別，再透過 Tuple 取得元素位置與元素的值。

```swift
var fruits = ["Apple", "Banana", "Peach"]

for (index, item) in enumerate(fruits) {
    print("fruits[\(index)] \(item), ")
}
//output: fruits[0] Apple, fruits[1] Banana, fruits[2] Peach,
```

### Tuple 與 Function 的應用

Function 回傳的複數結果，若不想大費周章宣告`struct`包裝回傳值，可使用輕量化的 Tuple 取代。

```swift
func http() -> (Int, String) {
    return (404, "Not Found")
}

var (code, message) = http()
println("HTTP \(code), \(message)")  //output: "HTTP 404, Not Found"
```

### Tuple 與 Switch 的應用

使用 Tuple 捕捉 switch-case 中的值 (Value Binding)。

```swift
var pos = (10, 0)

switch pos {
case let (_, y) where y == 0:
    println("on x-axis")
case let (x, _) where x == 0:
    println("on y-axis")
default:
    break
}
```