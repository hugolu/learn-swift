### if-else

條件判斷不需加`()`，要用也可以。
```swift
var num = -10
var abs: Int

if num > 0 {
    abs = num
} else {
    abs = -num
}
```

判斷式結果必須是 `Bool`。
```swift
var num = 0
if num {    //error: type 'Int' does not conform to protocol 'BooleanType'
}
```

複合判斷使用`&&`或`||`。
```swift
var weekend = true
var sunny = true
if weekend && sunny {
    var mood = "😄"
}
```

條件成立時，就算只執行一行程式也不能省略`{}`。
```swift
var num = 1
if true     //expectd '{' after 'if' condition
    num = 2
```

### switch

### for

### while-loop

### 指定範圍

### 字串比較