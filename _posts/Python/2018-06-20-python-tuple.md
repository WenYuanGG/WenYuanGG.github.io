---
layout: post
title:  "[Python] 學習使用數組 (Tuple)"
date:   2018-06-20
categories: [Python3]
comments: true
---

Python 的數組 (Tuple) 跟串列 (List) 很類似, 但是他們兩者有以下幾點不同：

1. 數組的<b>元素值不可以更改</b>
2. 在數組中不可以刪除個別元素或取代數組中的資料 (要刪除也<b>只能把整個數組元素都刪掉</b>)
3. 數組不能使用 `append` 和 `insert` 來加入新元素, 但可以使用 `+` 來加入新元素或者使用 `*` 來複製元素

<br/>

#### 宣告與建立數組 (Tuple)
{: style="color:MediumSeaGreen;"}

```python
tuple1 = () # 建立空數組
tuple2 = (2, 4, 7, 5, 3, 3) # 以數值建立數組
tuple3 = tuple('Hello World') # 以字串建立數組

# 以串列建立數組
tuple4 = tuple([i for i in range(1, 10)])

print(tuple1)
print(tuple2)
print(tuple3)
print(tuple4)
```

<br/>

#### 數組可使用的函式
{: style="color:MediumSeaGreen;"}

數組 (Tuple) 也可以使用串列所使用的函式, 例如： `len()` , `max()` , `min()` , `sum()`

```python
tuple1 = () # 建立空數組
tuple2 = (2, 4, 7, 5, 3, 3) # 以數值建立數組
tuple3 = tuple('Hello World') # 以字串建立數組

# 以串列建立數組
tuple4 = tuple([i for i in range(1, 10)])

print(len(tuple3)) # 印出 tuple3 的長度
print(max(tuple2)) # 印出 tuple2 的最大值
print(min(tuple2)) # 印出 tuple2 的最小值
print(sum(tuple4)) # 印出 tuple4 的數值總和
```

<br/>

#### 判斷元素是否存在於數組中
{: style="color:MediumSeaGreen;"}

可以使用 `in` 與 `not in` 運算子來判斷該元素是否存在於數組中

```python
tuple1 = () # 建立空數組
tuple2 = (2, 4, 7, 5, 3, 3) # 以數值建立數組
tuple3 = tuple('Hello World') # 以字串建立數組

# 以串列建立數組
tuple4 = tuple([i for i in range(1, 10)])

print(2 in tuple2)
print('H' not in tuple3)
print(9 in tuple4)
```

> `in` 與 `not in` 成立時會回傳 True , 不成立時則回傳 False

<br/>

#### 在數組中添加新元素
{: style="color:MediumSeaGreen;"}

數組不能使用串列的 `append` 以及 `insert` 來添加新元素, 數組必須使用 `+` 來添加新元素, 如下所示：

```python
tuple1 = ()
tuple2 = (2, 4, 7, 5, 3, 3)

# 在 tuple1 中加入元素 1, 2, 3 
tuple1 += (1, 2, 3)

# 在 tuple2 中加入元素 20
tuple2 += (20,)

print(tuple1)
print(tuple2)
```

> **注意：** 只連結一個元素時, 要在後面加上逗號, 如上面程式第 8 行所示

此外, 數組與串列一樣也可以使用 `*` 來複製, 如下所示：

```python
tuple1 = ()
tuple2 = (2, 4, 7, 5, 3, 3)

# 在 tuple1 中加入元素 1, 2, 3 
tuple1 += (1, 2, 3)

# 在 tuple2 中加入元素 20
tuple2 += (20,)

print(tuple1)
print(tuple2)

# 多複製一次 tuple2
tuple3 = tuple2 * 2

print(tuple3)
```

<br/>

#### 利用索引來擷取元素
{: style="color:MediumSeaGreen;"}

數組與串列相同, 都可以使用索引 (Index) 來<b>擷取特定元素</b>, 如下所示：

```python
tuple1 = ()
tuple2 = (2, 4, 7, 5, 3, 3)

# 在 tuple1 中加入元素 1, 2, 3 
tuple1 += (1, 2, 3)

# 在 tuple2 中加入元素 20
tuple2 += (20,)

# 多複製一次 tuple2
tuple3 = tuple2 * 2

print(tuple1[1])
print(tuple3[3:6])
print(tuple2[-1])
print(tuple3[2:])
```

當然也可以使用 `for` 迴圈來印出數組資料

```python
tuple1 = ()
tuple2 = (2, 4, 7, 5, 3, 3)

# 在 tuple1 中加入元素 1, 2, 3 
tuple1 += (1, 2, 3)

# 在 tuple2 中加入元素 20
tuple2 += (20,)

# 多複製一次 tuple2
tuple3 = tuple2 * 2

print(tuple1[1])
print(tuple3[3:6])
print(tuple2[-1])
print(tuple3[2:])

for i in tuple2:
	print(i, end = ' ')
```

<br/>

#### 刪除數組內容
{: style="color:MediumSeaGreen;"}

數組<b>不可以</b>刪除特定或單一元素, 也無法修改值, 只能透過 `del` 來<b>刪除整個數組的資料</b>

```python
tuple1 = ()
tuple2 = (2, 4, 7, 5, 3, 3)

tuple1 += (1, 2, 3)
tuple2 += (20,)

tuple3 = tuple2 * 2

del tuple3 # 刪除 tuple3
```

