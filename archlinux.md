# Arch Linux随记 #
-----------------
- pacman -Sc #清除现有安装包外pacman缓存

- yaourt --noconfirm packge-name #自动编译安装，有时需输入密码

- 若更新后界面出现显示问题，可在优化工具里更换主题

- 使用Shell找寻文本中出现频率最高10个字符串
`方法一：`
```Bash
#!/bin/sh
cat file.txt |awk '{
a[$1++]
}
END{
	for(i in a){
	print i,a[i]
	}
}|sort -nr |head -n 10
```
`方法二：`
```Bash
sort file.txt |uniq -c |sort -nr|head -n 10
```
- mutt自动收发邮件脚本 
  ```Bash
#!/bin/bash
cd /path
echo begin getmail
getmail -v -n 
cd ./temp
if test 0 -ne `ls ./ |wc -l`
then
rm ./*
fi
if test 0 -ne `ls ../homework/ |wc -l`
then
rm ../homework/*
fi
touch test.log
ls ~/Mail/inbox/new/ |awk '{
	temp_cat="cat ~/Mail/inbox/new/"$NF"|grep -e ^From:  >> test.log"
	temp_munpack="munpack ~/Mail/inbox/new/"$NF" >>test.log"
	system(temp_cat) 
	system(temp_munpack)
}'
sed  '/@/{N;s/\n/ /};s/.*<//g;s/>//g;s/(.*)//g' test.log >test2.log 
cat ./test2.log |awk '{
	print $2
	#temp_cp="cp "$2" ../homework/ans_"$2
	temp_pyrun="python3 ../barcodes_evaluation.py "$2"  ../barcodes_gt_test.txt > ../homework/ans_"$2
	system(temp_pyrun)
}'
cat ./test2.log |awk '{
temp1="echo homework results |mutt -s homework "$1" -a ../homework/ans_"$2 
system(temp1)
}'
if test 0 -ne `ls ~/Mail/inbox/new/ |wc -l`
then
mv ~/Mail/inbox/new/* ~/Mail/inbox/cur/
fi
echo end send mail
cd ..
```
启动定时脚本`$systemctl start cronie.service`

  设置每小时自动运行`$crontab -e`  
```Bash  
`0 * * * * /path/reply_homework.sh`
```

- 查找含有某字符串的所有文件`grep -rni LogisticRegressionOutput*`
	```
`*`: 表示当前目录所有文件，也可以是某个文件名
`-r` 是递归查找
`-n` 是显示行号
`-R` 查找所有文件包含子目录
`-i` 忽略大小写
`grep -i pattern files`：不区分大小写地搜索。默认情况区分大小写。 
`grep -l pattern files`：只列出匹配的文件名。
`grep -L pattern files`：列出不匹配的文件名。 
`grep -w pattern files`：只匹配整个单词，而不是字符串的一部分（如匹配`magic`，而不是`magical`）。 
`grep -C number pattern files`：匹配的上下文分别显示`[number]`行。 
`grep pattern1 | pattern2 files`：显示匹配`pattern1`或`pattern2`的行。 
`grep pattern1 files | grep pattern2`：显示匹配`pattern1`又匹配`pattern2`的行。
```

- `lsof`列出系统打开的文件

  ```
  `lsof -i` 用以显示符合条件的进程情况
  ```

- `netstat -nap | grep 10022`列出端口10022的使用情况。

- 查看线程占用
	```
`top -H`:一行显示一个线程。
`ps xH`:查看所有存在的线程。
`ps -mp <PID>`:查看一个进程起的线程数。
```


安装过程如果遇到permission denied的提示，就找到相应的文件，用chmod +x赋予其可执行权限

The directory /tmp/mathworks_30038/sys/java/jre/glnxa64/jre/bin/java does not exist.
/tmp/mathworks_30038/sys/java/jre/glnxa64/jre/bin/java does not appear to be a JRE directory.

./install -javadir /NSFCGZ/app/java/jdk1.8.0_11/jre -inputFile installer_input.txt



