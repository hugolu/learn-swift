## 基礎部分

先從程式語言最基礎的型別、運算子、流程控式開始介紹，這部分對有基礎的朋友可能稍嫌囉唆，建議快速瀏覽即可。

接下來的 optional 算是 swift 特有用法，它是從 objective-c 的`nil`衍生而來，不過 objective-c 的`nil`只適用於物件，swift 的 optional 則適用任何型別。

function 構成 swift 最基本、最關鍵的功能，讓大量程式碼可以重複使用。特別一提，swift function 的參考可以存到變數（常數）中，而這個變數（常數）的型別也就是 function type。

closure 是無名的 function，通常在生命週期中 function 只呼叫一次的情況下，不值得特別為那些程式碼寫一個 function，這時候可以把一次性的程式碼放到 closure 裡。