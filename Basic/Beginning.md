## 從頭開始

- [變數與常數](#Variable_Constant)
- [輸出](#ConsoleOutput)
- [註解](#Comment)
- [分號](#Semicolon)

<a name="Variable_Constant"></a>
### 變數與常數

宣告變數使用`var`，宣告常數使用`let`。如果變數宣告後不會再變更內容，可改宣告為常數，一來邏輯更清楚，二來能優化程式效能。

```swift
var a = 0
a = 1

let b = 0
b = 1       //error: cannot assign to 'let' value 'b'
```

一行內宣告多個常數或變數，可用`,`隔開。

```swift
var x = 1, y = 2, z = 3
let pi = 3.14159, e = 2.71828
```

Swift 支援 Unicode 字元。

```swift
let π = 3.14159
let 你好 = "你好世界"
let 🐶🐮 = "dogcow"
```

<a name="ConsoleOutput"></a>
### 輸出

Swift 使用 `print` 做不換行輸出，`println`做換行輸出。

```swift
println("hello")    //output: hello
println("world")    //output: world

print("hello")
print("world")      //output: helloworld
```

字串插值（string interpolation）讓輸出更方便。

```swfit
var num = 100
var str = "foobar"

println("\(str) \(num)")    //output: foobar 100
```

<a name="Comment"></a>
### 註解

```swift
// 這是單行註解

/* 這是一個,
   多行註解 */
```

<a name="Semicolon"></a>
### 分號

分號可有可無。要在一行內寫出多個statement用分號隔開，但會降低可讀性。

```swift
var x = 1
var y = 2; var z = 3
```
