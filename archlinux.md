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

- 天河二号中module配置
```Bash
#!/bin/bash
module purge
module load gcc/4.9.2 atlas/3.10.2 Python/2.7.9-gcc4.9.2 opencv/2.4.11 intel-compilers/15.0.1  intel-compilers/mkl-15 gmp/4.3.2 mpc/0.8.1 MPFR/2.4.2 ffmpeg/0.11.1 MPI/Gnu/MPICH/3.1-4.9.2
module unload Python/2.7.9-fPIC Python/2.7.9
module list
export PKG_CONFIG_PATH=/HOME/nsfc2015_304/share/pkgconfig 
```
`pkgconfig`中放有
```Bash
$ cat pkgconfig/opencv.pc 
#Package Information for pkg-config
prefix=/NSFCGZ/app/opencv/2.4.11
exec_prefix=${prefix}
libdir=${exec_prefix}/lib
includedir_old=${prefix}/include/opencv
includedir_new=${prefix}/include
Name: OpenCV
Description: Open Source Computer Vision Library
Version: 2.4.11
Libs: -L${exec_prefix}/lib  -lopencv_stitching -lopencv_objdetect -lopencv_superres -lopencv_videostab -lopencv_calib3d -lopencv_features2d -lopencv_highgui -lopencv_video -lopencv_photo -lopencv_ml -lopencv_imgproc -lopencv_flann -lopencv_core
Libs.private: -L/lib64 -lwebp -lpng -lz -ltiff -ljasper -ljpeg -lImath -lIlmImf -lIex -lHalf -lIlmThread -lgtk-3 -lgdk-3 -lpangocairo-1.0 -lpango-1.0 -latk-1.0 -lcairo-gobject -lcairo -lgdk_pixbuf-2.0 -lgio-2.0 -lgthread-2.0 -lgstvideo-1.0 -lgstapp-1.0 -lgstbase-1.0 -lgstriff-1.0 -lgstpbutils-1.0 -lgstreamer-1.0 -lgobject-2.0 -lglib-2.0 -ldc1394 -lv4l1 -lv4l2 -lavcodec -lavformat -lavutil -lswscale -lavresample -lgphoto2 -lgphoto2_port -lexif -lbz2 -ldl -lm -lpthread -lrt
Cflags: -I${includedir_old} -I${includedir_new}
```

- mutt自动收发邮件脚本 
```Bash
#!/bin/bash
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
	temp_cp="cp "$2" ../homework/ans_"$2
	system(temp_cp)
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