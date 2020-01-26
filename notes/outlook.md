#1.调色板颜色列表
	自己定义的，也可以不是这样。
	#define COL8_000000     0   //黑
	#define COL8_FF0000     1   //亮红
	#define COL8_00FF00     2   //亮绿
	#define COL8_FFFF00     3   //亮黄
	#define COL8_0000FF     4   //亮蓝
	#define COL8_FF00FF     5   //亮紫
	#define COL8_00FFFF     6   //浅亮蓝
	#define COL8_FFFFFF     7   //白
	#define COL8_C6C6C6     8   //亮灰
	#define COL8_840000     9   //暗红
	#define COL8_008400     10  //暗绿
	#define COL8_848400     11  //暗黄
	#define COL8_000084     12  //暗青
	#define COL8_840084     13  //暗紫
	#define COL8_008484     14  //浅暗蓝
	#define COL8_848484     15  //暗灰
	
#2.调色板的访问步骤
* 首先在一连串的访问中屏蔽中断。
* 将想要设定的调色板号码写入0x03c8，紧接着按照R,G,B的顺序写入0x03c9。如果还想设定下一个调色板，则省略调色板号码，再按照R,G,B的顺序写入0x03c9就可以了。
* 如果想要读出当前调色板的状态，首先要将调色板的号码写入0x03c7，再从0x03c9读取三次。读出的顺序就是R，G，B。如果想要读出下一个调色板，同样也是省略调色板号码的设定，按RGB的顺序读出。
* 如果是最初执行了CLI，那么最后要执行STI。
