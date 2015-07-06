### 一元，二元和三元運算子

* 一元運算子（*unary operator*）對單一操作物件操作，一元運算子分前綴運算子（*prefix*）和後綴運算子（*postfix*）。
* 二元運算子（*binary operator*）操作兩個操作物件，是中綴的（*infix*），因為它們出現在兩個操作物件之間。
* 三元運算子（*ternary operator*）操作三個操作物件。

```swift
var a = 1
var b = ++a         //unary prefix operator
var c = -a          //unary postfix operator
var d = b+c         //binary operator
a > d ? "😀" : "😢"  //ternary operator
```

### 指派運算子（*Assignment Operator*）

指派運算，用來初始化或更新變數的值。
```swift
let b = 10
var a = 5
a = b   // a 現在等於 10
```

### 數值運算子（*Arithmetic Operator*）

* 加法（+）
* 減法（-）
* 乘法（*）
* 除法（/）
* 餘數（%）
* 累加（++）
* 累減（--）
* 一元負號（-）
* 一元正號（+）
```swift
var x = 5
x + 2 //=7
x - 2 //=3
x * 2 //=10
x / 2 //=2
x % 2 //=1
++x   //=6
--x   //=5
+x    //=5
-x    //=-5
```

### 複合指派運算子（*Compound Assignment Operator*）

把其他運算子和指派運算（=）組合起來的複合指派運算子。
```swift
var a = 1
a += 2 // a 現在是 3
```

### 比較運算子（*Comparison Operator*）

* 等於（a == b）
* 不等於（a != b）
* 大於（a > b）
* 小於（a < b）
* 大於等於（a >= b）
* 小於等於（a <= b）
```swift
1 == 1   // true
2 != 1   // true
2 > 1    // true
1 < 2    // true
1 >= 1   // true
2 <= 1   // false
```

### 三元條件運算（*Ternary Conditional Operator*）

三元條件運算（`問題?答案1:答案2`）簡潔地表達根據問題成立與否作出二選一的操作。如果`問題`成立，回傳`答案1`的結果; 如果不成立，回傳`答案2`的結果。
```swift
var num = -10
var abs: Int

if num > 0 {
    abs = num
} else {
    abs = -num
}

abs = num > 0 ? num : -num
```