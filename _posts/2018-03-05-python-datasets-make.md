---
layout: post
title:  "[Python] datasets 建立自己的數據集"
date:   2018-03-05 10:38:00 +0800
categories: python
comments: true
---
我在之前的[文章](https://wenyuangg.github.io/python/2018/03/04/python-knn.html)中有示範過如何**載入 datasets 中已有的數據**來練習使用機器學習, 而在這篇文章中要練習的是如何`建立自己的數據`

此外, 你可以在官方的 sklearn API 的介紹中看到他提供的所有可建立的數據類型： [sklearn API 請點我](http://scikit-learn.org/stable/modules/classes.html#module-sklearn.datasets)

<br/>

#### 建立自己的數據
{: style="color:#3498DB;"}

- 我們示範使用了 `make_regression()`來建立一筆線性迴歸數據
    - sample： 100
    - feature： 1
    - target： 1
    - noise： 1

- 不明白甚麼是**線性回歸**可以[看這裡](https://zh.wikipedia.org/wiki/%E7%B7%9A%E6%80%A7%E5%9B%9E%E6%AD%B8)

{% highlight python %}
from sklearn import datasets
from sklearn.linear_model import LinearRegression
import matplotlib.pyplot as plt

# 建立自己的數據 (線性迴歸數據)
X, y = datasets.make_regression(n_samples=100, n_features=1, n_targets=1, noise=1)

# 畫點
plt.scatter(X, y)

# show 出圖
plt.show()
{% endhighlight %}

- 執行結果

![](https://i.imgur.com/R6mREqS.png)

> 每次執行看到不一樣的圖是正常的, 因為他是按照我們的要求來隨機建立 100 個線性回歸樣本

<br/>

---

