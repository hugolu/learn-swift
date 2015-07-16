# Optional

- [宣告、設定與讀取](#DeclartionGetSet)
- [檢查是否有值](#Check)
- [判斷取值](#OptionalBinding)
- [自動取值](#ImplicitlyUnwrappedOptional)
- [無值給預設值](#DoubleQuestionMark)

<a name="DeclartionGetSet"></a>
## 宣告、設定與讀取

沒有 Optional 的世界，必須特別定義“什麼”表示變數未初始化狀態，這個約定必須人為維護，不但編譯器無法幫忙檢查，開發者間也容易產生誤解。

```swift
var age:Int = -1  //年齡未定義，還是-1歲？！
```

### 宣告

- Optional 是個包裝型別的容器，有兩種狀態：一個是空無一物、一個就是型別的值。
- 型別後面加上問號`?`表示變數是個 Optional。型別與`?`間不可留空白。
- Optional 預設值為`nil`，宣告後不賦值即設為預設值。 

```swift
var age: Int?   			//return: nil
var name: String? = nil		//return: nil
```

### 設定、讀取

- 設定方式與一般賦值沒有差異。
- 取值必須加上驚嘆號`!`，強制解開包裝（*Force Unwrap*）。

```swift
var age: Int? = 18
age = age! + 1
```

<a name="Check"></a>
## 檢查是否有值

強制解開一個值為`nil`的 Optional，會讓程式爆炸。

``` swift
var age: Int?
age = age! + 1  //fatal error: unexpectedly found nil while unwrapping an Optional value
```

Optional 取值前要先判斷是否為nil。

```swift
var age: Int?

if age != nil {
    println("age: \(age!)")     //不會執行
}
```

<a name="OptionalBinding"></a>
## 判斷取值 (*Optional Binding*)

先判斷是否為`nil`，再用`!`取值方式太囉唆，判斷取值技巧將兩步驟合而為一。

```swift
var age: Int? = 20

if let ageNum = age {
    println("age=\(ageNum)") //output: age=20
}
```

- 若`age`有值，得到`ageNum=20`型別為`Int`，不需再透過`!`強制解開包裝。
- 若`age`無值，`{}`內程式不會被執行。

<a name="ImplicitlyUnwrappedOptional"></a>
## 自動取值 (*Implicitly Unwrapped Optional*)

- 如果變數（常數）一旦給值後不會再變回無值的狀態，可以利用自動取值的方式宣告。
- 型別宣告後面加上問號`!` 表示變數（常數）是 Implicitly Unwrapped Optional。

```swift
let name: String! = "Hugo"
var age: Int! = 18

age = age + 1
println("I'm \(name), \(age) years old.")   //output: I'm Hugo, 19 years old.
```

當然，如果欺騙程式，它會死給你看。

```swift
var age: Int! = nil
age = age + 1   //fatal error: unexpectedly found nil while unwrapping an Optional value
```

<a name="DoubleQuestionMark"></a>
## 無值給預設值

若希望在 Optional 無值時給預設值，可用雙問號`??`後面加預設值，這應該是三元運算的語法糖。

```swift
var age: Int? = nil

var hisAge = age ?? 18
var herAge = age != nil ? age! : 18
```