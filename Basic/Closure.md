## Closure

- [closure: 無名的 function](#function_without_name)
- [當作參數的 closure](#closure_as_parameter)
- [能省則省的 closure](#omitted_stuffs)

<a name="function_without_name"></a>
### closure: 無名的 function

觀察以下兩個範例。第一個使用`func`宣告 function，然後使用變數`say`儲存；第二的直接定義一個 closure 使用變數`say`儲存。closure 語法把 function 名稱右邊的參數列與回傳值放到`{}`的第一行
```swift
func hi() {
    println("hello world")
}

var say: ()->() = hi
say()   //output: hello world
```
```swift
var say: ()->() = { ()->() in
    println("hello world")
}

say()   //output: hello world
```
<a name="closure_as_parameter"></a>
### 當作參數的 closure

<a name="omitted_stuffs"></a>
### 能省則省的 closure