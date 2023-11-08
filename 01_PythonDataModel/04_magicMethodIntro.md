# 特殊方法簡介

下圖紀錄了語言中基本集合類型的介面，並指出這些圖表中的所有類別都是抽象基類（ABCs）。此外，它提到了抽象基類和collections.abc模塊將在第13章中進行詳細介紹，目的是提供一個關於Python中重要集合介面的概觀，展示它們是如何使用特殊方法構建的。

Python中的一些頂級抽象基類（ABCs）每個都有一個特殊方法。Collection 抽象基類（在Python 3.6中新增）統一了每個集合應該實現的三個基本接口：

- Iterable：用於支援 for 迴圈、解包unpacking 和其他迭代形式。
- Sized：用於支援 len 內建函數。
- Container：用於支援 in 運算子。

Python並不要求具體的類別實際繼承自這些ABCs。任何實現了 __len__ 方法的類別都滿足了 Sized 介面的要求。

Collection 的三個非常重要的特化是：

- Sequence：正式化了像 list 和 str 這樣的內建類別的介面。
- Mapping：由 dict、collections.defaultdict 等實現。
- Set：代表 set 和 frozenset 這兩個內建類別的介面。

其中，只有 Sequence 具有 Reversible 的特性，因為序列支援其內容的任意排序，而映射和集合則不支援。
這些抽象基類提供了對不同類型的集合的統一介面，使開發人員能夠使用相似的方法和操作來處理不同的集合類型，從而提高了代碼的可讀性和可重用性。此外，它們還有助於確保實現的類別具有所需的最基本的行為。
Python語言參考手冊的“資料模型”章節列出了80多個特殊方法名稱。其中超過一半用於實現算術、位元和比較運算符。以下是提供的表格，以作為特殊方法名稱的概述。
表格 1-1 顯示了特殊方法名稱，不包括用於實現中置操作符或核心數學函數（如 abs）的方法。這些方法中的大多數將在本書中進行介紹，包括最近添加的方法，如異步特殊方法（例如Python 3.5中新增的 __anext__）以及類自定義鉤子 __init_subclass__（自Python 3.6開始）。
這些特殊方法允許Python類別自訂其行為，並定義如何處理不同操作，從而使開發人員能夠更靈活地使用Python的內建功能和運算符。這提供了更高的自訂性和可讀性，並使類別能夠適應不同的應用場景。这也是Python中的“魔法方法”或“特殊方法”的基礎。

字串/位元串表示：
__repr__：用於返回對象的官方字串表示。
__str__：用於返回對象的非正式字串表示。
__format__：用於自訂對象的格式化輸出。
__bytes__：用於將對象轉換為位元串。
__fspath__：用於返回對象的檔案路徑表示。

轉換為數字：
__bool__：用於定義對象的布林值。
__complex__：用於轉換對象為複數。
__int__：用於轉換對象為整數。
__float__：用於轉換對象為浮點數。
__hash__：用於計算對象的哈希值。
__index__：用於指定對象可用作序列的索引。

模擬集合：
__len__：用於返回對象的長度。
__getitem__：用於獲取對象的元素。
__setitem__：用於設定對象的元素。
__delitem__：用於刪除對象的元素。
__contains__：用於檢查對象是否包含特定值。

迭代：