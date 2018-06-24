---
layout: post
title:  "[Python] 檔案處理"
date:   2018-06-22
categories: [Python3]
comments: true
---

#### 使用 `open()` 函數開檔

`open()` 的函數雛型如下：

```python
variable = open(Filename, mode)
```

第一個參數 Filename 就是你想要開啟的檔案名稱

第二個參數 mode 則是檔案的運作屬性, 常用的運作屬性大致有以下幾種：

| 屬性 | 功能 |
| -------- | -------- |
| w | 寫入資料 |
| r | 讀出資料 |
| a | 附加資料 |
| wb | 寫入二進位檔案 |
| rb | 讀取二進位檔案 |
| ab | 附加資料近二進位檔案 |

一般的 `w` `r` `a` 屬性是用來讀寫文字檔案

`wb` `rb` `ab` 屬性則是用來讀寫二進位形式檔案

而檔案的常用操作有以下的幾種函數：

| 函數 | 功能 |
| -------- | -------- |
| `write()` | 用以寫入資料 |
| `read()` | 讀取檔案中的所有內容 (存成字串形式) |
| `readline()` | 從檔案中讀取一行資料 |
| `readlines()` | 讀取檔案中的所有內容 (存成串列形式) |
| `read(num)` | 從檔案中讀取 num 個字元 |
| `close()` | 關閉檔案 |

<br/>

#### 練習寫入資料

```python
# 開啟檔案 example.txt , 若不存在則自動建立
# 並將檔案屬性設為寫入
file = open('example.txt', 'w')

# 寫入資料
file.write('Hello world!\n')
file.write('I love Python.')

# 關閉檔案
file.close()
```

執行後會在同目錄下生成一個 `example.txt` 檔

![](https://i.imgur.com/ku0gIlu.png)

<br/>

#### 練習讀取資料

從剛剛建立的 `exmaple.txt` 來讀取

- <b>使用 `read()` 來讀取</b>

```python
# 開啟檔案 example.txt , 並將檔案屬性設為讀取
file = open('example.txt', 'r')

# 使用 read() 一次讀出所有內容
data = file.read()

print(data)

# 關閉檔案
file.close()
```

![](https://i.imgur.com/1vWTnUU.png)

- <b>使用 `readline()` 來讀取</b>

```python
file = open('example.txt', 'r')

# 使用 readline() 一次讀出一行
line1 = file.readline()
line2 = file.readline()

print(line1, end = '')
print(line2)

print()

# 使用 repr() 後, 會連同轉義字元都一起印出來 (例如 \n)
print(repr(line1))
print(repr(line2))

file.close()
```

![](https://i.imgur.com/B4lYE1c.png)

- <b>使用 `readlines()` 來讀取</b>

```python
file = open('example.txt', 'r')

data = file.readlines()

print(data)

file.close()
```

![](https://i.imgur.com/POQbLrz.png)

> 從上面結果你可以發現 `read()` 和 `readlines()` 雖然都是一次讀取全部資料, 但是 `read()` 是將讀出來的東西存成字串, 而 `readlines()` 是將讀出來的東西存成串列

<br/>

#### 練習附加資料

假設你要寫入的檔案中本身就有資料, 那麼你新寫入的東西將會把原本的資料覆蓋掉, 如果想要避免此情況發生, 你可以使用附加的方式

```python
file = open('example.txt', 'a')

file.write('\nHow are you today?')

file.close()
```

![](https://i.imgur.com/qtviaNd.png)

> 如此一來, 裡面原有的資料就不會被覆蓋掉

<br/>

#### 寫入與讀取一起做

- <b>方法一</b>

```python
# 開啟檔案, 設定為寫入模式
file = open('practice.txt', 'w')

# 寫入資料
file.write('how are you today?\n')
file.write('I\'m file. Thanks.')

#關閉檔案
file.close()

# 開啟檔案, 設定為讀取模式
file = open('practice.txt', 'r')

# 讀取資料
data = file.read()
print(data)

# 關閉檔案
file.close()
```

> 這種方法的效率比較差, 因為開關了兩次檔案, 建議使用方法二

- <b>方法二</b>

`w+` 模式代表可以同時寫檔與讀檔, 當然你換成 `r+` 或 `a+` 也可以得到一樣的效果

```python
file = open('practice.txt', 'w+')

file.write('how are you today?\n')
file.write('I\'m fine. Thanks.')

# 將檔案指標移動到檔案頭
file.seek(0, 0)

data = file.read()
print(data)

file.close()
```

> 方法二要特別注意, 在你寫完檔之後要開始讀檔時, 其檔案指標其實是指到檔案尾的, 所以你直接讀會讀不到資料, 必須先用 `seek(0, 0)` 將檔案指標移回檔案開頭

<br/>

#### 讀寫二進位檔案

如果你要讀取的檔案中, 大部分都是數值的話, 那麼會推薦你用二進位的方式來存取會比較有效率, 在使用前還必須先引進 `pickle` 模組, 而它的讀寫檔案函數如下：

| 函數 | 功能 |
| -------- | -------- |
| `dump()` | 寫入資料 |
| `load()` | 讀取資料 |

```python
import pickle

file = open('achievement.txt', 'wb+')

# 使用 dump 函數寫入資料
pickle.dump('Name\tChinese\tEnglish\tMath', file)
pickle.dump('Chris\t90\t74\t88', file)
pickle.dump('Peggy\t98\t99\t100', file)

# 將檔案指標移動到檔案頭
file.seek(0, 0)

# 使用 load 函數讀出資料
print(pickle.load(file))
print(pickle.load(file))
print(pickle.load(file))

# 關閉檔案
file.close()
```

執行後可以得到以下結果

![](https://i.imgur.com/ObvSh3w.png)

<br/>

#### `open()` 的其他寫法

先建立一個檔案叫 `exmaple.txt` , 內容如下：

![](https://i.imgur.com/l8gIpO2.png)

接著寫程式讀取它

```python
# open 的另一種寫法
with open('example.txt', 'r') as file:
	for line in file:
		print(line)

file.close()
```

執行結果

![](https://i.imgur.com/8TC1mtZ.png)

