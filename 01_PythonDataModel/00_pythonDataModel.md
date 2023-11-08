# Python data model

Python 最讚的一點就是他的其一致性。在使用 Python一段時間後，你能夠開始對你不熟悉的功能做出明智而正確的猜測。
但是，如果你在學習Python之前學過另一種物件導向的語言，你可能會對使用`len(collection)`而不是`collection.len()`感到奇怪。這種明顯的不規則性只是一個冰山的一角，當你正確理解它時，它就是我們所謂的Pythonic的關鍵。也就是我們要介紹的Python data model模型，如果我們使用正確的話，自己寫的 API 就會像是內建的函式一樣好用。

你可以把 Python data model 看作是Python作為一個框架。它正式規範了語言本身的基本組件的接口，比如序列(sequences)、函式、迭代器(iterators)、協程(coroutines)、類、上下文管理器(context managers)等等。

在使用框架時，我們花了很多時間編寫被框架(framework)調用的方法。當我們利用Python數據模型來構建新的類時，同樣的情況也發生。Python解釋器調用特殊方法來執行基本對象操作，通常由特殊語法觸發。特殊方法名始終以雙下劃線開頭和結尾。例如：

- `obj[key]` 等同於 `obj.__getitem__()` `__getitem__` 是一種特殊方法
- `my_collection[key]`，解釋器調用 `my_collection.__getitem__(key)`。
當我們希望我們的物件能支援基本的語言結構時，我們實現特殊方法(magic method)
下面我們會開始舉例說明：
