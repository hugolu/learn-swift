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

optional 設定方式與一般給值沒有差異。取值必須加上驚嘆號`!`，強制解開包裝 force-unwrap。
```swift
var age: Int? = 18
age = age! + 1
```

<a name="check"></a>
### 檢查是否有值

<a name="optional_binding"></a>
### 判斷並取值 (*optional binding*)

<a name="implicitly_unwrapped_optional"></a>
### 自動取值 (*implicitly unwrapped optional*)

<a name="double_question_mark"></a>
### 無值給預設值