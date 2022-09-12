-----------
title: python语言相关
date: 2021/06/18 12:23:19

-----------

## python术语（term）
- modules:
1. modules object:
2. modules function:
- object types:

## 使用c/c++扩展python

## python库使用

### 协程库asyncio

所谓协程：指的是能在上下文（函数）中随意切换（调度）的一种方式。线程是切换多个函数，而协程相当于一个线程函数中随意暂停和启动。

协程（coroutine），又称为微线程，纤程。协程的作用：在执行A函数的时候，可以随时中断，去执行B函数，然后中断继续执行A函数（可以自动切换），单着一过程并不是函数调用（没有调用语句），过程很像多线程，然而协程只有一个线程在执行

```python
'''
python3.4引入asyncio异步io库
并且协程的异步操作可以通过该库来完成
'''
import asyncio

@asyncio.coroutine	#装饰器
async def gen(i):
    print('begin-'+i)
    yield from asyncio.sleep(1)	#关键字
    print('end-'+i)
    
loop = asyncio.get_event_loop()
#tasks = [gen('3')]
tasks = [gen(i) for i in range(4)]
loop.run_until_complete(asyncio.wait(tasks))
loop.close()  
```



```python

'''
python3.5之后，为异步io库引入新语法支持
@asyncio.coroutine ---> async def xxx:
yield from other_generator ---> await other_generator
'''
import asyncio

async def gen(i):
    print('begin-'+i)
    await gen2()
    print('end-'+i)

async def gen2():
    print('in gen2')



loop = asyncio.get_event_loop()
tasks = [gen('3')]
loop.run_until_complete(asyncio.wait(tasks))
loop.close()

'''
return:

begin-3
in gen2
end-3
'''

```







### requests 库

- 查看网页编码，网页是什么编码通过网络传输会来的就是什么编码的字符。

  res = response.content.decode('gb2312','ignore')

  这样一来，res就是python这中标准的字符串了（python3的字符串编码统一为’unicode’==ansi【windows下】）。

<img src="D:\document\Home\pics\charset_website.png" style="zoom:80%;" />

- 



### scrapy库

1.scrapy
    1.1 缩小爬取目标，爬取网站的某一个主题（url有规律)
    1.2 编写item.py文件，设置爬取内容
    1.3 通过继承某一种爬虫（spider,CrawlSpider,XMLFededSpider, CSVFeedSpider, SitemapSpider)来实现自己的爬虫。
        其中spider这个是其他爬虫的公共基类构造如下
        class spider():
            *name               -string
            *allowed_domains        -[]
            *start_urls         -[]
            custom_settings
            crawler
            crawler

            def from_crawler(crawler, *args, **kwargs)
            *def start_requests()
            *def make_requests_from_url(url)
            *def parse(response)
            def log(message[, level, component])
            def closed(reason)

Q:  什么叫动态网页爬取?
A:  python有许多库可以让我们很方便地编写网络爬虫，爬取某些页面，获得有价值的信息！但许多时候，爬虫取到的页面仅仅是一个静态的页
    面，即网页的源代码，就像在浏览器上的“查看网页源代码”一样。一些动态的东西如javascript脚本执行后所产生的信息，是抓取不到的，
    这里暂且先给出这么一 些方案，可用于python爬取js执行后输出的信息。
L:  https://www.cnblogs.com/taolusi/p/9282565.html

Q:  WebKit 是一个开源的浏览器引擎,那什么是浏览器引擎？
A:  浏览器的主要构成：
    1.用户界面
    2.浏览器引擎(负责窗口管理、Tab进程管理等)
    3.渲染引擎(有叫内核，负责HTML解析、页面渲染)
    4.JS引擎(JS解释器，如Chrome和Nodejs采用的V8)
L:  https://segmentfault.com/a/1190000019142746
L:  https://blog.csdn.net/alooffox/article/details/88913037


### lxml
1.xpath
    form lxml import xpath



### 文件库

文件操作
http://blog.csdn.net/wirelessqa/article/details/7974531



### socket库

**import socket**

客户端：						ipv4协议        tpc连接
1.创建一个通道（套接字）对象s  s=socket.socket（socket.AF_INET,socket.SOCK_STREAM）
                                                     udp连接用socket.SOCK_DGRAM
