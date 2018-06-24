---
layout: post
title:  "[Python] 字串處理"
date:   2018-06-21
categories: [Python3]
comments: true
---

在數據分析的時代, <b>處理字串的能力也是必備的</b>, 在這篇文章中會說明一些經常用到的字串處理方法, 而這些方法已經可以讓你處理大部分的問題, 以下待續

<br/>

#### 宣告以及建立字串

在 C/C++ 中, 字串需要使用<b>雙引號</b>括住 (字元使用<b>單引號</b>括住), 但是 Python 比較特殊, 它並沒有字元與字串之分, 因此你要用<b>單引號或雙引號都可以</b>

- <b>以下為建立空字串的方法</b>

```python
# 以下 3 個都是建立空字串的意思
str1 = str()
str2 = ''
str3 = ""

print(str1)
print(str2)
print(str3)
```

- <b>也可以直接初始化字串</b>

```python
str1 = 'Hello World!'

print(str1)
```

- <b>也可以使用 len max min 函數</b>



| 函數 | 功能 |
| -------- | -------- |
| `len(string)`  | 回傳字串長度 |
| `max(string)`  | 回傳字串中最大值 |
| `min(string)`  | 回傳字串中最小值 |

--

```python
str1 = 'Hello World!'

print(len(str1)) # 印出字串長度
print(max(str1)) # 印出字串最大值
print(min(str1)) # 印出字串最小值
```

<br/>

#### 利用索引來擷取某字元或某字串

```python
str1 = 'Hello World!'

print(str1[3])
print(str1[6:]) # 從索引 6 印到最後一個字
print(str1[-1]) # 印出最後一個字元
print(str1[2:7])
```

- <b>也可以利用 `for` 迴圈來印出字串</b>

```python
str1 = 'Hello World!'

print(str1[3])
print(str1[6:])
print(str1[-1])
print(str1[2:7])

for i in str1:
	print(i, end = '')
print()
```

<br/>

#### 連結與複製字串

- <b>將兩字串合在一起</b>

```python
str1 = 'Hello '
str2 = 'World!'
str3 = str1 + str2 # 將兩字串接在一起

print(str3)
```

- <b>複製字串</b>

```python
str1 = 'Hello '
str2 = 'World!'
str3 = str1 * 2 # 複製字串

print(str3)
```

<br/>

#### 判斷某字元 (or 字串) 是否存在於字串中

使用 `in` 與 `not in` 來判斷

```python
str1 = 'Hello '
str2 = 'World!'
str3 = str1 * 2

print('e' in str1)
print(str1 in str3)
print(str1 not in str3)
print('o' not in str2)
```

<br/>

#### 字串測試的方法

以下為一些常用的測試字串方法

| 方法 | 功能 |
| -------- | -------- |
| `isupper()` | 若字串為大寫字母組成, 就回傳 True |
| `islower()` | 若字串為小寫字母組成, 就回傳 True |
| `isspace()` | 若字串為空白字元組成, 就回傳 True |
| `isalnum()` | 若字串為字母和數字組成, 就回傳 True |
| `isalpha()` | 若字串為字母組成, 就回傳 True |
| `isdigit()` | 若字串為數字組成, 就回傳 True |
| `isidentifier()` | 若字串為識別字, 就回傳 True |

--

```python
str1 = 'Hello'
str2 = '5566'
str3 = 'Hello5566'
str4 = 'APPLE'
str5 = 'banana'
str6 = 'list'

print(str4.isupper())
print(str5.islower())
print(str3.isalnum())
print(str1.isalpha())
print(str2.isdigit())
print(str6.isidentifier())
```

> 這是很好用的功能, 可以判斷你的字串到底是由哪些東西所組成

<br/>

#### 如何去掉字串中的空白字元

在字串中很怕遇到空白字元來擾亂了格式, 因此 Python 也提供了幾種<b>去除空白字元的方法</b>, 如下

| 方法 | 功能 |
| -------- | -------- |
| `lstrip()` | 刪除字串左側空白 |
| `rstrip()` | 刪除字串右側空白 |
| `strip()` | 刪除字串兩側的空白 |

--

```python
str1 = '  This is an example.  '

print(str1.lstrip())
print(str1.rstrip())
print(str1.strip())
```

<br/>

#### 字串中的子字串

Python 提供了一些方法讓你可以在字串中找尋特定的子字串, 也可以計算某字串出現的次數

| 方法 | 功能 |
| -------- | -------- |
| `startswith(str)` | 若字串尾端為 str , 就回傳 True |
| `endswith(str)` | 若字串開頭為 str , 就回傳 True |
| `find(str)` | 找尋字串中出現 str 的最小索引值 |
| `rfind(str)` | 找尋字串中出現 str 的最大索引值 |
| `count(str)` | 計算 str 在字串中出現的次數 |

--

```python
str1 = 'This is an example.'

print(str1.startswith('This'))
print(str1.endswith('example.'))
print(str1.find('a'))
print(str1.rfind('a'))
print(str1.count('i'))
```

<br/>

#### 轉換字串

Python 也有提供方法可以將字串進行<b>轉換</b>

| 方法 | 功能 |
| -------- | -------- |
| `capitalize()` | 將字串的第一個字轉成大寫 |
| `lower()` | 將字串所有字元轉成小寫 |
| `upper()` | 將字串所有字元轉成大寫 |
| `title()` | 將字串中每一個單字的第一個字元轉成大寫 |
| `swapcase()` | 將字串中的大寫轉小寫, 小寫轉大寫 |
| `replace(str1, str2)` | 將字串中的 str1 以 str2 取代 |

--

```python
str1 = 'My name is Chris Chen. I am 21 years old.'

print(str1.capitalize())
print(str1.lower())
print(str1.upper())
print(str1.title())
print(str1.swapcase())
print(str1.replace('Chris', 'Peggy'))
```

<br/>

#### 如何將字串格式化

在做資料處理, 如果可以將字串以一定的格式排列好的話, 經常可以<b>增加資料的可視性</b>, 以下為 Python 所提供的字串格式化方法

| 方法 | 功能 |
| -------- | -------- |
| `ljust(width)` | 字串欄寬為 width , 並向左靠齊 |
| `rjust(width)` | 字串欄寬為 width , 並向右靠齊 |
| `center(width)` | 字串欄寬為 width , 並向中靠齊 |

--

```python
str1 = 'My name is Chris Chen.'

print('|' + str1.ljust(30) + '|')
print('|' + str1.rjust(30) + '|')
print('|' + str1.center(30) + '|')
```

- <b>也有切割字串的方法</b>

| 方法 | 功能 |
| -------- | -------- |
| `split(str)` | 每當字串遇到 str 就進行切割, 並將切下來的部份放進 list 中 |

--

```python
str1 = 'My name is Chris Chen.'
str2 = '192.168.43.43'
str3 = 'DF-664-897-99'

list1 = str1.split(' ')
print(list1)

list2 = str2.split('.')
print(list2)

list3 = str3.split('-')
print(list3)
```

<br/>

#### ASCII 與數字間的轉換

| 方法 | 功能 |
| -------- | -------- |
| `ord(char)` | 將字元 char 轉成 ASCII 對應的數字 |
| `chr(num)` | 將數字 num 轉成 ASCII 對應的字元 |

--

```python
num = ord('a')
print('ASCII \'a\' is integer', num)

char = chr(num)
print('Integer', num, 'is ASCII', char)
```
