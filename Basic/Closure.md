# Closure

- [Closure：無名的 Function](#FunctionWithoutName)
- [接受參數的 Closure](#ClosureHasParameters)
- [當作參數的 Closure](#ClosureAsParameter)
- [能省則省的 Closure](#OmittedStuffs)

<a name="FunctionWithoutName"></a>
## Closure：無名的 Function

範例一，首先定義 Function，**然後**把它儲存在變數`hi`。

```swift
func greeting() {
    println("hello world")
}
var hi = greeting

hi()   //output: hello world
```

範例二，直接定義 Closure 儲存在變數`hi`。

```swift
var hi = { ()->() in
    println("hello world")
}

hi()   //output: hello world
```

- Closure 不包含 Function 名稱。
- Closure 語法把 Function 名稱右邊參數列與回傳型別放到`{`右邊然後加上`in`，其餘不變。
- 儲存 Closure 變數的型別能從`=`右邊的 Closure 內容推測得知，型別定義可省略。

<a name="ClosureHasParameters"></a>
## 接受參數的 Closure

Closure 接受參數的用法與 Function 雷同。

```swift
var hi = { (name: String)->() in
    println("hello \(name)")
}

hi("hugo")   //output: hello hugo
```

<a name="ClosureAsParameter"></a>
## 當作參數的 Closure

Closure 當作參數的用法與 Function 雷同。

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

上面`repeat`呼叫方式可讀性不好，可以把傳入的 Closure 拉到`()`後面，這個技巧稱作 Trailing Closure。

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

上面 Closure 內的`()->() in`可以省略，因為參數型別已定義在`action`上。

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

<a name="OmittedStuffs"></a>
## 能省則省的 Closure

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

以下範例省略回傳型別（回傳型別由`action`定義）。

```swift
repeat(3) {
    (index: Int, message: String) in
    return "第\(index)次說\(message)"
}
```

以下範例省略參數型別（參數型別由`action`定義）。

```swift
repeat(3) {
    index, message in
    return "第\(index)次說\(message)"
}
```

以下範例省略參數名稱（第一個參數名為`$0`, 第二個參數名為`$1`，以此類推）。

```swift
repeat(3) {
    return "第\($0)次說\($1)"
}
```

以下範例省略回傳動作`return`（Closure 最後一個 Statement 結果預設為回傳值，可不加`return`）。

```swift
repeat(3) {
    "第\($0)次說\($1)"
}
```

以下範例是最終精簡版本。

```swift
repeat(3) { "第\($0)次說\($1)" }
```