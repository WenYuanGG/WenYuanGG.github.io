---
layout: post
title:  "[Linux C] 使用 RAW socket 實現 Ping 功能"
date:   2017-05-17
categories: [網路程式設計]
comments: true
---

不管是在 Linux 或 Window 上 , 你一定或多或少使用過 Ping 這個指令吧！ 使用 Ping 指令可以讓你迅速知道目標主機現在是否存活 (當然有其他例外) 。而這篇文章就是要探討 , 該如何使用 RAW socket 來實現簡單的 Ping 功能

<br/>

#### 關於 ICMP

我們在這次的實作中就是要利用 ICMP (網際網路控制訊息協定) 來實現 Ping 功能。這個協定用於 TCP/IP 網路中傳送控制訊息 , 提供可能發生在通訊環境中的各種問題回饋 , 而透過這些回饋資訊 , 就可以針對問題做出診斷 ， 再採取應對的措施

所以意思就是 , 我們只要對目標主機發送 ICMP 封包的 , 再針對其反饋的封包資訊來加以分析 ， 就可以知道目標主機目前是否存活 , 如同下圖所示

![](https://i.imgur.com/DsTZz2L.png)

此外 , 雖然 ICMP 和 IP 都同屬 OSI 第三層協定 , 但是 ICMP 本身不能自行傳送 , 而是需要倚賴 IP 的幫忙才能傳送 , 這也表示需要兩台 PC 同時支援 IP , 才能使用 ICMP 來進行錯誤偵測與診斷

<br/>

#### ICMP 封包結構

ICMP Header 從第 160 位元開始 , 結構如下圖所示

![](https://i.imgur.com/f91xzsk.png)

其各欄位功能大致如下：

- <b>Type :</b> ICMP 種類 , 可用來識別其狀況
- <b>Code :</b> 進一步劃分 ICMP Type , 可用來識別其錯誤原因
- <b>Checksum :</b> 用來檢查封包資訊有無錯誤
- <b>ID ：</b> 由發送者所定 , 而目標主機的 Echo Reply 必須與我們所設的 ID 相同 , 所以可作為識別之用
- <b>Sequence :</b> 用來紀錄序號 , 而 Echo Reply 同樣必須和發送端相同 , 也是作為識別之用的

> ID Sequence 合起來就可以用來識別特定配對的 Echo Request 和 Echo Reply

而 Type 和 Code 所對應的網路狀況我在這裡就不贅述了 , 想要明白各種狀況可以到[網際網路控制訊息協定](https://zh.wikipedia.org/wiki/%E4%BA%92%E8%81%94%E7%BD%91%E6%8E%A7%E5%88%B6%E6%B6%88%E6%81%AF%E5%8D%8F%E8%AE%AE)察看哦~

<br/>

#### RAW socket 實現 Ping 功能

{% highlight cpp linenos %}
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <netinet/ip.h>
#include <netinet/ip_icmp.h>
#include <arpa/inet.h>

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

// 做 checksum 運算 , 可驗證資料再傳輸時有無毀損
unsigned short checksum(unsigned short *buf, int bufsz){
	unsigned long sum = 0xffff;

	while(bufsz > 1){
		sum += *buf;
		buf++;
		bufsz -= 2;
	}

	if(bufsz == 1)
		sum += *(unsigned char*)buf;

	sum = (sum & 0xffff) + (sum >> 16);
	sum = (sum & 0xffff) + (sum >> 16);

	return ~sum;
}

int main(int argc, char *argv[]){
	int sd;
	struct icmphdr hdr;
	struct sockaddr_in addr;
	int num;
	char buf[1024];
	struct icmphdr *icmphdrptr;
	struct iphdr *iphdrptr;

	if(argc != 2){
		printf("usage: %s IPADDR\n", argv[0]);
		exit(-1);
	}

	addr.sin_family = PF_INET; // IPv4
    
	// 將輸入的位址轉成 network order 形式
	num = inet_pton(PF_INET, argv[1], &addr.sin_addr);
	if(num < 0){
		perror("inet_pton");
		exit(-1);
	}

	// Create a IPv4 RAW socket, and ready to handle ICMP packets
	sd = socket(PF_INET, SOCK_RAW, IPPROTO_ICMP);
	if(sd < 0){
		perror("socket");
		exit(-1);
	}

	// Empty the contents of hdr
	memset(&hdr, 0, sizeof(hdr));

	// Initialize the ICMP packet
	hdr.type = ICMP_ECHO;
	hdr.code = 0;
	hdr.checksum = 0;
	hdr.un.echo.id = 0;
	hdr.un.echo.sequence = 0;

	// 計算出 ICMP Header 的 checksum
	hdr.checksum = checksum((unsigned short*)&hdr, sizeof(hdr));

	// 送出 ICMP Header 封包
	num = sendto(sd, (char*)&hdr, sizeof(hdr), 0, (struct sockaddr*)&addr, sizeof(addr));
	if(num < 1){
		perror("sendto");
		exit(-1);
	}
	printf(KYEL"We have sended an ICMP packet to %s\n", argv[1]);

	memset(buf, 0, sizeof(buf));

	printf(KGRN"Waiting for ICMP echo ...\n");

	// 接收來自對方主機的 Echo Reply
	num = recv(sd, buf, sizeof(buf), 0);
	if(num < 1){
		perror("recv");
		exit(-1);
	}

	// Take out IP header
	iphdrptr = (struct iphdr*)buf;

	// Take out ICMP header
	icmphdrptr = (struct icmphdr*)(buf+(iphdrptr->ihl)*4);

	// Handle the ICMP Type
	switch(icmphdrptr->type){
		case 3:
			printf(KBLU"The host %s is a unrechable purpose!\n", argv[1]);
			printf(KBLU"The ICMP type is %d\n", icmphdrptr->type);
			printf(KBLU"The ICMP code is %d\n", icmphdrptr->code);
			break;
		case 8:
			printf(KRED"The host %s is alive!\n", argv[1]);
			printf(KRED"The ICMP type is %d\n", icmphdrptr->type);
			printf(KRED"The ICMP code is %d\n", icmphdrptr->code);
			break;
		case 0:
			printf(KRED"The host %s is alive!\n", argv[1]);
			printf(KRED"The ICMP type is %d\n", icmphdrptr->type);
			printf(KRED"The ICMP code is %d\n", icmphdrptr->code);
			break;
		default:
			printf(KMAG"Another situations!\n");
			printf(KMAG"The ICMP type is %d\n", icmphdrptr->type);
			printf(KMAG"The ICMP code is %d\n", icmphdrptr->code);
			break;
	}

	close(sd);
	return EXIT_SUCCESS;
}
{% endhighlight %}

- <b>Ping 中華電信 IP 試試看</b>

![](https://i.imgur.com/zTygDE8.png)

- <b>Ping 不存在的 IP 試試看</b>

![](https://i.imgur.com/bzQH62M.png)


<br/>

#### 參考文獻

- [網際網路控制訊息協定](https://zh.wikipedia.org/wiki/%E4%BA%92%E8%81%94%E7%BD%91%E6%8E%A7%E5%88%B6%E6%B6%88%E6%81%AF%E5%8D%8F%E8%AE%AE)
- [ICMP 協定](http://www.pcnet.idv.tw/pcnet/network/network_ip_icmp.htm)