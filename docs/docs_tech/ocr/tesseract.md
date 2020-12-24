# tesseract OCR

## tesseract 用法

tesseract --help-extra  
tesseract  --psm 7  test.png stdout digits  
tesseract -l chi_sim --psm 7  20201214091752.jpg stdout  
tesseract --list-langs  

## --psm 参数

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

## Tesseract训练

大体流程为：安装jTessBoxEditor -> 获取样本文件 -> Merge样本文件 –> 生成BOX文件 -> 定义字符配置文件 -> 字符矫正 -> 执行批处理文件 -> 将生成的traineddata放入tessdata中

### 获取样本文件

画图打开，另存为tif文件 

### Merge样本文件

打开jTessBoxEditor，Tools->Merge TIFF，将样本文件全部选上，并将合并文件保存

### 生成BOX文件

tesseract testlang.normal.exp0.tif -l chi_sim testlang.normal.exp0 makebox

### 定义字符配置文件

在目标文件夹内生成一个名为font_properties的文本文件，内容为  

font 0 0 0 0 0
【语法】：<fontname> <italic> <bold> <fixed> <serif> <fraktur>    

fontname为字体名称，italic为斜体，bold为黑体字，fixed为默认字体，serif为衬线字体，fraktur德文黑字体，1和0代表有和无，精细区分时可使用。  

### 字符矫正

打开jTessBoxEditor，BOX Editor -> Open，打开num.font.exp0.tif；矫正<Char>上的字符  
  
### 执行批处理文件

在目标目录下生成一个批处理文件

``` bat
rem 执行改批处理前先要目录下创建font_properties文件 

echo Run Tesseract for Training.. 
tesseract.exe num.font.exp0.tif num.font.exp0 nobatch box.train 
 
echo Compute the Character Set.. 
unicharset_extractor.exe num.font.exp0.box 
mftraining -F font_properties -U unicharset -O num.unicharset num.font.exp0.tr 


echo Clustering.. 
cntraining.exe num.font.exp0.tr 

echo Rename Files.. 
rename normproto num.normproto 
rename inttemp num.inttemp 
rename pffmtable num.pffmtable 
rename shapetable num.shapetable  

echo Create Tessdata.. 
combine_tessdata.exe num. 

echo. & pause
```

### 将生成的traineddata放入tessdata中

最后将num.trainddata复制到Tesseract-OCR中tessdata文件夹即可。  

## 最后的测试
