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

條件判斷不需加`()`，要用也可以。
```swift
var score = 0

switch score {
case 0:
    "白癡"  //執行
case 100:
    "天才"
default:
    "平凡人"
}
```

一定要包含所有`case`，或是使用`default`捕捉剩下的情況。
```swift
var score = 0

switch score {
case 0:
    "白癡"  //執行
case 100:
    "天才"
}   //error: switch must be exhaustive, consider adding a default clause
```

`case`不需`break`可自動跳出`switch`，`case`不做任何事必須使用`break`。使用`fallthrough`繼續執行下個`case`。

```swift
var score = 0

switch score {
case 0:
    fallthrough
case 100:
    "天才白痴一線間"    //執行
default:
    break
}
```

可比較多個條件。
```switch
var grade = 100

switch grade {
case 0, 100:
    "天才白痴一線間"    //執行
default:
    "平凡人"
}
```

可比較任何型別。
```swift
var fruit = "蘋果"

switch fruit {
case "蘋果":
    "好吃"              //執行
case "榴槤":
    "噁心"
default:
    "還好"
}
```

### for

### while-loop

### 指定範圍