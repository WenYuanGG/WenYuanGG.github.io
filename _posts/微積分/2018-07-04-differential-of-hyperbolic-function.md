---
layout: post
title:  "反雙曲函數的微分"
date:   2018-07-04
categories: [微積分]
comments: true
mathjax: true
---

以下要介紹常見的反雙曲函數的微分方法 (導函數) , 我會仔細撰寫他的詳細證明過程 ， 而再開始證明前 , 你還需要知道雙曲函數的微分以及一些不等式 ，我再下面都會一一說明

<br/>

### 你必須先知道的

#### 雙曲函數的微分

- $\frac{d}{dx}(sinh(x))=cosh(x)$

- $\frac{d}{dx}(cosh(x))=sinh(x)$

- $\frac{d}{dx}(tanh(x))=sech^2(x)$

- $\frac{d}{dx}(sech(x))=-sech(x)tanh(x)$

- $\frac{d}{dx}(csch(x))=-csch(x)coth(x)$

- $\frac{d}{dx}(coth(x))=-csch^2(x)$

<br/>

#### 雙曲函數恆等式

- $cosh^2(x)-sinh^2(x)=1$

<br/>

### 反雙曲函數詳細微分過程

#### 1. $sinh^{-1}(x)$ 的微分

令 $y=sinh^{-1}(x)\Rightarrow sinh(y)=x$

$\frac{dy}{dx}=\frac{1}{\frac{dx}{dy}}=\frac{1}{cosh(y)}$

$\because cosh(x)=\sqrt{1+sinh^2(y)}$

$\therefore \frac{1}{cosh(y)}=\frac{1}{\sqrt{1+sinh^2(y)}}$

最後再將 $x$ 代入 , 可得 $\frac{1}{\sqrt{1+x^2}}$

得證 $\Rightarrow \frac{d}{dx}(sinh^{-1}(x))=\frac{1}{\sqrt{1+x^2}}$

<br/>

#### 2. $cosh^{-1}(x)$ 的微分

令 $y=cosh^{-1}(x)\Rightarrow cosh(y)=x$

$\frac{dy}{dx}=\frac{1}{\frac{dx}{dy}}=\frac{1}{sinh(y)}$

$\because sinh(y)=\sqrt{cosh^2(y)-1}$

$\therefore \frac{1}{sinh(y)}=\frac{1}{\sqrt{cosh^2(y)-1}}$

最後再將 $x$ 代入 , 可得 $\frac{1}{\sqrt{x^2-1}}$

得證 $\Rightarrow \frac{d}{dx}(cosh^{-1}(x))=\frac{1}{\sqrt{x^2-1}}$

未完待續...