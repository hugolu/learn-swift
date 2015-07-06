## Function

- [function 的格式](#format)
- [function 的多載](#overloading)
- [function 不限數量的參數](#variadic_parameters)
- [function 型別](#function_type)
- [function 當作參數傳遞](#function_as_parameter)
- [function 當作回傳值](#function_as_return)
- [巢狀 function](#nested_function)
- [function 與 optional](#function_and_optional)
- [curried function](#curried_function)

<a name="format"></a>
### function 的格式

最簡單的 function - 沒有參數，沒有回傳值。
```swift
func hi() {
    println("hello")
}

hi()    //output: hello
```

接受參數的 function。
```swift
func hi(name: String) {
    println("Hello, \(name).")
}

hi("John")    //output: Hello, John.
```

雙重名稱的參數，內部、外部使用。
```swift
func hi(friendName name: String) {
    println("Hello, \(name).")
}

hi(friendName: "John")    //output: Hello, John.
```

自動產生內外同名的參數。
```swift
func hi(#name: String) {
    println("Hello, \(name).")
}

hi(name: "John")    //output: Hello, John.
```

function 參數預設值 - 沒有傳入參數自動使用預設值。
```swift
func hi(name: String = "World") {
    println("Hello, \(name).")
}

hi(name: "John")    //output: Hello, John.
hi()                //output: Hello, world.
```

function 傳入參數為常數，若要修改要另外用 `var` 宣告。
```swift
func hi(name: String) {
    name = "World"      // error: cannot assign to 'let' value 'name'
    println("Hello, \(name).")
}
```
```swift
func hi(var name: String) {
    name = "World"
    println("Hello, \(name).")
}

hi("John")    //output: Hello, World.
```



<a name="overloading"></a>
### function 的多載

<a name="variadic_parameters"></a>
### function 不限數量的參數

<a name="function_type"></a>
### function 型別

<a name="function_as_parameter"></a>
### function 當作參數傳遞

<a name="function_as_return"></a>
### function 當作回傳值

<a name="nested_function"></a>
### 巢狀 function

<a name="function_and_optional"></a>
### function 與 optional

<a name="curried_function"></a>
### curried function