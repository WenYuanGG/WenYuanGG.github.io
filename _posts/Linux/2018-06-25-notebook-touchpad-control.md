---
layout: post
title:  "ubuntu 筆電觸控板問題"
date:   2018-06-25
categories: [Linux]
comments: true
---

由於硬體支援問題，某些 notebook 可能再安裝了 Linux 系統後，發現觸控板的開關沒辦法使用，造成你無法關閉觸控板而老是誤觸的問題，但是這個問題是可以藉由指令來解決的，繼續看下去

<br/>

#### 1. 按下 `ctrl + alt + t` 來開啟終端機

![](https://i.imgur.com/ydKIjvY.png)

<br/>

#### 2. 輸入指令

請輸入以下指令

```bash
# 用來關閉觸控板
synclient touchpadoff=1

# 用來開啟觸控板
synclient touchpadoff=0
```

- <b>關閉觸控板</b>

![](https://i.imgur.com/o0VNAXv.png)

- <b>開啟觸控板</b>

![](https://i.imgur.com/GkOxcMk.png)

學會了嘛？ 趕快去試試看吧~