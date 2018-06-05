---
layout: post
title:  "寫簡單的 Linux Kernel Module"
date:   2017-05-04
categories: [Linux]
comments: true
---

#### 1. 先創建一個資料夾 並寫個簡單的 kernel module (hello.c) 
{: style="color:MediumSeaGreen;"}

{% highlight bash %}
mkdir helloworld

cd helloworld

vim hello.c
{% endhighlight %}

- `hello.c` 的 code 如下：

{% highlight cpp %}
/* hello.c */
#include <linux/init.h>
#include <linux/module.h>
  
MODULE_DESCRIPTION("Hello_world");
MODULE_LICENSE("GPL");
  
static int hello_init(void)
{
 printk(KERN_INFO "Hello world !\n");
 return 0;
}
  
static void hello_exit(void)
{
 printk(KERN_INFO "ByeBye !\n");
}
  
module_init(hello_init);
/* When u use insmod, it will enter hello_init function */

module_exit(hello_exit);
/* When u use rmmod, it will enter hello_exit function */
{% endhighlight %}

<br/>

#### 2. 接著在同個目錄下寫個 Makefile
{: style="color:MediumSeaGreen;"}

{% highlight bash %}
vim Makefile
{% endhighlight %}

- `Makefile` 的內容如下：

{% highlight bash %}
PWD := $(shell pwd) 
KVERSION := $(shell uname -r)
KERNEL_DIR = /usr/src/linux-headers-$(KVERSION)/
 
MODULE_NAME = hello
obj-m := $(MODULE_NAME).o
 
all: 
 make -C $(KERNEL_DIR) M=$(PWD) modules
clean: 
 make -C $(KERNEL_DIR) M=$(PWD) clean
{% endhighlight %}

<br/>

#### 3. 最後就可以使用 sudo make 指令來編譯
{: style="color:MediumSeaGreen;"}

> 將他編譯成 `.ko` 檔

{% highlight bash %}
sudo make
{% endhighlight %}

![picture01](https://raw.githubusercontent.com/a1996850622/mdsite_document/master/Linux_kernel_module/picture01.JPG)

<br/>

#### 4. 使用 insmod 來載入 module 
{: style="color:MediumSeaGreen;"}

{% highlight bash %}
sudo insmod hello.ko

# 載完後用 dmesg 來觀看 kernel 訊息
dmesg
{% endhighlight %}

![picture02](https://raw.githubusercontent.com/a1996850622/mdsite_document/master/Linux_kernel_module/picture02.JPG)

> 可以看到成功 print 出 hello world ! 字串

<br/>

你也可以使用 `lsmod` 指令來列出目前在使用的 module

{% highlight bash %}
# grep "hello" 用來濾出我們的 hello module 
lsmod | grep "hello"
{% endhighlight %}

![picture03](https://raw.githubusercontent.com/a1996850622/mdsite_document/master/Linux_kernel_module/picture03.JPG)

<br/>

最後想要移除掉模組的話 要使用 `rmmod`

{% highlight bash %}
sudo rmmod hello.ko

# 再觀察一次 kernel 訊息
dmesg
{% endhighlight %}

![picture04](https://raw.githubusercontent.com/a1996850622/mdsite_document/master/Linux_kernel_module/picture04.JPG)

> 可以看到 print 出了 ByeBye ! 字串

<br/>

---
