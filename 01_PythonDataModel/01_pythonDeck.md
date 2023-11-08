# 很 Pythonic 的撲克牌組

很 pythonic 的牌組

```python
import collections

Card = collections.namedtuple('Card', ['rank', 'suit'])

class FrenchDeck:
    ranks = [str(n) for n in range(2, 11)] + list('JQKA')
    suits = 'spades diamonds clubs hearts'.split()

    def __init__(self):
        self._cards = [Card(rank, suit) for suit in self.suits]

    def __len__(self):
        return len(self._cards)

    def __getitem__(self, position):
        return self._cards[position]

```

這個程式碼是在Python中建立一個卡牌的應用示例，它有一些很好的設計特點：

1. 使用namedtuple：通過使用collections.namedtuple，你可以輕鬆地創建一個輕量級的卡牌類型，並指定其屬性。這使代碼更加清晰和容易理解。
1. 定義FrenchDeck類：這個類代表一副法式撲克牌，並包括了撲克牌的花色和點數。類的結構使代碼組織有秩序，並提供了一個清晰的方式來管理撲克牌。
1. 使用__len__特殊方法：這個方法允許你通過len(deck)來獲取撲克牌的數量，使代碼更具可讀性。它還允許你使用內置函數len()來計算牌堆的大小。
1. 使用__getitem__特殊方法：這個方法允許你通過索引來訪問撲克牌，就像使用deck[index]一樣。這使你能夠以更自然的方式處理撲克牌，而不需要額外的方法。

```python
deck = FrenchDeck()
print(deck[0])  # Output: Card(rank='2', suit='spades')
print(deck[-1])  # Output: Card(rank='A', suit='hearts')
```

我們應該創建一個方法來選擇一張隨機卡片嗎？不需要。Python 已經具備了一個從序列中獲取隨機項目的函數：random.choice。我們可以在一個牌堆實例上使用它：

```python
from random import choice

print(choice(deck))  # Output: Card(rank='3', suit='hearts')
print(choice(deck))  # Output: Card(rank='K', suit='spades')
print(choice(deck))  # Output: Card(rank='2', suit='clubs')
```

我們剛剛看到使用特殊方法來利用Python數據模型的兩個優點：

- 使用這個 class 的人不必記住非標準操作的方法名稱。
例如，數量是要.size()、.length()還是其他什麼東西
- 更容易受益於豐富的Python標準庫，避免重新發明輪子，像是使用random.choice函數。

更厲害的是。 因為我們的__getitem__代理到self._cards的[]運算符，我們的牌堆自動支持切片。以下是我們如何查看一副全新牌堆的前三張卡片，然後只挑選梅花牌 (aces)，從索引12開始，每次跳過13張卡片：

```python
# 使用切片操作來查看牌堆中的前三張卡片
deck[:3]
# Output: [Card(rank='2', suit='spades'), Card(rank='3', suit='spades'), Card(rank='4', suit='spades')]

# 使用切片操作來挑選梅花牌 (aces)，從索引12開始，每次跳過13張卡片
deck[12::13]
# Output: [Card(rank='A', suit='spades'), 
# Card(rank='A', suit='diamonds'), Card(rank='A', suit='clubs'),
# Card(rank='A', suit='hearts')]
```

更不錯的事，只要實現__getitem__特殊方法，我們的牌堆也變成了可迭代的：

```python
# 遍歷牌堆中的每張卡片
for card in deck:
    print(card)
```

因為我們實現了__getitem__方法。這使得牌堆可以像標準的可迭代對象一樣使用，你可以使用for循環來迭代它。
同樣，我們還可以反向迭代：

```python
# 反向遍歷牌堆中的每張卡片
for card in reversed(deck):
    print(card)
```

迭代(Iteration)通常是隱式的。如果一個集合（collection）沒有__contains__方法，in運算符將執行順序掃描。這就是為什麼in運算符可以與我們的FrenchDeck類一起工作的原因，因為它是可迭代的。看一下下面的例子：

```python
# 檢查特定的卡片是否在牌堆中
print(Card('Q', 'hearts') in deck)  # Output: True
print(Card('7', 'beasts') in deck)  # Output: False
```

那如何排序呢？常見的卡片大小是按點數（Ａ為最高，2最小）排序，然後按花色順序排列，花色順序為黑桃>紅心>方塊>梅花。以下是一個按照這個規則排名卡片的函數，它返回2梅花的排名為0，黑桃A為51：

```python
def spades_high(card):
    rank_value = FrenchDeck.ranks.index(card.rank)
    return rank_value * len(suit_values) + suit_values[card.suit]
```

這邊 index 會回傳索引位置，也就是說，根據上面我們的 ranks

```python
ranks = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A']
```

如果我們調用ranks.index('Q')，他會回10，因為 'Q' 在列表中的索引位置是10（索引從0開始計算）。
然後我們就可以排序了

```python
# 使用排序函数sorted对牌堆进行排序，根据spades_high函数的规则
for card in sorted(deck, key=spades_high):
    print(card)
# Card(rank='2', suit='clubs')
# Card(rank='2', suit='diamonds')
# Card(rank='2', suit='hearts')
# ...
# Card(rank='A', suit='diamonds')
# Card(rank='A', suit='hearts')
# Card(rank='A', suit='spades')
```

- 這段文字描述了 FrenchDeck 類的設計，它繼承了 object 類，但大部分功能不是通過繼承實現的，而是通過實現特殊方法 __len__ 和 __getitem__ 來實現的。這兩個特殊方法使得 FrenchDeck 的行為類似於標準的 Python 序列（如列表或元組），因此它可以使用像迭代和切片等核心語言特性，同時還可以利用標準庫中的功能，如 random.choice、reversed 和 sorted。
- 通過組合，也就是將 FrenchDeck 中的部分功能委派給內部的 self._cards 列表對象，我們實現了 FrenchDeck 的核心功能，例如索引、切片、迭代等。這種組合方法使我們可以很容易地重用現有的 Python 道具（列表），而不必自己重新實現這些功能，從而實現了代碼的重用和簡化。這是 Python 中設計的一個重要原則之一，即“委派(delegate)而不是繼承”。

那我可以洗牌嗎？，截至目前，FrenchDeck 是不可變的，因此無法進行洗牌。卡片和它們的位置不能被修改，除非違反封裝原則並直接處理 _cards 屬性。作者在第 13 章中提到，將通過添加一行 setitem 方法來解決這個問題。
