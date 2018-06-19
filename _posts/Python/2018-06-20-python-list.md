---
layout: post
title:  "[Python] 學習使用串列 (List)"
date:   2018-06-20
categories: [Python3]
comments: true
---

串列 (List) 是一個儲存資料的容器, 如果你有學過 <b>C/C++ Java</b> 等其它語言的話, 你會發現他跟<b>陣列 (array)</b> 很類似, 只不過 Python 串列的功能更為強大, 可以同時<b>存放多種不同類型的資料</b>

<br/>

#### 練習使用一維串列
{: style="color:MediumSeaGreen;"}

一維陣列的形式如下：

![](https://i.imgur.com/VzFeNaw.png)


- <b>宣告串列</b>

```python
list1 = [] # 建立空串列

# 下面的存放資料方式都是正確的
list2 = [1, 2, 3, 4, 5]
list3 = ['chris', 'peggy', 'mary']
list4 = [1, 2, 3, 'chris', 'peggy', 'mary']

print(list1)
print(list2)
print(list3)
print(list4)
```

- <b>使用 index 印出特定項目</b>

```python
list1 = [] # 建立空串列

# 下面的存放資料方式都是正確的
list2 = [1, 2, 3, 4, 5]
list3 = ['chris', 'peggy', 'mary']
list4 = [1, 2, 3, 'chris', 'peggy', 'mary']

print(list1[0]) # 印出第 1 項
print(list1[3]) # 印出第 4 項
print(list1[0:4]) # 印出 1~4 項
print(list2[-1]) # 印出最後一項
print(list4[-2]) # 印出最後第二項
print(list4[2:]) # 從第三項印到最後一項
```

- <b>在串列插入新項目</b>

```python
list1 = [] # 建立空串列

# 下面的存放資料方式都是正確的
list2 = [1, 2, 3, 4, 5]
list3 = ['chris', 'peggy', 'mary']
list4 = [1, 2, 3, 'chris', 'peggy', 'mary']

# 把 'new' 字串加到 list2 的最後一項
list2.append('new')

# 把 4 加到 list4 的 index=3 的地方
list4.insert(3, 4)

print(list2)
print(list4)
```

- <b>從串列中刪除項目</b>

```python
list1 = [] # 建立空串列

# 下面的存放資料方式都是正確的
list2 = [1, 2, 3, 4, 5]
list3 = ['chris', 'peggy', 'mary']
list4 = [1, 2, 3, 'chris', 'peggy', 'mary']

# 刪除 list2 的最後一項
list2.pop()

# 刪除 list3 index=1 的項目
list3.pop(1)

# 刪除 list4 中, 值出現 'chris' 的第一項
list4.remove('chris')

print(list2)
print(list3)
print(list4)
```

- <b>使用 count 與 index</b>

`count(value)` 可以計算 `value` 在串列中<b>出現的次數</b> ; 而 `index(value)` 可以回傳 `value` 在串列中的<b>索引值</b>

```python
list1 = [] # 建立空串列

# 下面的存放資料方式都是正確的
list2 = [1, 2, 3, 4, 5]
list3 = ['chris', 'peggy', 'mary']
list4 = [1, 2, 3, 'chris', 'peggy', 'mary']
list5 = [1, 1, 1, 2, 2, 1, 3]

# 印出 list5 中出現 1 的次數
print(list5.count(1))

# 印出 list5 中, 值為 3 的索引值
print(list5.index(3))
```

- <b>將串列內容做排序 or 反轉</b>

```python
list5 = [1, 1, 1, 2, 2, 1, 3]

# 將他由小到大排序好
list5.sort()

print(list5)

# 將他的順序反轉
list5.reverse()

print(list5)
```

- <b>判斷項目是否存在於串列中</b>

```python
list5 = [1, 1, 1, 2, 2, 1, 3]

# 若 list5 中有 2 的話就回傳 True , 沒有就回傳 False
print(2 in list5)
print(2 not in list5)
```

- <b>計算最大最小值以及總和</b>

```python
list5 = [1, 1, 1, 2, 2, 1, 3]

print(max(list5)) # 印出最大值
print(min(list5)) # 印出最小值
print(sum(list5)) # 印出總和
print(len(list5)) # 印出 list5 的長度
```

- <b>串列的連結以及複製</b>

```python
list1 = [1, 2, 3]
list2 = [4, 5, 6]

# 將 list1 和 list2 兩串列合起來
list3 = list1 + list2
print(list3)

# 將 list1 多複製一次
list4 = list1 * 2
print(list4)
```

- <b>使用 for 迴圈來印出串列</b>

```python
List = ['Peter', 'Kevin', 'George', 'Chris', 'Peggy', 'Mary']

for i in range(len(List)):
    print('List[%i] = %s' %(i, List[i]))
```

<br/>

#### 練習使用二維串列
{: style="color:MediumSeaGreen;"}

由<b>多個一維串列所構成</b>, 架構如下：

![](https://i.imgur.com/7sLYG09.png)

- <b>宣告串列</b>

```python
# 宣告一個二維串列
List = [[1, 2, 3], [4, 5, 6]]

print(List)
print(List[0])
print(List[1])
```

- <b>將新項目加到二維串列中</b>

下面用數字以亂數產生

```python
import random

rows = int(input('Enter the amount of row: '))
columns = int(input('Enter the amount of column: '))
List = [] # 宣告一個一維串列

for i in range(rows):
    List.append([]) # 令其轉為二維串列形式

    for j in range(columns):
        # 將亂數產生的值放入串列中
        List[i].append(random.randint(1, 100))

print(List)
```

- <b>使用 for 迴圈來印出二維串列</b>

```python
import random

rows = int(input('Enter the amount of row: '))
columns = int(input('Enter the amount of column: '))
List = [] # 宣告一個一維串列

for i in range(rows):
    List.append([]) # 令其轉為二維串列形式

    for j in range(columns):
        # 將亂數產生的值放入串列中
        List[i].append(random.randint(1, 100))

# 使用 for 迴圈來印出串列
for i in range(len(List)):
    for j in range(len(List[i])):
        print('%5d' %(List[i][j]), end = '')
    print()
```