---
layout: post
title:  "反三角函數的微分"
date:   2018-07-04
categories: [微積分]
comments: true
mathjax: true
---

以下要介紹常見的反三角函數的微分方法 (導函數) , 並會仔細撰寫其詳細過程 , 而再開始證明之前 , 你還需要先知道三角函數的微分以及一些常用的三角不等式 , 我再下面都會一一說明

<br/>

### 你必須先知道的

#### 三角函數的微分

- $\frac{d}{dx}(sin(x))=cos(x)$

- $\frac{d}{dx}(cos(x))=-sin(x)$

- $\frac{d}{dx}(tan(x))=sec^2(x)$

- $\frac{d}{dx}(cot(x))=-csc^2(x)$

- $\frac{d}{dx}(sec(x))=sec(x)tan(x)$

- $\frac{d}{dx}(csc(x))=-csc(x)cot(x)$

<br/>

#### 三角恆等式

- $sin^2(x)+cos^2(x)=1$

- $tan^2(x)+1=sec^2(x)$

- $1+cot^2(x)=csc^2(x)$

> 上面的三角函數微分和三角恆等式是身為通訊人一定要背的滾瓜爛熟的式子 , 只要常用就一定會記得了

<br/>

### 反三角函數詳細微分過程

#### 1. $sin^{-1}(x)$ 的微分

令 $y=sin^{-1}(x)\Rightarrow sin(y)=x$

$\frac{dy}{dx}=\frac{1}{\frac{dx}{dy}}=\frac{1}{cos(y)}$

$\because cos(y)=\sqrt{1-sin^2(y)}$

$\therefore \frac{1}{cos(y)}=\frac{1}{\sqrt{1-sin^2(y)}}$

最後再將 $x$ 代入 , 可得 $\frac{1}{\sqrt{1-x^2}}$

得證 $\Rightarrow \frac{d}{dx}\{sin^{-1}(x)\}=\frac{1}{\sqrt{1-x^2}}$

<br/>

#### 2. $cos^{-1}(x)$ 的微分

令 $y=cos^{-1}(x)\Rightarrow cos(y)=x$

$\frac{dy}{dx}=\frac{1}{\frac{dx}{dy}}=\frac{1}{-sin(y)}$

$\because sin(y)=-\sqrt{1-cos^2(y)}$

$\therefore \frac{1}{-sin(y)}=\frac{-1}{\sqrt{1-cos^2(y)}}$

最後再將 $x$ 代入 , 可得 $\frac{-1}{\sqrt{1-x^2}}$

得證 $\Rightarrow \frac{d}{dx}\{cos^{-1}(x)\}=\frac{-1}{\sqrt{1-x^2}}$

<br/>

#### 3. $tan^{-1}(x)$ 的微分

令 $y=tan^{-1}(x)\Rightarrow tan(y)=x$

$\frac{dy}{dx}=\frac{1}{\frac{dx}{dy}}=\frac{1}{sec^2(y)}$

$\because sec^2(y)=1+tan^2(y)$

$\therefore \frac{1}{sec^2(y)}=\frac{1}{1+tan^2(y)}$

最後再將 $x$ 代入 , 可得 $\frac{1}{1+x^2}$

得證 $\Rightarrow \frac{d}{dx}\{tan^{-1}(x)\}=\frac{1}{1+x^2}$

<br/>

#### 4. $cot^{-1}(x)$ 的微分

令 $y=cot^{-1}(x)\Rightarrow cot(y)=x$

$\frac{dy}{dx}=\frac{1}{\frac{dx}{dy}}=\frac{1}{-csc^2(y)}$

$\because csc^2(y)=1+cot^2(y)$

$\therefore \frac{1}{-csc^2(y)}=\frac{-1}{1+cot^2(y)}$

最後再將 $x$ 代入 , 可得 $\frac{-1}{1+x^2}$

得證 $\Rightarrow \frac{d}{dx}\{cot^{-1}(x)\}=\frac{-1}{1+x^2}$

<br/>

#### 5. $sec^{-1}(x)$ 的微分

令 $y=sec^{-1}(x)\Rightarrow sec(y)=x$

$\frac{dy}{dx}=\frac{1}{\frac{dx}{dy}}=\frac{1}{sec(y)tan(y)}$

$\because tan(y)=\sqrt{sec^2(y)-1}$

$\therefore \frac{1}{sec(y)tan(y)}=\frac{1}{sec(y)\sqrt{(sec^2(y)-1)}}$

最後再將 $x$ 代入 , 可得 $\frac{1}{x\sqrt{x^2-1}}$

得證 $\Rightarrow \frac{d}{dx}\{sec^{-1}(x)\}=\frac{1}{x\sqrt{x^2-1}}$

<br/>

#### 6. $csc^{-1}(x)$ 的微分

令 $y=csc^{-1}(x)\Rightarrow csc(y)=x$

$\frac{dy}{dx}=\frac{1}{\frac{dx}{dy}}=\frac{1}{-csc(y)cot(y)}$

$\because cot(y)=\sqrt{(csc^2(y)-1)}$

$\therefore \frac{1}{-csc(y)cot(y)}=\frac{-1}{x\sqrt{x^2-1}}$

得證 $\Rightarrow \frac{d}{dx}\{csc^{-1}(x)\}=\frac{-1}{x\sqrt{x^2-1}}$

> 把上面證明學會後 , 就算考試時忘記 , 也可以在現場快速的推導出來哦~
