## 字符串是啥？

字符串可以看作是字符序列。

## 字符集

字符集是符号和文字组成的集合，在 OI 中，处理字符串时计算复杂度往往要考虑到字符集大小带来的常数影响。

举个栗子，如果一道题只包含'A' ~ 'Z' 意味着字符集大小是 26 。 如果再加上 '0' ～ '9' 字符集大小就变成了 36

计算复杂度时，字符集大小带来的常数往往要用 $\alpha$ 表示。

## 如何存字符串

可以开一个 `char` 数组 , 如 `char a[100]`

也可以用 `vector` 如  `vector<char> v`

同时 STL 中也提供了字符串容器 `std :: string` 