2.建立连接（连接通道）  s.connect（（IP地址，端口号））

3.开始通信  （发送数据或接收数据）
发送数据   s.send（内容）
接收数据    s.recv（字节数）

4.关闭通道（通信完成）   s.close（）




----------------------------------------------------------------------
服务器：
1.创建通道		s=socket.socket（socket.AF_INET,socket.SOCK_STREAM）
						udp连接用socket.SOCK_DGRAM

2.绑定端口          s.bind（(本机ip，端口号)）


3.监听端口
	如果为tcp连接
		监听端口   s.listen(监听数量)
  	否则：
		不监听

4.等待连接（通过永久循环）
  while  Ture：
       sock，addr=s.accept（）收到连接，接受对方sock（通道）和addr（IP地址，端口号）
       t=threading.Thread（target=要执行的函数，args=（sock，addr））创建线程来处理
        t.start（）    启动线程

5.写线程进行通信（收发数据）
发送数据    sock.send（）
接受数据    sock.recv()
关闭通道    sock.close()


参考链接：

python  socket编程
http://blog.csdn.net/chchlh/article/details/38001547

### tkinter库

   grid布局            (相对于pack布局)
在Tkinter模板下
使用pack进行布局的话，你不得不使用一些额外的frame控件，而且还需要花费一些功夫让他们变得好看。如果你使用grid的话，你只需要对每个控件使用grid,所有的东西都会以合适的方式显示

from tkinter import *           导入Tkinter模板

master=Tk()                     创建主框架（容器）我们其他容器就放在这个master对象里

Label（xxx，text=‘...’）       在xxx父容器里创建标签内容为‘...’
Entry（xxx）			创建一个入口
Button（xxx，text=‘...’command=？？）创建一个按钮 command是可选择的，实现某种功能
Cheakbutton（xxx，text=‘...’，variable=IntVar（））  创建勾选框
Canvas 画布控件；显示图形元素如线条或文本 
Checkbutton 多选框控件；用于在程序中提供多项选择框 
Frame 框架控件；在屏幕上显示一个矩形区域，多用来作为容器 
Listbox 列表框控件；在Listbox窗口小部件是用来显示一个字符串列表给用户 
Menubutton 菜单按钮控件，由于显示菜单项。 
Menu 菜单控件；显示菜单栏,下拉菜单和弹出菜单 
Message 消息控件；用来显示多行文本，与label比较类似 
Radiobutton 单选按钮控件；显示一个单选的按钮状态 
Scale 范围控件；显示一个数值刻度，为输出限定范围的数字区间 
Scrollbar 滚动条控件，当内容超过可视化区域时使用，如列表框。. 
Text 文本控件；用于显示多行文本 
Toplevel 容器控件；用来提供一个单独的对话框，和Frame比较类似 
Spinbox 输入控件；与Entry类似，但是可以指定输入范围值 
PanedWindow PanedWindow是一个窗口布局管理的插件，可以包含一个或者多个子控件。 
LabelFrame labelframe 是一个简单的容器控件。常用与复杂的窗口布局。 

tkMessageBox 用于显示你应用程序的消息框

上面的对象创建出来拉都要用grid进行格式设定才生效即如：
Label(master, text="First").grid(row=0, column=1)  row代表行，column代表列


常见格式设定如下：
row=0    在第0行
column=0   在第0列
rowspan=1   在row+1  行
columnspan=1    在column+1   列
sticky=W        往西（左）对齐    N/S/W/E  四个选项
以下不常用
padx=5         在x方向上相隔5个单位（很小）与rowspan不一样
pady=4          在y方向上相隔4个单位


如果想插入图片则如下：
from PIL import ImageTk,Image

photo=Image.open（‘addr’）  先打开图片文件（addr为文件位置）
im=ImageTk.PhotoImage（photo）  根据拿到的文件创建图片对象im
label=Label(master,image=bm)    将对象im和标签容器组合（相当于将图片贴在容器上）

label.grid（......）           将标签容器放在大容器master的合适位置

list 转为 str						

list3=['www', 'google', 'com']

str4 = "".join(list3)  输出为：wwwgooglecom  
print str4  

str5 = ".".join(list3)  输出为：www.google.com 
print str5  

