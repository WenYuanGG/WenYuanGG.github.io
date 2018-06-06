---
layout: post
title:  "[Java] Convex Hull 演算法"
date:   2017-05-06
categories: [Algorithm]
comments: true
---

建議可以先到 [這裡](http://www.csie.ntnu.edu.tw/~u91029/ConvexHull.html) 了解 Convex Hull 演算法的基本概念, 我覺得他的觀念講的很清楚

#### 我的做法
{: style="color:MediumSeaGreen;"}

- 我是使用 JAVA AWT 實作圖形化, 而 JAVA AWT 視窗的**座標原點是在最左上角**，我沒有重新修改，但這並不影響我的運算結果

- 在平面上亂數產生 N 個點 (由使用者決定) , 接著從這些點中找出**左界和右界**，再依此左界來找出最左上角的點，我將會把此點當成我的**起始點位 (current)** ，如下圖：

![](https://i.imgur.com/IusDQ64.png)

- 現在我們可以先確定最左上角的點也是凸包的其中一點，所以我要以這點為起始點位 (current) ，用迴圈**由左往右尋找可以與起始點形成斜率較大的點**，找到後除了用 buffer 把它紀錄下來之外，也要把它放進 current ，因為我們要繼續以這點為起始點繼續尋找下個點，**直到抵達右界(right)**

- 抵達右界後 (right) 之後我們要反過來，由**右往左尋找斜率較大的點**，就如同步驟 3 一樣，只是方向變了而已。如下圖：

![](https://i.imgur.com/JnMASM9.png)

- 做完步驟 3 4 之後就可以找出最外圍的點到底有哪些了，因為在過程中找出來的點我們都存到一個 buffer 裡，所以接下來就可以直接把這些座標取出來畫一個凸包

> 在第 3 4 步驟的時候要注意，假設找到兩個斜率一樣大的點，我們要**取長度比較長的**那個，所以記得多寫一個判斷式

<br/>

#### 完整程式碼
{: style="color:MediumSeaGreen;"}

{% highlight java %}
import java.awt.*;
import java.awt.Color;
import java.awt.Graphics;
import java.awt.event.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.util.List;
import java.util.Vector;

class Convex extends Frame implements ActionListener, TextListener, MouseMotionListener{
	/*-------------定義會用到的東西----------------*/
	static Convex frm = new Convex();
	static Button btn = new Button("Send");
	static TextArea txa = new TextArea("請輸入要幾個點");
	//static TextArea txa1 = new TextArea("Convex Hull");
	static Label lab = new Label();
	static Label lab1 = new Label("Convex Hull");
	int amount;  //接收由文字方塊輸入的數字

	//定義一個 Vertex 類別，放置 x y 形成一個座標
	private static class Vertex {
		public int x;
		public int y;

		//建構子，可以直接拿來放 x y
		public Vertex(int x, int y) {
			this.x = x;
			this.y = y;
		}
	}
  	/*----------------------------------------------*/



	/*-------------程式的 Entry Point---------------*/
	public static void main(String args[]){
		btn.addActionListener(frm);  //將frm設為btn的事件傾聽者
		txa.addTextListener(frm);  //將frm設為txa的事件傾聽者
		frm.addMouseMotionListener(frm);

		frm.setLayout(null);  //不使用版面配置
		frm.setTitle("Convex Hull");  //設定視窗名稱
		btn.setBounds(425,500,75,50);  //定義按鈕格式
		txa.setBounds(200,500,225,50);  //定義文字方塊格式
		lab1.setBounds(100,30,300,40); //定義文字方塊格式
		lab1.setBackground(Color.yellow);  //lab1設成黃色背景
		lab.setBounds(30,500,190,50);
		//txa1.setEditable(false);  //把 txa1 設為不可編輯
		frm.setSize(500,550);  //設定視窗大小
		frm.add(btn);  //加入按鈕
		frm.add(txa);  //加入文字方塊
		frm.add(lab1); //加入label
		frm.add(lab);  //加入label
		frm.setVisible(true);  //顯示出視窗

		//案 X 時讓視窗可以自動關閉
		frm.addWindowListener(new WindowAdapter(){
			public void windowClosing(WindowEvent e){
				frm.dispose();
			}
		});
	}
	/*-----------------------------------------------*/



	/*--------------------處理事件-------------------*/
	//處理 button 的事件
	public void actionPerformed(ActionEvent e){
		Graphics g = getGraphics();
		update(g);  //先清空畫面後再呼叫paint函式
	}

	//處理 TextArea 的事件
	public void textValueChanged(TextEvent e){
		//幫接收到的字串轉成整數放到 amount
		amount = Integer.valueOf(txa.getText());
	}

	//處理滑鼠移動事件
	public void mouseMoved(MouseEvent e){
		lab.setText("座標：("+e.getX()+","+e.getY()+")");
	}

	//處理滑鼠拖曳事件
	public void mouseDragged(MouseEvent e){
		lab.setText("座標：("+e.getX()+","+e.getY()+")");
	}
	/*-----------------------------------------------*/



	/*---------------------畫圖----------------------*/
	//在平面上畫出亂數產生的點
	public void paint(Graphics g){
		//陣列的大小由我們自己的輸入來決定
		int x[] = new int[amount];
		int y[] = new int[amount];
		List<Vertex> vertices = new Vector<Vertex>();

		//標出座標點
		for(int i=0; i<x.length; i++){
			//x y由亂數產生
			x[i] = (int)(Math.random()*441)+30;
			y[i] = (int)(Math.random()*405)+75;

			Vertex v = new Vertex(x[i], y[i]);
			vertices.add(v);  //做成一個座標陣列
			g.setColor(Color.red);  //設定點的顏色為紅色
			g.fillOval(x[i], y[i], 5, 5);  //畫出亂數座標點
		}

		//計算執行 Convex Hull 演算法所要花的時間
		long begin = System.currentTimeMillis();
		List<Vertex> polygon = convexhull(vertices);  //執行 Convex Hull 演算法
		long elapsed = System.currentTimeMillis()-begin;
		double seconds = elapsed/1000.0;

		//依序將最外圍的點做連線
		for(int i=1; i<polygon.size(); i++) {
			g.setColor(Color.black);
      		g.drawLine(polygon.get(i-1).x, polygon.get(i-1).y, polygon.get(i).x, polygon.get(i).y);
    	}
    	//最後記得再把最後一個點連回第一點
    	g.setColor(Color.black);
    	g.drawLine(polygon.get(0).x, polygon.get(0).y, polygon.get(polygon.size()-1).x, polygon.get(polygon.size()-1).y);

    	System.out.printf("耗費了 %f 秒\n", seconds);
	}
	/*-----------------------------------------------*/



	/*------執行 Convex Hull，找出凸包最外圍的點-----*/
	public static List<Vertex> convexhull(List<Vertex> vertices){
		List<Vertex> polygon = new Vector<Vertex>();
		int left, right;  //左右範圍
		int dxs,dys;  //起始斜率
    	int dxc,dyc;  //暫存斜率
    	int dxt,dyt;  //測試斜率
    	int lqc,lqt;  //長度平方(暫存/測試)
    	int compareResult;  //斜率比較結果
    	int x;  //存放數據用的
		Vertex current = null;  //目前拜訪的點
		Vertex next = null;  //下一個選擇點

		//先取出座標點的左右範圍
		left = vertices.get(0).x;
		right = vertices.get(0).x;
		for(int i=1; i<vertices.size(); i++){
			//依序取出 x 來做比較，藉此求出左右範圍
			x = vertices.get(i).x;
			if(x<left) left = x;
			if(x>right) right = x;
		}

		//找出最左上角的點，準備當成起點
		for(Vertex v : vertices){
		//依序把 vertices 加入 v
			if(v.x==left && (current==null || v.y>current.y))
				current = v;
				//再找出一個y最大的點，放到current
		}

		//將我們剛剛找到的起點加入polygon,就是最左上角的點
		polygon.add(current);



		//1. 我們首先定義起始斜率為無限大
		//2. 以currnt.x為基準來測試右邊的點
		//3. 在這些斜率中找出斜率最大的
		//4. 若有斜率相同的情況則取長度最長者
		//5. 新找到的斜率就作為下次的起始斜率
		//6. 繞完迴圈之後就可以得到全部座標中的斜率最大者
		dys = 1;  dxs = 0;  //先把起始斜率設成無限大
		while(current.x<=right){
			dyc = -1;  dxc = 0;  lqc = 0;
			for(Vertex v : vertices){
			//依序把 vertices 加入 v
				if(v.x>=current.x){
					dyt = v.y - current.y;
					dxt = v.x - current.x;

					//把起始斜率和測試斜率送到 compareSlope 函式作比較
					if(compareSlope(dyt, dxt, dys, dxs) == -1){
						compareResult = compareSlope(dyt,dxt,dyc,dxc);
						lqt = dyt*dyt+dxt*dxt;  //計算長度平方

						if(compareResult>=0)
							if(compareResult>0 || lqt>lqc){
								next = v;
								dyc = dyt;
								dxc = dxt;
								lqc = lqt;
							}
					}
				}
			}

			if(next == null) break;
			//若找到我們要的點以後，就把起始斜率換成最後一次的暫存斜率
			dys = dyc;
			dxs = dxc;
			polygon.add(next);  //把找出來的點加到polygon
			vertices.remove(next);  //把我們找過的點移除掉
			current = next;  //把next放到current，把它當成目前點位來繼續找下個點
			next = null;  //把next清空
		}


		//1. 我們首先定義起始斜率為無限大
		//2. 以 current.x 為基準來測試左邊的點
		//3. 在這些斜率中找出斜率最大的
		//4. 若有斜率相同的情況則取長度最長者
		//5. 新找到的斜率就作為下次的起始斜率
		//6. 繞完迴圈之後就可以得到全部座標中的斜率最大者
		dys = 1;  dxs = 0;
		while(current.x>left){
			dyc = -1;  dxc = 0;  lqc = 0;
			for(Vertex v : vertices){
				if(v.x<current.x){
					dyt = v.y - current.y;
					dxt = v.x - current.x;

					if(compareSlope(dyt, dxt, dys, dxs) == -1){
						compareResult = compareSlope(dyt, dxt, dyc, dxc);
						lqt = dyt*dyt+dxt*dxt;  //計算長度平方

						if(compareResult>=0)
							if(compareResult>0 || lqt>lqc){
								next = v;
								dyc = dyt;
								dxc = dxt;
								lqc = lqt;
							}
					}
				}
			}

			if(next == null) break;
			dys = dyc;
			dxs = dxc;
			polygon.add(next);
			current = next;
			next = null;
		}

		return polygon;
		//回傳我們找到的最外圍的點
	}
	/*-----------------------------------------------*/



	/*-------------------比較斜率--------------------*/
	public static int compareSlope(int dy2, int dx2, int dy1, int dx1){
		if(dx2!=0 && dx1!=0){
		//如果兩個數的斜率都不是無限的話...
			 double test = dy2*dx1-dy1*dx2;
			 return (int)Math.signum(test);
			 //若 test>0 則返回 1，若 test<0 則回傳 -1，若 test 為 0 則回傳 0
		}

		else{
			//只有其中一數的斜率是無限的時候
			if(dx2!=0 || dx1!=0){
				if(dx2 == 0){
					// dy2/dx2 斜率，無限大或無限小
					return dy2>=0 ? 1 : -1;
				}
				else{
					// dy1/dx1 斜率，無限大或無限小
					return dy1>=0 ? -1 : 1;
				}
			}

			else{
			//此情況代表兩個座標點的斜率都是無窮
				if(dy2>=0){
					// dy2/dx2 斜率，無限大或無限小
					return dy1>=0 ? 0 : 1;
				}
				else{
					// dy2/dx2 斜率，無限大或無限小
					return dy1>=0 ? -1 : 0;
				}
			}
		}
	}
	/*-----------------------------------------------*/
}
{% endhighlight %}

<br/>

#### 執行結果
{: style="color:MediumSeaGreen;"}

- 由使用者輸入要產生幾個亂數點

![](https://i.imgur.com/FCNvDSY.png)

- 我們試試看產生 500 個點

![](https://i.imgur.com/IxC86Fu.png)

- 此外我在程式中還有計算耗費時間

![](https://i.imgur.com/qPbo0fY.png)

