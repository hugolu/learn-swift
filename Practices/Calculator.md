# Calculator

目標：這個練習透過解析文字得到四則運算的答案，不處理複雜的例外規則。

規格：使用 TDD（Test-Driven Development）方式，列出各階段的需求。

## 階段一規格

```swift
func calc(exp: String) -> Int? {
    if exp == "123" {
        return 123
    }
    if exp == "123+456-345" {
        return 234
    }
    return nil
}

func myAssert(exp: Bool, msg: String) {
    if exp == false {
        println("Assert: \(msg)")
    }
}

myAssert(calc("123") == 123, "123 = 123")
myAssert(calc("123+456-345") == 234, "123+456-345 = 234")
myAssert(calc("-123") == nil, "-123 = nil")
myAssert(calc("123++456") == nil, "123++456 = nil")
myAssert(calc("123+456+") == nil, "123+456+ = nil")
```

- `calc()`內的 pseudo-code 只是為了不發生 assertion，往後實作會替換成真的。
- 運算子不可重複出現。
- 運算子不可出現在運算式開頭或結尾。
- Swift 自帶的`assert()`常會把 playground 弄到死當，寫一個類似的`myAssert()`取代。

## 階段二規格

```swift
func calc(exp: String) -> Double? {
    if exp == "2x3" {
        return 6.0
    }
    if exp == "10/4" {
        return 2.5
    }
    if exp == "1+2x3" {
        return 7.0
    }
    return nil
}

func myAssert(exp: Bool, msg: String) {
    if exp == false {
        println("Assert: \(msg)")
    }
}

myAssert(calc("2x3") == 6, "2x3 = 6")
myAssert(calc("10/4") == 2.5, "10/4 = 2.5")
myAssert(calc("10%4") == nil, "10%4 = nil")
myAssert(calc("1+2x3") == 7, "1+2x3 = 7")
```

- 加入乘除功能，回傳型別變成`Double?`。
- 只支援四則運算（加減乘除）。
- 運算需合乎先乘除後加減的規則。

## 未完待續 ;)