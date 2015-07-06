## Optional

- [使用 optional 的理由](#reasons)
- [宣告](#declartion)
- [設定與讀取](#get_set)
- [檢查是否有值](#check)
- [判斷並取值 (*optional binding*)](#optional_binding)
- [自動取值 (*implicitly unwrapped optional*)](#implicitly_unwrapped_optional)
- [無值給預設值](#double_question_mark)

<a name="reasons"></a>
### 使用 optional 的理由

optional 是個包裝型別的容器，有兩種狀態：一個是空無一物、一個就是型別的值。

以下範例使用`-1`表示`age`未初始化的狀態，容易產生誤解。
```swift
var age:Int = -1
```

<a name="declartion"></a>
### 宣告

型別後面加上問號 `?` 表示變數是個 optional。optional 初始可以設定為 `nil`。
```swift
var age: Int?   //nil
var name: String? = nil
```
> `Int` 與 `?` 之間不可留空白。

<a name="get_set"></a>
### 設定與讀取

optional 設定方式與一般給值沒有差異。取值必須加上驚嘆號`!`，強制解開包裝（*force-unwrap*）。
```swift
var age: Int? = 18
age = age! + 1
```

<a name="check"></a>
### 檢查是否有值

強制解開一個 `nil` 的optional，會讓程式炸掉。
``` switch
var age: Int?
age! //fatal error: unexpectedly found nil while unwrapping an Optional value
```

optional 取值前要先判斷是否為nil。
```swift
var age: Int?

if age != nil {
    println("age: \(age!)")     //不會執行
}
```

<a name="optional_binding"></a>
### 判斷並取值 (*optional binding*)

先判斷是否為 `nil`，再使用 `!` 取值方式太囉唆，optional binding技巧將兩步驟合併為一個。
```swift
var age: Int? = 20

if let ageNum = age {
    println("age=\(ageNum)") //output: age=20
}
```
* 若 age 有值，得到 ageNum=20 型別為 `Int`，使用 ageNum 不需透過 `!` 強制解開包裝。
* 若 age 無值，`{}`內程式不會被執行。

<a name="implicitly_unwrapped_optional"></a>
### 自動取值 (*implicitly unwrapped optional*)

如果變數或常數一旦給值後就不會再變回無值的狀態，可以利用自動取值的方式宣告。型別後面加上問號 `!` 表示變數是個 implicitly unwrapped optional。
```swift
let name: String! = "Hugo"
var age: Int! = 18

age = age + 1
println("I'm \(name), \(age) years old.")   //output: I'm Hugo, 19 years old.
```

當然，如果你欺騙程式，它會死給你看。
```swift
var age: Int! = nil
age = age + 1   //fatal error: unexpectedly found nil while unwrapping an Optional value
```

<a name="double_question_mark"></a>
### 無值給預設值