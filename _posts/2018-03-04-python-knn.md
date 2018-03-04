---
layout: post
title:  "[python] 練習通用的機器學習法"
date:   2018-03-04 03:04:15 +0800
categories: python
comments: true
---
#### 1. 蒐集數據
{: style="color:#3498DB;"}

在這裡我們直接使用 `sklearn 函數庫`中已有的數據來做練習

因為 `sklearn 函數庫`中提供了一些數據集供新手來練習用，想知道裡面提供那些數據可以看[這裡](http://scikit-learn.org/stable/datasets/index.html)

> 我們在這裡使用的是**鳶尾花 (iris)** 的數據集

<br/>

#### 2. 載入鳶尾花數據
{: style="color:#3498DB;"}

{% highlight python %}
from sklearn import datasets
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsClassifier

# Load 鳶尾花的數據集
iris = datasets.load_iris()

# 取得鳶尾花的屬性
iris_X = iris.data 

# 取得鳶尾花的類別
iris_Y = iris.target

# 印出 5 筆鳶尾花的屬性
print(iris_X[:5, :])

# 印出鳶尾花的類別
print(iris_Y)
{% endhighlight %}

- 執行程式後你會看到這樣的結果

![](https://i.imgur.com/zZ4iFRd.png)

- 結果中的 **5 個陣列**代表的便如下表所示

| 花萼长度 | 花萼宽度 | 花瓣长度 | 花瓣寬度
| -------- | -------- | -------- | ------- |
| 5.1     | 3.5     | 1.4     | 0.2 |
| 4.9 | 3.0 | 1.4 | 0.2 |
| 4.7 | 3.2 | 1.3 | 0.2 |
| 4.6 | 3.1 | 1.5 | 0.2 |
| 5.0 | 3.6 | 1.4 | 0.2 |
{:.table}

<br/>

- 結果中的 0 1 2 則代表 3 種花

> 但這些數據代表甚麼在這並不重要，重要的是後面的機器學習

<br/>

#### 3. 機器學習
{: style="color:#3498DB;"}

{% highlight python %}
from sklearn import datasets
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsClassifier

# Load 鳶尾花的數據集
iris = datasets.load_iris()

# 取得鳶尾花的屬性
iris_X = iris.data 

# 取得鳶尾花的類別
iris_Y = iris.target

# 印出 5 筆鳶尾花的屬性
# print(iris_X[:5, :])

# 印出鳶尾花的類別
# print(iris_Y)

# 將鳶尾花數據切割成訓練集和測試集，0.3 代表測試集的數據占總數據的 3 成
X_train, X_test, Y_train, Y_test = train_test_split(iris_X, iris_Y, test_size=0.3)

# 使用 KNN 的分類器
knn = KNeighborsClassifier()

# 將訓練集丟進去 knn 做訓練
knn.fit(X_train, Y_train)

# 觀看預測數據以及真實數據的差別
print("預測數據: ", knn.predict(X_test))
print("真實數據: ", Y_test)
{% endhighlight %}

- 輸出結果如下

![](https://i.imgur.com/bq8ZTnM.png)

> 最後預測出那筆數據可能屬於哪一類的鳶尾花

<br/>

#### 4. 討論
{: style="color:#3498DB;"}

從結果中可以看到，預測出來的數據**仍可能錯誤**，因為不管再精良的機器學習演算法都不可能做到 100% 正確率

此外，在這裡我們使用的是 `KNN 演算法`，但是不管你換成甚麼演算法，其做法都**大同小異**

<br/>

---
