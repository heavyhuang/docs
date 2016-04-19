# Arch Linux随记 #
-----------------
- pacman -Sc #清除现有安装包外pacman缓存

- yaourt --noconfirm packge-name #自动编译安装，有时需输入密码

- 若更新后界面出现显示问题，可在优化工具里更换主题
- 使用Shell找寻文本中出现频率最高10个字符串
方法一：
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
方法二：
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
pkgconfig中放有
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