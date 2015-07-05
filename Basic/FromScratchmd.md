### 變數與常數

宣告變數使用`var`，宣告常數使用`let`。
```swift
var variable = 10
let constant = 20
```

一行內宣告多個常數或變數，可用`,`隔開。
```swift
var x = 1, y = 2, z = 3
let a = 1, b = 2, c = 3
```

Swift 支援 Unicode 字元。
```swift
let π = 3.14159
let 你好 = "你好世界"
let 🐶🐮 = "dogcow"
```

### 輸出

Swift 使用 `print` 做不換行輸出，`println`做換行輸出。
```swift
println("hello")    // 輸出 hello
println("world")    // 輸出 world

print("hello")
print("world")      // 輸出 helloworld
```

string interpolation 

### 註解

```swift
// 這是單行註解

/* 這是一個,
   多行註解 */
```

### 分號

分號可有可無。要在一行內寫出多個statement用分號隔開，但會降低可讀性。
```swift
var x = 1
var y = 2; var z = 3
```
