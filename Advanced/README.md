# 進階部分

- [Class](Advanced/Class.md)
- [Struct](Advanced/Struct.md)
- [Enum](Advanced/Enum.md)
- [集合型別](Advanced/CollectionTypes.md)
- [型別轉型](Advanced/Typecasting.md)
- [擴充型別](Advanced/ProtocolExtension.md)
- [通用型別](Advanced/Generic.md)
- [高階函式](Advanced/HighOrderFunctions.md)
- [記憶體管理](Advanced/ARC.md)

首先介紹功能強大的複合型別`class`、`struct`、`enum`，這幾個型別看起來有幾分相似但適用情況不同。

然後介紹幾個好用又強大的集合型別`Array`、`Dictionary`、`Set`、`Tuple`，前三者是由`struct`衍伸出來的通用型別，最後一個是 Swift 語言提供的輕量、靈巧的瑞士小刀。

Swift 是個~~嚴重潔癖~~語法嚴謹的語言，在 Type Inference 幫助下，強型別且不允許轉型對於程式強健性有極大幫助。但某些情況依然需要轉型的幫助，Swift 藉由`as!`與`as?`的幫助，在不犧牲嚴謹前提下提供最大限度的彈性。

協定`protocol`在某些語言又稱介面`interface`，只定義不實做，試圖避開多重繼承的混水；型別擴充`extension`讓已定義好的`class`、`struct`、`enum`在不修改原始程式情況下，得以擴充屬性或方法。

通用型別提升程式重用性，相同的邏輯針對不同的型別，只要一份程式碼就能搞定。

高階函式`sort()`、`filter()`、`map()`、`reduce()`看來神奇，只要看清楚分解動作，還是能輕鬆駕馭。

ARC 記憶體管理是最後的重頭戲，不懂這個程式還是能執行，但有可能因為沒釋放不再使用的記憶體，被系統砍掉而閃退。做好記憶體管理是尊重別人、也是對自己負責 (咦?)