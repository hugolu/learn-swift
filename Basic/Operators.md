### 一元，二元和三元運算子

* 一元運算子（*unary operator*）對單一操作物件操作，一元運算子分前綴運算子（*prefix operator*）和後綴運算子（*postfix operator*）。
* 二元運算子（*binary operator*）操作兩個操作物件，是中綴的（*infix*），因為它們出現在兩個操作物件之間。
* 三元運算子（*ternary operator*）操作三個操作物件。

```swift
var a = 1
var b = ++a         //unary prefix operator
var c = -a          //unary postfix operator
var d = b+c         //binary operator
a > d ? "😀" : "😢"  //ternary operator
```

