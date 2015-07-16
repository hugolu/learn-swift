# 基礎部分

- [從頭開始](Beginning.md)
- [型別](Types.md)
- [運算子](Operators.md)
- [流程控制](ControlFlow.md)
- [Optional](Optional.md)
- [Function](Function.md)
- [Closure](Closure.md)

先從程式語言最基礎的型別、運算子、流程控式開始介紹，這部分對有基礎的朋友可能稍嫌囉唆，建議快速瀏覽即可。

接下來的 Optional 算是 Swift 特有用法，它是從 Objective-C 的`nil`衍生而來，不過 Objective-C 的`nil`只適用於物件，Swift 的 Optional 則適用任何型別。

Function 構成 Swift 最基本、最關鍵的功能，讓大量程式碼可以重複使用。特別一提，Swift Function 的參考可以存到變數（常數）中，而這個變數（常數）的型別也就是 Function Type。

Closure 是無名的 function，通常在生命週期中 Function 只呼叫一次的情況下，不值得特別為那些程式碼正式寫一個 Function，這時候可以把一次性的程式碼放到 Closure 裡。