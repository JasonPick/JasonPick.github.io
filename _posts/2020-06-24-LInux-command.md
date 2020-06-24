---

---

这里整理一下linux中的常用命令，参考自[github](https://github.com/LiPingjiang/bigdata/edit/master/linux/linux.md)


# Linux 常用命令


* 查看进程有关的命令


```bash
查看java进程
ps aux|grep java 
查看所有进程
ps aux 
查看所有有关tomcat的进程
ps –ef|grep tomcat 
高亮要查询的关键字
ps -ef|grep --color java 
高亮要查询的关键字
kill -9 19979 
```

-## #查看资源相关情况

- ‘top’//查看系统资源占用状况
    
- ‘uptime’//系统运行时间

- ‘load average’//负载情况


- CPU 资源状况
    
    *  `vmstat -n 2 3`//其中2代表采样的时间间隔，3代表采样的次数
    
       - `us+sy>80`说明cpu不足
       
    *  `mpstat -P ALL 2`  查看所有CPU核信息


- 内存状况

    * `free -m`查看总的内存
    
    * `pidstat -p PID -r 采样间隔秒数`
    
    
- 硬盘

   * ‘df -h’
   
   
- 磁盘IO

   * ‘iostat -xdk 2 3’
   
   
- 网络IO

    * ‘ifstat’



-## #查找特定的文件


>  find path [options] params


```shell
find ~ -name "target3.java"  //精确查找
find ~ -name "target*"   // 模糊查找文件
find ~ -iname "targer*"  //忽略大小写

man find  //更多关于 find 的指令说明
```


-## #检索文件内容


>grep [options] pattern file


```shell
grep "hadoop" README*
```

-## #管道操作符 ‘｜’


作用：可将指令连接起来，前一个指令的输出作为后一个指令的输入


示例：

```shell
find ~ | grep "Hadoop"
```


  *  管道只处理正确的输出，不执行错误的输出
  
  
  * 右边命令必须能够接收检索文件输入流，否则传递过程中数据会被抛弃
  
  
  * 常用的右面命令：sed,awk,grep,cut,head,top,less,
  
  > grep -o 只输出符合 RE 的字符串


    ```shell
    grep 'partial\[true\]' *.info,log | grep -o 'engine\[[0-9a-z]\]'
    ```

  >grep -v   //过滤掉指令本身


    ```shell
    ps -ef | grep ’tomcat‘ | grep -v ’tomcat‘
    ```



-## #对文件内容做统计


> awk [options] 'cmd' file


* 一次读取一行文本，按输入分隔符进行切片，切成多个组成部分

* 将切片直接保存在内建的变量中，$1,$2...($0表示行的全部)

* 支持对单个切片的判断，支持循环判断，默认分隔符为空格


```shell
awk '{print $1,$4}' README.txt  //打印第一个和第4个切片的内容
```


```shell
awk '$1=="tcp" && $2 == 1 {print $0}' netstat.txt
```


```shell
awk '($1=="tcp" && $2 == 1) || NR==1 {print $0}' netstat.txt  //带有表头的数据
```


```shell
awk -F "," '{print $2}' test.txt   //-F 以逗号为分隔符
```


-## #批量替换文件内容


> sed [option] 'sed command' filename


* 全名 stream editor ，流编辑器

* 适合用于对文本的编辑


```shell
sed 's/^Str/String/' replace.java     //将Str打头的字符替换为String，不改变文档内容
```

```shell
sed -i 's/^Str/String/' replace.java     //将Str打头的字符替换为String，改变文档内容
```


```shell
sed -i 's/\.$/\;/' replace.java  //将 ’.‘ 替换为’；‘
```


```shell
sed -i 's/Jack/me/' replace.java   //将Jack替换为me  但只能替换一个Jack
```


```shell
sed -i 's/Jack/me/g' replace.java   //将所有Jack替换为me
```


```shell
sed -i '/^ *$/d' replace.java   //删除空行
```


```shell
sed -i 'Integer/d' replace.java //删除Integer所在行
```


```shell
grep 'partial\[true\]' *.info,log | grep -o 'engine\[[0-9a-z]\]' |
awk '{engineer[$1]++}END{for(i in enginearr) print i "\t" enginearr[i]}'
```


-## #查看系统负载有两个常用的命令


```shell
 w
10:57:38 up 14 min,  1 user,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    192.168.147.1    18:44    0.00s  0.10s  0.00s w
uptime
10:57:47 up 14 min,  1 user,  load average: 0.00, 0.00, 0.00
```





