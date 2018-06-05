---
layout: post
title:  "增加一個 System Call 到 Linux Kernel (v4.x)"
date:   2017-05-04
categories: [Linux]
---

#### 我的系統環境
{: style="color:MediumSeaGreen;"}

- **作業系統：** Lubuntu 16.04
- **Kernel 版本：** 4.8.0-49-generic

<br/>

#### 更新系統
{: style="color:MediumSeaGreen;"}

- 更新

{% highlight bash %}
sudo apt-get update
sudo apt-get upgrade
{% endhighlight %}

- 查看你當前的 **Kernel 版本**

{% highlight bash %}
uname -r
{% endhighlight %}

![picture01](https://2.bp.blogspot.com/-0k5fdklzdWQ/WQs1A8MV9cI/AAAAAAAAAKM/jGcdqvzht78HSXmFJFVNPMqjbZwHouTyQCLcB/s1600/1.JPG)

<br/>

#### 下載 Kernel Source
{: style="color:MediumSeaGreen;"}

到 [Kernel Archive](https://www.kernel.org/) 下載 kernel source
我下載的是當時較為穩定的 " 4.10.14 " kernel
你可以選擇你自己要的 kernel , 不一定要跟我一樣

![image](https://3.bp.blogspot.com/-TUBrR9gACv8/WQs2XHirWdI/AAAAAAAAAKY/6pKH6joaK50n7fg62POYuE44oS3RqzIIwCLcB/s1600/2.JPG)

<br/>

下載完後請進行**解壓縮**

{% highlight bash %}
# 進入 root 模式
sudo su

# 把檔案解壓縮到 /usr/src 目錄底下
tar -xvf linux-4.10.5.tar.xz -C /usr/src
{% endhighlight %}

<br/>

建立資料夾

{% highlight bash %}
# 把目錄轉到剛剛下載下來的 kernel 檔案夾
cd /usr/src/linux-4.10.14

# 在裡面創建一個名叫 hello 的資料夾
mkdir hello

# 把目錄轉到 hello 資料夾
cd hello
{% endhighlight %}

<br/>

建立一個 `hello.c`

{% highlight bash %}
vim hello.c
{% endhighlight %}

`hello.c 的內容`

{% highlight cpp %}
/* hello.c */
#inlcude <linux/kernel.h>
asmlinkage long sys_hello(void){
    printk("Hello world!\n");
    return 0;
}
{% endhighlight %}

<br/>

於同一個下再建立一個 `Makefile`

{% highlight bash %}
vim Makefile
{% endhighlight %}

`Makefile` 的內容

{% highlight bash %}
obj-y := hello.o
{% endhighlight %}

<br/>

接著在 linux-4.10.14 目錄下改 Makefile
告訴你的 compiler ，新的 system call 可以在 hello 目錄中找到

{% highlight bash %}
# 把目錄轉回去
cd /usr/src/linux-4.10.14

# 打開此目錄下的 Makefile
vim Makefile
{% endhighlight %}

你可以在裡面找到一行, 如下**我括起來的地方**

![image alt](https://3.bp.blogspot.com/-4r1MUjs4py8/WQs976nVflI/AAAAAAAAALA/t0-N0ymHUiU-pHWQubTA0y0UjjhdwfbcQCLcB/s1600/3.JPG)

請你在**後面補上 hello**
這樣 kernel 在編譯時才會到你的 hello 目錄
改成下面這樣

{% highlight bash %}
core-y += kernel/ certs/ mm/ fs/ ipc/ security/ crypto/ block/ hello/
{% endhighlight %}

<br/>

接著因為我的系統是 **64 位元**的 ((我想大部分的人都是))
所以我要**去修改 syscall_64.tbl 檔**

{% highlight bash %}
cd arch/x86/entry/syscalls
vim syscall_64.tbl
{% endhighlight %}

請你在最後一行添加上你的 system call
然後請把編號記住, 等一下會用

> 註：你會發現下面突然跳到500多號，那是放x32的東西，我們要擺在300多號那邊

![image alt](https://4.bp.blogspot.com/-n-VtlCq7hdQ/WQtASqkGWrI/AAAAAAAAALY/mjNroCyqaeAKNXvu39fJViYxiWuxCCPMgCLcB/s1600/4.JPG)

<br/>

接著編輯 syscalls.h 檔
將我們 syscall 的原型添加進檔案的最後一行 (#endif之前)

{% highlight bash %}
# 把目錄轉回去
cd /usr/src/linux-4.10.14

cd include/linux
vim syscalls.h
{% endhighlight %}

![image](https://2.bp.blogspot.com/-1RLBFOHlSdg/WQtCSGXBBgI/AAAAAAAAALo/GZ6fhFQlQLUBzWbZq28QndS7lk8TlYPcQCLcB/s1600/5.JPG)

<br/>

#### 編譯 kernel
{: style="color:MediumSeaGreen;"}

先把會用到的套件安裝好

{% highlight bash %}
# 這個套件可以幫我們 build 出 kernel-pakage
sudo apt-get install fakeroot build-essential kernel-package libncurses5 libncurses5-dev
{% endhighlight %}

![image alt](https://1.bp.blogspot.com/-Ba1CeFDl218/WQtG0alzuvI/AAAAAAAAAMM/Fl7zWf7UCXsxGK9tCx26x34RXV9wxz2NQCLcB/s1600/6.JPG)

> 選擇第二個，**保持本地版本**就好

<br/>

把 kernel 的 config **複製**出來

{% highlight bash %}
# 把目錄轉回去
cd /usr/src/linux-4.10.14

cp /boot/config-`uname -r` ./.config
{% endhighlight %}

然後用 `make menuconfig` 來設定組態

{% highlight bash %}
make menuconfig
{% endhighlight %}

會跑出以下畫面讓你**設定 configuration**

![image alt](https://2.bp.blogspot.com/-xHRva45jLmY/WQtg0SyLZnI/AAAAAAAAAM4/gQ0CKYKYCDgMYnQervk1JXuP51s8knvWACLcB/s1600/7.JPG)

但是我的目的只是要加一個 system call
所以我直接 **"按兩下 ESC 離開"**
如果你有要做其他設定再自行添加

<br/>

然後就可以 compile 你的 kernel 了

{% highlight bash %}
# 清理殘留的 pakage
make-kpkg clean

# 編譯 kernel
fakeroot make-kpkg -j 4 --initrd kernel_image kernel_headers
# 等編譯完後會產生 kernel 的 image 和 headers
{% endhighlight %}

<br/>

> 如果你在編譯的時候出現 `"fatal error: openssl/opensslv.h: No such file or directory"` 錯誤訊息的話，表示你的 **SSL 庫有缺失**，請你先安裝下面套件後再重新編譯

{% highlight bash %}
sudo apt-get install python-pip python-dev libffi-dev libssl-dev libxml2-dev libxslt1-dev libjpeg8-dev zlib1g-dev

# 再重新下指令編譯
fakeroot make-kpkg -j 4 --initrd kernel_image kernel_headers
{% endhighlight %}

#### 編譯完成
{: style="color:MediumSeaGreen;"}

編譯完成後會在 `/usr/src` 路徑下產生了這個 kernel 的 **image 和 headers** ，我們要用指令安裝他們

{% highlight bash %}
# 轉目錄
cd /usr/src

# 安裝 kernel image
sudo dpkg -i linux-image-4.10.14_4.10.14-10.00.Custom_amd64.deb

# 安裝 kernel headers
sudo dpkg -i linux-headers-4.10.14_4.10.14-10.00.Custom_amd64.deb

# 安裝完後就重開機
reboot
{% endhighlight %}

<br/>

重開機後再看看你的 kernel 版本有沒有變新

{% highlight bash %}
# 查看 kernel version
uname -r
{% endhighlight %}

![image alt](https://1.bp.blogspot.com/-s7kDQlzSFxQ/WQt1e60FoGI/AAAAAAAAANU/jnLPgz2DcacW4HUTfUAr8yJl-K01BDv5gCLcB/s1600/9.JPG)

<br/>

#### 測試 system call
{: style="color:MediumSeaGreen;"}

{% highlight bash %}
# 建一個 hello.c
vim hello.c
{% endhighlight %}

`hello.c` 的內容如下

{% highlight cpp %}
/* hello.c */
#include <linux/kernel.h>
#include <unistd.h>
#include <sys/syscall.h>
#include <stdio.h>

int main(){
    /* 使用我們剛剛新增的 system call */
    long int sys = syscall(332);
    
    /* print 出 syscall 的回傳值, 若為 0 則代表成功 */
    printf("sys_hello returned %ld\\n", sys);
    
    return 0;
}
{% endhighlight %}

{% highlight bash %}
# 編譯 hello.c
gcc -g -Wall hello.c -o hello

# 執行
./hello
{% endhighlight %}

![image alt](https://4.bp.blogspot.com/-TjO-uSiLoVo/WQt22_g8seI/AAAAAAAAANg/OVEZLlaFIHQSubkd_K8tXjh6eLr1aXxTQCLcB/s1600/10.JPG)

> 看到**回傳 0** 則代表成功囉

<br/>

使用 `dmesg` 來查看 kernel 內的訊息

{% highlight bash %}
dmesg
{% endhighlight %}

![image alt](https://4.bp.blogspot.com/-aXj7BIZrkf4/WQt3V0sPMTI/AAAAAAAAANk/0ji34DBmIQYI6GvlUQfYRoXFx_YSx8OxgCLcB/s1600/11.JPG)

> 你會看到 kernel 裡面成功 printk 出了 Hello World! 字串

<br/>

---
