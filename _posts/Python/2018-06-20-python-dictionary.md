---
layout: post
title:  "[Python] 學習使用詞典 (Dictionary)"
date:   2018-06-20
update:	2018-06-21
categories: [Python3]
comments: true
---

詞典 (Dictionary) 是由<b>鍵值 (Key) 和數值 (Value) 所組成的數對</b>, 每個 Key 都有其對應的 Value

<br/>

#### 宣告與建立詞典 (Dictionary)

```python
dict1 = {} # 宣告一個空詞典

# 自行建立一個詞典 (Value 不一定要是數字, 也可以是字串, 甚至是符號)
dict2 = {'Chinese': 90, 'English': 88, 'Math': 93, 'OS': 93}

print(dict1)
print(dict2)
```

> **宣告空集合 (Set) :** `set1 = set()`

> **宣告空詞典 (Dictionary) :** `dict1 = {}`

> 兩個宣告法不要搞錯囉!

<br/>

#### 從詞典中加入新項目或刪除項目

```python
dict1 = {}
dict2 = {'Chinese': 90, 'English': 88, 'Math': 93, 'OS': 93}

# 添加新項目進 dict1
dict1['Name'] = 'Chris'
dict1['Gender'] = 1
dict1['Phone'] = '123-4567'
dict1['E-mail'] = 'chenwy0806@gmail.com'

# 將 dict2 中 Key == 'OS' 的項目刪除
del dict2['OS']
print(dict2)

# 與 del 一樣可以刪除特定項目
dict1.pop('Name')
print(dict1)

# 刪除 dict1 的最後一項
dict1.popitem()
print(dict1)

# 將 dict1 的內容清空
dict1.clear()
print(dict1)
```


<br/>

#### 一些詞典常見的用法

- <b>判斷某鍵值是否存在於詞典中</b>

與 串列 數組 集合 一樣, 都可以使用 `in` 與 `not in` 來判斷項目有無存在其中

```python
dict1 = {}
dict2 = {'Chinese': 90, 'English': 88, 'Math': 93, 'OS': 93}

# 添加新項目進 dict1
dict1['Name'] = 'Chris'
dict1['Gender'] = 1
dict1['Phone'] = '123-4567'
dict1['E-mail'] = 'chenwy0806@gmail.com'

print('Name' in dict1)
print('Gender' not in dict1)
```

- <b>判斷兩詞典是否相同</b>

與 串列 數組 集合 一樣, 都可以使用 `==` 與 `!=` 來判斷兩詞典是否相同

```python
dict1 = {}
dict2 = {'Chinese': 90, 'English': 88, 'Math': 93, 'OS': 93}
dict3 = {'Chinese': 90, 'English': 88, 'Math': 93, 'OS': 93}

# 添加新項目進 dict1
dict1['Name'] = 'Chris'
dict1['Gender'] = 1
dict1['Phone'] = '123-4567'
dict1['E-mail'] = 'chenwy0806@gmail.com'

print(dict1)
print(dict2)
print(dict3)
print(dict1 == dict2)
print(dict2 == dict3)
print(dict1 != dict2)
print(dict2 != dict3)
```

- <b>詞典的其他用法</b>

```python
dict1 = {}
dict2 = {'Chinese': 90, 'English': 88, 'Math': 93, 'OS': 93}

# 添加新項目進 dict1
dict1['Name'] = 'Chris'
dict1['Gender'] = 1
dict1['Phone'] = '123-4567'
dict1['E-mail'] = 'chenwy0806@gmail.com'

print(len(dict1)) # 印出 dict1 的長度
print(dict1.keys()) # 印出 dict1 所有的 key
print(dict2.values()) # 印出 dict2 所有的 value
print(dict2.items()) # 印出 dict2 所有的項目
print(dict2.get('Chinese')) # 印出 dict2 中 'Chinese' 的 value

# 把 dict2 複製一份給 dict3
dict3 = dict2.copy()
print(dict3)

# 將 dict1 合併到 dict3
dict3.update(dict1)
print(dict3)
```

<br/>

#### 使用索引來印出詞典內容

```python
dict1 = {}
dict2 = {'Chinese': 90, 'English': 88, 'Math': 93, 'OS': 93}

# 添加新項目進 dict1
dict1['Name'] = 'Chris'
dict1['Gender'] = 1
dict1['Phone'] = '123-4567'
dict1['E-mail'] = 'chenwy0806@gmail.com'

print(dict1['Name'])
print(dict2['Math'])
```

- <b>也可以使用 `for` 迴圈來印</b>

```python
dict1 = {}
dict2 = {'Chinese': 90, 'English': 88, 'Math': 93, 'OS': 93}

# 添加新項目進 dict1
dict1['Name'] = 'Chris'
dict1['Gender'] = 1
dict1['Phone'] = '123-4567'
dict1['E-mail'] = 'chenwy0806@gmail.com'

print(dict1['Name'])
print(dict2['Math'])

for key in dict2:
	print('dict2[\'%s\'] = %d' %(key, dict2[key]))
```