str6 = " ".join(list3)  输出为：www google com 
print str6  

----------------------------------------------------
str转为list

str=‘xxxx’
list1=list（str）     输出为 【‘x’，‘x’，‘x’，‘x’】
print list1

------------------------------------------------------------
实例化一个Menu对象，这个在主窗体添加一个菜单  
  menu = Menu(self.master)  
  self.master.config(menu=menu)  

创建File菜单，下面有Save和Exit两个子菜单 

    file = Menu(menu)  
    file.add_command(label='Save')  
    file.add_command(label='Exit', command=self.client_exit)  
    menu.add_cascade(label='File',menu=file)  

创建Edit菜单，下面有一个Undo菜单  

    edit = Menu(menu)  
    edit.add_command(label='Undo')  
    menu.add_cascade(label='Edit',menu=edit)



参考链接：
listbox3.5版本
http://blog.csdn.net/aa1049372051/article/details/51878578

stringvar用法
http://blog.csdn.net/wuxiushu/article/details/52515926

布局方式
https://www.cnblogs.com/mathpro/p/8066530.html

国外文献
http://effbot.org/tkinterbook/

控件学习
http://blog.csdn.net/pfm685757/article/details/50162567

事件绑定
http://blog.csdn.net/u014027051/article/details/53813152?locationNum=10&fps=1

事件驱动
http://blog.csdn.net/u014027051/article/details/53813152?locationNum=10&fps=1





### re 库

- 正则表达式实质就是一串字符串，不过只是有普通字符和特殊字符之分而已。

  普通字符：

  ​	打印字符：

  ​	非打印字符：

  <img src=".\pics\非打印.png" style="zoom:80%;" />

  | \d   | 匹配数字                                 |
| ---- | ---------------------------------------- |
  | \w   | 匹配字符、数字、下划线等价于[a-zA-Z0-9_] |

  

  特殊字符（具有一定功能的字符）：特殊字符根据功能划分又分为两类【匹配符、数量符】，其实就是正则表达式的全部

  ​	匹配符：代表匹配方式，匹配哪些内容
  
  <img src=".\pics\匹配符.png" style="zoom: 80%;" />
  
  ​	数量符：代表按照匹配符匹配的次数。
  
  ​			<img src=".\pics\数量符.png" style="zoom:80%;" />
  
  小括号，中括号，大括号的含义与区别：
  
  小括号，用于**分组的目的**。小括号内的正则模式为一个组。(\s*)：代表匹配任意多个空白字符
  
  中括号，用于限定字符**匹配的范围**。是该范围内的一个字符[\s*]: 代表匹配空白字符或者\*号
  
  大括号，用于限定正则模式**匹配的次数**



## python使用过程碰到的一些问题

### pip安装时连接超时

Q:		pip 安装时连接超时	read timed out
A:		-手动指定源 -i来指定
		pip install numpy -i https://pypi.doubanio.com/simple/
链接：https://www.jianshu.com/p/8e042b7e91b6





### python解释器：

cpython（c语言写的）
ipython（基于cpython，ui不同）
pypy（采用jit技术，实现脚本语言的编译，提高运行速度）
jython（java语言实现的，实现与java的共通相互可以调用）
ironpython（.net平台中的语言实现的，运行于.net平台）

### bytes 数据序列化和反序列化
```python3




```







## python语法

### try,except,finally,else,rasie相关语法

> Refernce:
> [异常处理](https://justcode.ikeepstudying.com/2019/01/python-try-catch-python-%e5%bc%82%e5%b8%b8%e5%a4%84%e7%90%86-python-%e8%8e%b7%e5%8f%96%e5%bc%82%e5%b8%b8%e5%90%8d%e7%a7%b0-try%e4%b8%8eexcept%e5%a4%84%e7%90%86%e5%bc%82%e5%b8%b8%e8%af%ad%e5%8f%a5/]
>
> [异常处理简单介绍](http://c.biancheng.net/view/2315.html)
>

#### except
sementic:
    捕捉try中执行的异常，可以是自定义的异常（继承自Exception）

#### finally
sementic:
    一般是和try连用，无论try段落成功执行与否，都会执行finally段落。

#### else
sementic:
    一般用于try执行成功之后执行的段落，其实没什么用，不如放在try段落的最后执行。

