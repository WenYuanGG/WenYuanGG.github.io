---
layout: post
title:  "如何讓程式印出來的字串有顏色"
date:   2018-06-28
categories: [Linux]
comments: true
---

寫程式時 , 如果可以再 terminal 上面印出各種顏色的字串 , 可以讓你在觀看結果時會更加的方便與迅速

#### Escape sequences 轉義序列

這是用於定義顏色的 ANSI 轉義碼 , 所有的 ANSI 轉義序列都是以 ESC 開頭 , 我最常使用的是 ASCII Hex 的撰寫方式 , 其撰寫格式如下：

`/1xB[(文字裝飾);(顏色代碼)m`

> 括號的部份是你可以改變的

- <b>文字裝飾</b>

有以下幾種

| 0 | 1 | 4 | 3 |
| -------- | -------- | -------- | --- |
| 正常 | 粗體 | 底線 | 背景 |

- <b>顏色代碼</b>

| 基本的 8 色 | 基本的高對比色 | xterm 的 256 色 |
| -------- | -------- | -------- |
| 30 ~ 37 | 90 ~ 97 | 0 ~ 255 |

這種 ANSI 轉義序列當然不只可以用在程式語言中而已 , 也可以直接用在 terminal , 我下面會以 C 語言做展示

#### C 語言 Escape sequences 寫法

```cpp
#include <stdio.h>

#ifndef _DEBUG_COLOR_
#define _DEBUG_COLOR_
    #define KDRK "\x1B[0;30m"
    #define KGRY "\x1B[1;30m"
    #define KRED "\x1B[0;31m"
    #define KRED_L "\x1B[1;31m"
    #define KGRN "\x1B[0;32m"
    #define KGRN_L "\x1B[1;32m"
    #define KYEL "\x1B[0;33m"
    #define KYEL_L "\x1B[1;33m"
    #define KBLU "\x1B[0;34m"
    #define KBLU_L "\x1B[1;34m"
    #define KMAG "\x1B[0;35m"
    #define KMAG_L "\x1B[1;35m"
    #define KCYN "\x1B[0;36m"
    #define KCYN_L "\x1B[1;36m"
    #define WHITE "\x1B[0;37m"
    #define WHITE_L "\x1B[1;37m"
    #define RESET "\x1B[0m"
#endif

int main(int argc, char *argv[]){
    printf(KDRK"KDRK\n");
    printf(KGRY"KGRY\n");
    printf(KRED"KRED\n");
    printf(KRED_L"KRED_L\n");
    printf(KGRN"KGRN\n");
    printf(KGRN_L"KGRN_L\n");
    printf(KYEL"KYEL\n");
    printf(KYEL_L"KTEL_L\n");
    printf(KBLU"KBLU\n");
    printf(KBLU_L"KBLU_L\n");
    printf(KMAG"KMAG\n");
    printf(KMAG_L"KMAG_L\n");
    printf(KCYN"KCYN\n");
    printf(KCYN_L"KCYN_L\n");
    printf(WHITE"WHITE\n");
    printf(WHITE_L"WHITE_L\n");
    printf(RESET"RESET\n");

    return 0;
}
```

- <b>執行結果會如下圖</b>

![](https://i.imgur.com/lm89GZN.png)
