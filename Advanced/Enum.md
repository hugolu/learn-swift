### Enum

- [使用`enum`避免瑣碎的定義](#why_enum)
- [`enum`的`rawValue`](#rawValue)
- [`enum`搭配`switch`](#enum_switch)
- [Associated Value](#associated_value)
- [`enum`初始化函式](#enum_init)

<a name="why_enum"></a>
## 使用`enum`避免瑣碎的定義

根據史丹佛白鬍子教授[Paul][1]的發音，`enum`唸作"衣囊"。
[1]: https://itunes.apple.com/us/course/developing-ios-8-apps-swift/id961180099

先談談沒有`enum`的時候，如何使用變數(常數)表示共同認知的事物。假設我們要定義一些寵物種類，除了使用字串具體描述對象，最簡單的方式莫過於運用整數配合事先說好的規則(convention)。

- Dog = 1
- Cat = 2
- Rabbit = 3

```swift
var hisPet = 2  //表示是隻兔子
var herPet = 3  //但如果出現未定義的數值該怎麼辦？
```

使用`enum`不需要繁瑣的定義，程式自然能區別變數的意義。

```swift
enum Pet {
    case Dog
    case Cat
    case Rabbit
}

var hisPet = Pet.Dog
var herPet: Pet = .Cat
```

<a name="rawValue"></a>
## `enum`的`rawValue`

配合rawValue定義`enum`的值，可以讓`enum`自帶有意義的資訊。

```switch
enum Pet: String {
    case Dog = "🐶"
    case Cat = "🐱"
    case Rabbit = "🐰"
}

var myPet = Pet.Rabbit
println("my pet is a \(myPet.rawValue)")    //output: my pet is a 🐰
```

<a name="enum_switch"></a>
## `enum`搭配`switch`

`enum`最大的用處就是搭配`switch`。

```switch
enum Pet {
    case Dog
    case Cat
    case Rabbit
}

var myPet = Pet.Rabbit

switch myPet {
case .Dog: "is a dog"
case .Cat: "is a cat"
case .Rabbit: "is a rabbit"     //return: "is a rabbit"
default: break                  //not necessary
}
```

> 只要`case`涵蓋所有`enum`的可能性，`switch`最後不使用`default`捕捉剩餘可能也無妨。

<a name="associated_value"></a>
## Associated Value

Associated Value 讓`enum`功能更強大，除了`case`宣告的種類之外，還可以把值夾帶在`enum`變數中。先判斷變數屬於那個`case`，再透過`(let name)`取出夾帶在`enum`的值，用法請見範例。

```switch
enum Value {
    case 整數(Int)
    case 浮點(Float)
    case 字串(String)
}

func describe(value: Value) -> String {
    switch value {
    case .整數(let int):
        return "整數: \(int)"
    case .浮點(let float):
        return "浮點: \(float)"
    case .字串(let string):
        return "字串: \(string)"
    }
}

describe(Value.整數(1234))        //return: "整數: 1234"
describe(Value.浮點(3.14))        //return: "浮點: 3.14"
describe(Value.字串("hello"))     //return: "字串: hello"
```

<a name="enum_init"></a>
## `enum`初始化函式

`enum`跟`class`、`struct`一樣，都可以有初始化函式，不過應該沒人會這麼使用吧？

```switch
enum Value {
    case 整數(Int)
    case 浮點(Float)
    case 字串(String)
    
    init() {
        self = .字串("hello world")
    }
}

var value = Value()
```