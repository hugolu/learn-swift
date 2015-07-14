# 擴充型別

## Protocol

### Protocol 簡介

`protocol`規範對象應該具備的屬性與方法，適用於`class`、`struct`、`enum`。

- `protocol`只宣告不實做。
- 規範屬性可以是唯讀`{ get }`或可讀寫`{ get set }`，但不可唯寫。
- 規範方法方式如同定義 Function，不需加上`{}`。
- 可以使用 Protocol 型別的變數儲存物件，但只能使用 Protocol 定義的屬性與方法。

```swift
protocol Movable {
    var x: Int { get set }
    var y: Int { get set }
    func move(x: Int, y: Int)
}

class Point: Movable {
    var x: Int = 0
    var y: Int = 0

    func move(x: Int, y: Int) {
        self.x = x
        self.y = y
    }
}

var point: Movable = Point()    //return: {x 0 y 0}
point.move(10, y: 20)
point                           //return: {x 10 y 20}
```

### Protocol 多重繼承

Swift 不支援`class`多重繼承機制，但可以繼承多個`protocol`。

```swift
protocol Movable {
    var x: Int { get set }
    var y: Int { get set }
    func move(x: Int, y: Int)
}

protocol Resizable {
    var w: Int { get set }
    var h: Int { get set }
    func resize(w: Int, h: Int)
}

class Rect: Movable, Resizable {
    var x: Int = 0
    var y: Int = 0
    var w: Int = 10
    var h: Int = 10
    
    func move(x: Int, y: Int) {
        self.x = x
        self.y = y
    }

    func resize(w: Int, h: Int) {
        self.w = w
        self.h = h
    }
}

var m: Movable = Rect()
m.move(10, y: 20)
m.resize(20, h: 10)         //error: 'Movable' does not have a member named 'resize'

var r: Resizable = Rect()
r.resize(20, h: 10)
r.move(10, y: 20)           //error: 'Resizable' does not have a member named 'move'
```

同時繼承類別與協定，應先宣告繼承`class`，再宣告`protocol`。

```swift
protocol Movable {
    func move(x: Int, y: Int)
}

protocol Colorful {
    var color: String { get set }
}

class Point {
    var x: Int = 0
    var y: Int = 0
}

class ColorPoint: Point, Movable, Colorful {
    var color: String = "blue"
    func move(x: Int, y: Int) {
        self.x = x
        self.y = y
    }
}

var point = ColorPoint()    //return: {{x 0 y 0} color "blue"}
```

### Protocol 誤區

- 不可把`protocol`屬性宣告為常數。

```swift
protocol Moveable {
    let x: Int { get }  //error: 'let' declaration cannot be computed properties
}
```

## Extension

`extension`用來擴充`class`、`struct`、`enum`的能力。使用時機可能有：

- 沒有擴充對象的程式碼。
- 擴充類別使用`final`關鍵字。
- 不想讓擴充影響未來的使用者。

```swift
class Foo {
    func fun1() {}
}

extension Foo {
    func fun2() {}
}

var foo = Foo()
foo.fun1()
foo.fun2()
```

`extension`只能針對以下類型。

- computed property
- method
- init
- subscript
- protocol
- nested type

```swift
extension Int {
    var toString: String {
        get {
            var output: String = ""
            var num = self
            while num != 0 {
                switch num % 10 {
                    case 0: output = "零" + output
                    case 1: output = "壹" + output
                    case 2: output = "貳" + output
                    case 3: output = "參" + output
                    case 4: output = "肆" + output
                    case 5: output = "伍" + output
                    case 6: output = "陸" + output
                    case 7: output = "柒" + output
                    case 8: output = "捌" + output
                    case 9: output = "玖" + output
                    default: break
                }
                num /= 10
            }
            return output
        }
    }
    
    func times(action: ()->()) {
        for _ in 1...self {
            action()
        }
    }
    
    subscript(n: Int) -> Int {
        return self * n
    }
}

689.toString                //return: "陸捌玖"
3.times { print("Hello!") } //output: Hello!Hello!Hello!
3[4]                        //return: 12
```
