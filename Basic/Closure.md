## Closure

- [closure: 無名的 function](#function_without_name)
- [接受參數的 closure](#closure_has_parameters)
- [當作參數的 closure](#closure_as_parameter)
- [能省則省的 closure](#omitted_stuffs)

<a name="function_without_name"></a>
### closure: 無名的 function

觀察以下兩個範例。第一個使用`func`宣告 function，然後使用變數`say`儲存；第二個直接定義 closure 使用變數`say`儲存。closure 語法把 function 名稱右邊的參數列與回傳值放到`{}`的第一行然後加上`in`，其餘不變。

```swift
func greeting() {
    println("hello world")
}

var hi = greeting
hi()   //output: hello world
```

```swift
var hi = { ()->() in
    println("hello world")
}

hi()   //output: hello world
```

> `say`的型別能從`=`右邊推測得知，型別定義`()->()`可以省略。

<a name="closure_has_parameters"></a>
### 接受參數的 closure

closure 接受參數的用法與 function 雷同，直接看範例。

```swift
var hi = { (name: String)->() in
    println("hello \(name)")
}

hi("hugo")   //output: hello hugo
```

<a name="closure_as_parameter"></a>
### 當作參數的 closure

closure 當作參數的用法與 function 雷同，直接看範例。

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

上面`repeat`呼叫方式可讀性不好，可把要傳入的 closure 拉到`()`最後面，這個技巧稱作 trailing closure。

```swift
func repeat(count: Int, action: ()->()) {
    for _ in 1...count {
        action()
    }
}

repeat(3) {
    ()->() in
    println("hello world")  //output: hello world (3 times)
}
```

上面 closure 內的`()->() in`可以省略，因為傳入的參數型別已經定義在`action`上。

```swift
func repeat(count: Int, action: ()->()) {
    for _ in 1...count {
        action()
    }
}

repeat(3) {
    println("hello world")  //output: hello world (3 times)
}
```

<a name="omitted_stuffs"></a>
### 能省則省的 closure

先來個囉唆的完整版。

```swift
func repeat(count: Int, action: (Int, String) -> String) {
    for i in 1...count {
        action(i, "hello")
    }
}

repeat(3) {
    (index: Int, message: String) -> String in
    return "第\(index)次說\(message)"
}
//output: 第1次說hello
//output: 第2次說hello
//output: 第3次說hello
```

以下範例省略回傳型別。

```swift
repeat(3) {
    (index: Int, message: String) in
    return "第\(index)次說\(message)"
}
```

以下範例省略參數型別。

```swift
repeat(3) {
    index, message in
    return "第\(index)次說\(message)"
}
```

以下範例省略參數。

```swift
repeat(3) {
    return "第\($0)次說\($1)"
}
```

以下範例省略 return。

```swift
repeat(3) {
    "第\($0)次說\($1)"
}
```

以下範例是最終精簡版本。

```swift
repeat(3) { "第\($0)次說\($1)" }
```