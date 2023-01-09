# Linux

1. 命令行 = 终端 

   命令 = linux程序

2. 命令通用格式

   command [-options] [parameter]

   (必填).         （选填）.    (选填)

   ls -l /home/hello 以列表的形式，显示这个目录下的内容

   cp -r test1 test2 复制test1文件夹成为test2

3. ls

   ls [-a -l -h] [path]

   不带选项时，以平铺形式，列出目录内容

   -a 所有文件 包括隐藏的文件/文件夹 前面带有.的文件为隐藏文件

   -l 已列表形式展示 展示更多信息  ｜权限 ｜ 用户和用户组 |  大小| 创建时间

   -h 与-l组合起来用 -lh，显示大小的时候显示单位 默认为b 

   -t 将文件依建立时间之先后次序列出

   -F 在列出的文件名称后加一符号；例如可执行档则加 "*", 目录则加 "/"

   -R 若目录下有文件，则以下之文件亦皆依序列出

   ​	[root@localhost ~]# ls -ahl

   ​	total 28K

   ​	dr-xr-x---. 2 root root 135 Dec 16 02:34 .

   ​	dr-xr-xr-x. 17 root root 224 Dec 14 01:38 ..

   ​	-rw-------. 1 root root 1.3K Dec 14 01:38 anaconda-ks.cfg

   ​	-rw-------. 1 root root 1.2K Dec 16 02:34 .bash_history

   ​	-rw-r--r--. 1 root root  18 Dec 28 2013 .bash_logout

   ​	-rw-r--r--. 1 root root 176 Dec 28 2013 .bash_profile

   ​	-rw-r--r--. 1 root root 176 Dec 28 2013 .bashrc

   ​	-rw-r--r--. 1 root root 100 Dec 28 2013 .cshrc

   ​	-rw-r--r--. 1 root root 129 Dec 28 2013 .tcshrc

   ​	从上面可以看到，每一行都有7列，分别是：

   1. **第一列**共10位，第1位表示文档类型，`d`表示目录，`-`表示文件，`l`表示链接文件，`d`表示可随机存取的设备，如U盘等，`c`表示一次性读取设备，如鼠标、键盘等。后9位，依次对应三种身份所拥有的权限，身份顺序为：owner、group、others，权限顺序为：readable、writable、excutable。如：`-r-xr-x---`的含义为**当前文档是一个文件，拥有者可读、可执行，同一个群组下的用户，可读、可执行，其他人没有任何权限**。
   2. **第二列**表示链接数，表示有多少个文件链接到inode号码。
   3. **第三列**表示拥有者
   4. **第四列**表示所属群组
   5. **第五列**表示文档容量大小，单位字节
   6. **第六列**表示文档最后修改时间，注意不是文档的创建时间哦
   7. **第七列**表示文档名称。以点(.)开头的是隐藏文档

4. cd

   cd [path] 切换目录到目录 不写目录就回到home 

   . 当前目录 ..上一级目录 ~home目录

5. pwd 打印当前工作目录

6. mkdir

   mkdir [-p] path

   参数必填 表示路径 相对路径或者绝对路径均可

   -p选填，标识自动创建不存在的父目录，用于创建连续多层次的目录

7. touch

   touch path 创造一个文件

8. cat

   cat path 查看文件内容，不能翻页

9. more

   more path 查看文件内容，可以翻页

   查看的时候通过空格翻页，通过q退出查看，b上一页，回车下一行

10. cp

    cp [-r] param1 param2

    -r可选 复制文件夹时必须使用，表示递归复制，整个目录复制走

    参数一，路径，表示被复制文件或文件夹路径

    参数二，路径，表示要复制到的地方

11. mv

    mv param1 param2 移动文件或文件夹

12. rm

    rm [-r -f] param1 ..... paramN

    -r用于删除文件夹

    -f表示force 强制删除（不会弹出提示信息）

    普通用户删除内容不会给予提示，只有root管理员删除内容会有提示

    一般用户用不到-f

    param为路径，用空格隔开

    rm支持通配符* 用于做模糊匹配

    test* 表示匹配所有以test开头的内容

    *test 表示所有以test结尾的内容

    *test *表示所有包含test的内容

13. su - root 切换root用户

    exit退出root用户

14. grep

    grep [-n] 关键字 path    过滤得到含有关键字的行

    -n 表示在结果中显示匹配的行的行号

    关键字：表示过滤的关键字 建议用“ ”包起来

    path可以不填 作为管道符的输入端口

15. wc

    wc [-c -m -l -w] 文件路径

    -c  统计bytes数量

    -m 统计字符数量

    -l 统计行数

    -w 统计单词数量

    不加option地运行，得到的数字 第一个为行数 第二个为单词数 第三个为字节数

    文件路径可以不填，作为管道符的输入端口

16. 管道符 |

    将管道符左边命令的结果，作为右边命令的输入

    cat hello.txt | grep "hello"   在hello.txt 的内容汇总寻找含有"hello"的行

17. which

    which 要查找的linux程序

    给出要查找的linux程序的路径

18. find

    find 起始路径 -name "文件名"

    - 通过find命令查找指定文件

    find 起始路径 -size +/- N[kMG]

    - +/-表示大于小于 N表示数字大小 , kMG是单位 

    ```linux
    find / -size -10k    查找小于10kB的文件
    ```

    最好切换到root用户使用

    文件名可以使用通配符，进行模糊查找

19. echo

    echo 输出的内容

    只是单纯地输出内容

20. 反引号 `

    反引号引用的内容将被当成命令去执行

21. 重定向符 > or >>

    &gt;，将左侧命令的结果，覆盖写入到右侧文件中

    &gt;&gt;，将左侧命令的结果，追加写入到右侧文件中

    