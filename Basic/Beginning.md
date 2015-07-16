# 從頭開始

- [變數與常數](#Variable_Constant)
- [輸出](#ConsoleOutput)
- [註解](#Comment)
- [分號](#Semicolon)

<a name="Variable_Constant"></a>
## 變數與常數

- 宣告變數（Variable）使用`var`。
- 宣告常數（Constant）使用`let`。
- 如果賦值後不會再變更內容，應宣告為常數，一來邏輯清楚，二來優化程式效能。

```swift
var a = 0
a = 1

let b = 0
b = 1       //error: cannot assign to 'let' value 'b'
```

一行內宣告多個常數或變數，使用`,`隔開。

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
## 輸出

Swift 使用`print`做不換行輸出，`println`做換行輸出。

```swift
println("hello")    //output: hello
println("world")    //output: world

print("hello")
print("world")      //output: helloworld
```

字串插值（String Interpolation）讓輸出更方便。

```swfit
var num = 100
var str = "foobar"

println("\(str) \(num)")    //output: foobar 100
```

<a name="Comment"></a>
## 註解

- Swift 支援單行、多行註解。
- Swift 支援巢狀註解（Nested Comments）。

```swift
// 這是單行註解

/* 這是一個
   多行註解 */

/* 巢狀註解開始
/*
這是註解A
*/
/*
這是註解B
*/
巢狀註解結束 */
```

> 註解其實會騙人 - "Let code speak, not comment."

<a name="Semicolon"></a>
## 分號

分號可有可無，若要在一行內寫出多個語句（Statement）要用分號隔開。這會降低可讀性，應避免這種寫法。

```swift
var x = 1; var y = 2; println("x=\(x), y=\(y)")    //output: x=1, y=2
```
