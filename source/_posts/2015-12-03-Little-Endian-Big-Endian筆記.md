title: 'Little Endian & Big Endian筆記'
categories: [computer science]
tags: [computer-science]
date: 2015-12-03 15:56:53
---


endian指的是當物理上的最小單元比邏輯上的最小單元小時，邏輯單元對映到物理單元的排布關係。

<!-- more -->
實際的例子

如果你在文件上看到一個雙字組的data，Ex: long MyData=0x12345678，要寫到從0x0000開始的記憶體位址時。
如果是Big Endian的系統，
存到記憶體會變成 0x12 0x34 0x56 0x78，最高位元組在位址最低位元，最低位元組在位址最高位元，依次排列。
如果是Little Endian的系統，
存到記憶體會變成 0x78 0x56 0x34 0x12，最低位元組在最低位元，最高位元組在最高位元，反序排列。
比較的結果就是這樣：

|            | big-endian | little-endian |
| :--------: | :--------: | :------------: |
| 0x0000     | 0x12       | 0x78          |
| 0x0001     | 0x34       | 0x56          |
| 0x0002     | 0x56       | 0x34          |
| 0x0003     | 0x78       | 0x12          |

