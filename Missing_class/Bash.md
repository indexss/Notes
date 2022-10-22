

# Bash

- pwd ->present working directory
- cd -> change directory    cd /usr/
- . -> current directory.      //cd ./home 进入当前目录的home
- ..-> parent directory.      // cd ..
- ls -> show the files in the current directory //ls ..
- cd ~ back to home directory
- cd - 回到上次的目录
- -l ->more info 
- drwxr-xr-x d为目录 wxr说明文件拥有者可以读写执行，文件所属组不能写，其他用户不能写 **三位一组**![img](https://img-blog.csdnimg.cn/20190615111930306.jpg)
- mv ->move **不是移动 是重命名rename**


```bash
mv 6.0001.md food.md
```

- cp ->copy cp file1name file2name 两个文件内容一样名字不一样

- rm ->remove删除//


- rm -r 只能删除文件/删除单个文件


- rm -R 文件和文件夹删除/删除目录以及里面的文件


- rm -f 忽略不存在的文件

- rmdir 删除空目录

- mkdir 生成新目录

- man -ls//-rm 使用手册
- 大于号小于号作为操作刘 大于号输出流 小于号输入流
- cat -> prints the contents of a file 