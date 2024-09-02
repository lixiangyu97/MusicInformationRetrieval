# 安装VS步骤

‍

‍

​![image](assets/image-20240723144137-elfjphn.png)​

​![image](assets/image-20240723144159-xbdjnde.png)​

​![image](assets/image-20240723144101-nyqhahm.png)​

​![image](assets/image-20240723144226-40rid9v.png)​

​![image](assets/image-20240723144233-lx8vic0.png)​

‍

‍

​![image](assets/image-20240723144416-kfsslgi.png)​

‍

如果发现太慢

则使用以下办法

​![image](assets/image-20240723144654-urf9hjg.png)​

​![image](assets/image-20240723144704-fa9dfjc.png)​

‍

‍

[Dns检测|Dns查询 - 站长工具 (chinaz.com)](https://tool.chinaz.com/dns/)

‍

‍

输入download.visualstudio.microsoft.com

‍

‍

​![image](assets/image-20240723150912-5sbmrjw.png)​

‍

找到TTL数值较小的一个(不是越小越好)，复制，然后组成这一行加到host文件中

110.242.20.64 download.visualstudio.microsoft.com(前面的IP写自己搜到的那一个)

‍

‍

‍

​![image](assets/image-20240723150815-geny0fx.png)​

‍

‍

cd C:\Windows\System32\drivers\etc

​![image](assets/image-20240723165044-yj4bloh.png)​

‍

‍

输入notepad hosts并回车打开hosts，在末尾加上刚才那一段话

​![image](assets/image-20240723165144-3t936ep.png)​

如果不能直接保存 另存为后去掉后缀名然后替换原文件

‍

‍

**刷新DNS（这一步一般不需要）** ：windows徽标+R，打开cmd, 执行`ipconfig /flushdns`​

‍

‍

‍

​![image](assets/image-20240723165309-yhym84s.png)​

‍

发现速度快了很多！！

‍

​![image](assets/image-20240723165402-7bahir5.png)​

‍

‍

下载完成后自动弹出如下界面

‍

​![image](assets/image-20240723165514-zrkktvs.png)​

‍

点击更改

将C盘换成D盘

当然要是你的C盘足够大 那就不用换

​![image](assets/image-20240723165607-sqoxs7l.png)​

‍

选择使用 C++ 的桌面开发和Linux 的 C++ 开发

（先选这两个 不够再装）

‍

​![image](assets/image-20240723170704-3e8aigw.png)安装成功之后即可使用。
