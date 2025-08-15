# Project 2:  基于数字水印的图片泄露检测

本项目通过python实现图片水印嵌入和提取，并进行包括翻转、平移、截取、调对比度的鲁棒性测试，经过测试，水印可以抵挡翻转和平移的攻击，但对于截取和调对比度还有所不足。
##  水印方案 
本水印提取需要原始图像支持，属于盲水印（不依赖原图）与非盲水印之间的非盲方案。方法使用离散小波变换 (DWT) 。
###  嵌入方案
由于SM4需要进行S盒查找和线性变换，因此我使用T_table，通过查表来原来的方法进行优化。

## DWT原理
### 步骤 1：DWT 分解（Haar 小波）
$I \xrightarrow{\text{DWT}} (LL, LH, HL, HH)$

### 步骤 2：在 LH 子带嵌入水印
嵌入水印公式：
$LH' = LH + \alpha \cdot \hat{W}$，因此提取水印只需要变换方程即可。

### 步骤 3：逆小波变换
水印提取的过程中需要进行逆小波变换：$I' = \mathrm{IDWT}(LL, LH', HL, HH)$，之后求解$\hat{W}$


## 实现过程与鲁棒性测试

这里原始图片为

![alt text](reference.png)

水印选择：

![alt text](watermark.png)

提取出的水印：

![alt text](results/exteacted.png)

### 鲁棒性测试
这里我对图片进行翻转、平移、截取、调对比度的更改，检测水印的鲁棒性。

- 翻转：图片翻过来
- 平移：x 方向平移 40 像素， y 方向平移 20 像素
- 截取：进行图片裁剪，裁剪指定区域，这里我选择[100:200, 50:300]
- 调对比度：不改变亮度，将对比度提高一倍

这里给出翻转结果：

![alt text](results/flipped_exteacted.png)

其他结果可以查看文件，只有反转和平移将水印大致恢复。
##  安装 Installation

```bash
git clone https://github.com/Sunburnlyz/MyProject.git
cd MyProject/Project2
```
##  运行 Running
直接运行main函数即可。