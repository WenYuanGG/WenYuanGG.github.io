---
layout: post
title:  "在 Linux 上寫 C 語言該注意的事"
date:   2018-06-05
categories: [Linux]
comments: true
---

> 我將告訴你 - 在 Linux 上寫 C 語言該注意的事

#### 1. if...else... 排序
{: style="color:MediumSeaGreen;"}

有學過計算機結構學就會了解, CPU 會將你的指令一行一行照順序地 Fetch 進 Instruction Memory , 因此可以想像, 如果我們 <b>將發生機率高的程式區塊放在 if 中的話, 便會讓程式整體的效率提升</b>, 如下所示：

{% highlight cpp %}
if(Condition){
    // 放發生機率高的事件  
} else{
    // 放發生機率低的事件
}
{% endhighlight %}

<br/>

#### 2. static 的運用
{: style="color:MediumSeaGreen;"}

重要的 <b>函式或變數要使用 static 來宣告</b>, 可以避免多個程式連結執行時, 因為在其他檔案中已宣告過相同的函式或變數而發生錯誤, 宣告方式如下：

{% highlight cpp %}
static int errno; // 宣告變數
// 宣告函式
static void function(void){
    
}
{% endhighlight %}

> 使用 static 宣告的函式或變數代表只在該程式中有效

<br/>

#### 3. 善用 #define
{: style="color:MediumSeaGreen;"}

在程式中儘量不要出現數字, <b>固定的數字儘量使用 #define 來定義</b>, 其一是為了方便管理程式, 其二是怕你不小心敲到鍵盤多打到其它數字， 這時你就 GG 了, 因為你很難在幾千甚至上萬行的程式中找出錯誤點, 宣告方式如下：

{% highlight cpp %}
#define PI 3.14159
#define BUFSIZE 1024
{% endhighlight %}

> 聽說在某些外商公司會強烈要求程式中**不可以出現數字**

<br/>

#### 4. 善用 man page
{: style="color:MediumSeaGreen;"}

使用函數時要先看 man page 中是如何定義該函數的型態, 例如下方：

{% highlight cpp %}
ssize_t write(int fd, const void *buf, size_t count);
{% endhighlight %}

你可以看到 `write()` 的回傳參數型態是 `ssize_t` , 一般情況下可能不會方生錯誤, 但是當你換到不同的 CPU 架構或者其他執行平台時就可能會發生錯誤

> 假設你想看 `write()` 的函數定義，你可以再瀏覽器搜尋 `man write`

<br/>

#### 5. 有借有還，再借不難
{: style="color:MediumSeaGreen;"}

不管你是分配了動態記憶體或者開了新的 file descriptor , 只要你 <b>不用時都要記得關掉 (釋放掉)</b>, 否則你的 <b>程式可能會再執行一段時間後因為資源不足而掛掉</b>, 如下所示：

{% highlight cpp %}
int sockfd = socket(res->ai_family, res->ai_socktype, res->ai_protocol);
...
close(sockfd);
{% endhighlight %}

{% highlight cpp %}
int *p = malloc(sizeof(int));
...
free(p);
{% endhighlight %}

<br/>

#### 6. 更為嚴謹的編譯方式
{: style="color:MediumSeaGreen;"}

建議在使用 gcc 編譯時可以加入 -g -Wall 參數, 這兩個參數會 <b>幫你編入錯誤訊息 (GDB可用) 以及顯示出所有警告訊息</b>, 如下所示

{% highlight bash %}
/home/chris $ gcc -g -Wall exmaple.c -o output 
{% endhighlight %}

> 試著去除所有的 Warning Message , 讓你的程式更加完美

<br/>

#### Reference
{: style="color:MediumSeaGreen;"}

- 學校上課內容

<br/>

> 如果有其他要補充的, 歡迎下方留言告訴我

