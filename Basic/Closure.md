## Closure

- [closure: 無名的 function](#function_without_name)
- [當作參數的 closure](#closure_as_parameter)
- [能省則省的 closure](#omitted_stuffs)

<a name="function_without_name"></a>
### closure: 無名的 function

觀察以下兩個範例。第一個使用`func`宣告 function，然後使用變數`say`儲存；第二個直接定義 closure 使用變數`say`儲存。closure 語法把 function 名稱右邊的參數列與回傳值放到`{}`的第一行然後加上`in`，其餘不變。
```swift
func hi() {
    println("hello world")
}

var say = hi
say()   //output: hello world
```
```swift
var say = { ()->() in
    println("hello world")
}

say()   //output: hello world
```
> `say`的型別能從`=`右邊推測得知，型別定義`()->()`可以省略。

<a name="closure_as_parameter"></a>
### 當作參數的 closure

closure 用法與 function 雷同，直接看範例。
```swift
func repeat(count: Int, action: ()->()) {
    for _ in 1...count {
        action()
    }
}

repeat(3, {
    ()->() in
    println("hello world")  //output: hello world (3 times)
})
```

<a name="omitted_stuffs"></a>
### 能省則省的 closure