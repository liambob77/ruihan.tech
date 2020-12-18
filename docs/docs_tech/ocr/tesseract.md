# tesseract OCR 点点滴滴

## tesseract 用法

tesseract --help-extra  
tesseract  --psm 7  test.png stdout digits  
tesseract -l chi_sim --psm 7  20201214091752.jpg stdout  
tesseract --list-langs  

## --psm 参数

>
0：定向脚本监测（OSD）
1： 使用OSD自动分页
2 ：自动分页，但是不使用OSD或OCR（Optical Character Recognition，光学字符识别）
3 ：全自动分页，但是没有使用OSD（默认）
4 ：假设可变大小的一个文本列。
5 ：假设垂直对齐文本的单个统一块。
6 ：假设一个统一的文本块。
7 ：将图像视为单个文本行。
8 ：将图像视为单个词。
9 ：将图像视为圆中的单个词。
10 ：将图像视为单个字符。
