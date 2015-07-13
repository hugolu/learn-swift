## 運算子

- [一元，二元和三元運算子](#Operator)
- [指派運算子](#Assignment)
- [數值運算子](#Arithmetic)
- [複合指派運算子](#CompoundAssignment)
- [比較運算子](#Comparison)
- [三元條件運算](#TernaryConditional)

<a name="Operator"></a>
### 一元，二元和三元運算子

* 一元運算子（*Unary Operator*）對單一操作物件操作，可做前綴（*Prefix*）或後綴（*Postfix*）。
* 二元運算子（*Binary Operator*）操作兩個操作物件，它們出現在兩個操作物件之間，是中綴（*Infix*）。
* 三元運算子（*Ternary Operator*）操作三個操作物件。

```swift
var a = 1               //=  assignment operator
var b = ++a             //++ unary prefix operator
var c = -a              //-  unary postfix operator
var d = b+c             //+  binary operator
true ? "😀" : "😢"     //?: ternary operator
```

<a name="Assignment"></a>
### 指派運算子

指派運算子 (*Assignment Operator*)，用來初始化或更新變數的值。

```swift
let b = 10
var a = 5
a = b   // a 現在等於 10
```

<a name="Arithmetic"></a>
### 數值運算子

數值運算子（*Arithmetic Operator*）如下：

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

<a name="CompoundAssignment"></a>
### 複合指派運算子

複合指派運算子 (*Compound Assignment Operator*) 把其他運算子和指派運算子(`=`)組合起來。

```swift
var a = 1
a += 2 // a 現在是 3
```

<a name="Comparison"></a>
### 比較運算子

比較運算子 (*Comparison Operator*) 如下：

* 等於 `==`
* 不等於 `!=`
* 大於 `>`
* 小於 `<`
* 大於等於 `>=`
* 小於等於 `<=`

```swift
1 == 1   // true
2 != 1   // true
2 > 1    // true
1 < 2    // true
1 >= 1   // true
2 <= 1   // false
```

<a name="TernaryConditional"></a>
### 三元條件運算

三元條件運算 (*Ternary Conditional Operator*) 簡潔地表達根據問題成立與否作出二選一的操作。例如`問題?答案1:答案2`，如果`問題`成立，回傳`答案1`的結果; 如果不成立，回傳`答案2`的結果。

```swift
var num = -10
var abs: Int

if num > 0 {                    //囉唆的寫法
    abs = num
} else {
    abs = -num
}

abs = num > 0 ? num : -num      //簡潔的寫法
```