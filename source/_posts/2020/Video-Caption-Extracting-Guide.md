---
title: "手抄字幕指南"
tags: ["ACGN"]
date: 2020-08-13 10:34:05
update: 2020-10-14 11:20:05
categories: "工具手册"
thumbnail:
---

最近在整理 CASO 制作的《向阳素描》，但其中第二季只能找到内嵌字幕，不得不手抄。姑且在这里整理一下手抄字幕的方法和工作流程。

## 工具
手抄字幕需要用到以下工具：
* COCR：视频硬字幕提取工具，用于代替古老的 exrXP。
  * 用于从视频中截取字幕部分，进行图像处理、OCR，得到粗略的文本和时间轴。
  * 开源软件，Apache-2.0 License，项目地址：[sum1re/caption_ocr_tool](https://github.com/sum1re/caption_ocr_tool)
* Aegisub：字幕编辑工具。
  * 用于编辑处理字幕、调整时间轴。
  * 开源软件，官网：[http://www.aegisub.org/](http://www.aegisub.org/)，项目地址：[Aegisub/Aegisub](https://github.com/Aegisub/Aegisub)
  * 官网上最新的稳定版是 2014 年发布的 3.2.2，但实际上最新的可用版本是 2018 年发布的 r8942。推荐后者，下载地址在：[http://plorkyeran.com/aegisub/](http://plorkyeran.com/aegisub/)
  * 务必使用安装包安装，便携版将没有中文翻译。
* VS Code 及 Lyrics/Subtitles Support 插件：文本编辑器
  * 用于直接修改 ass 字幕。

## 工作流程
1. 对每一集进行 COCR 处理，不保留 OP/ED 的字幕，不考虑注释字幕。
2. 用 Aegisub 重新保存每一集的字幕。（用于解决 COCR 生成的有问题的时间标注）
3. 在播放器中播放每一集，检查字幕完整性，添加缺失的注释、多行字幕。
4. 根据 BD 版重新调整时间轴。
### COCR 处理

1. 在 COCR 中打开视频；
2. 打开滤镜设置，确认滤镜设置无误 →[滤镜设置](#滤镜设置)；
3. 点击开始，等待视频图像处理；
4. 保存 COCR 工程文件；
5. 删除合并字幕图片，注意保留质量最优的，每隔一段时间保存 →[删除合并字幕图片](#删除合并字幕图片)；
6. 启动 OCR，等待 OCR 结束，完成后立即保存，得到识别后的字幕文字；
7. 进行 OCR 结果预处理 →[文字预处理](#文字预处理)；
8. 开始对照图片校对，注意定时保存；
9. 导出为 ass 字幕。

#### 滤镜设置
下面的设置仅为参考，请根据实际情况调整。
* 裁剪：根据字幕位置调整好坐标，尺寸应当于字幕相当
* 色彩模型：
  * 模型：COLOR_BGR2GRAY
* 自适应二值化：
  * 像素值上限：255
  * 自适应算法：ADAPTIVE_THRESH_MEAN_C
  * 阈值类型：THRESH_BINARY_INV（使得处理后字幕是白底黑字）
  * 邻域尺寸：45
  * 常数：-45

#### 删除合并字幕图片
COCR 中，删除合并字幕图片的操作方法如下：
* 删除字幕：**左键**点击需要删除的字幕；
* 批量删除字幕：**右键**点击第一条需要删除的字幕，**左键**点击最后一条需要删除的字幕；
* 合并重复字幕：**右键**点击第一条重复字幕，**右键**点击最后一条重复字幕，之后再**左键**点击需要保留的字幕。

关于标记的说明：
* 红色的`DEL`标记表示将要被删除的字幕；
* 紫色的`B`、`M`、`E`分别表示一组重复字幕的开头、中间、结尾，`S`标记表示最终保留的字幕图象。

（我试图与原作者沟通，把这个表述加入到文档中，但原作者认为有图就够了）

#### 文字预处理
一般需要按顺序进行下面的这些批量替换：
* 删除 `;`、`:`、`；`、`：`、`.`、`,`、`。`、`，`、`+`、`、`
  * 正则 `[;:；：.,。，+、]`
* 删除`【`、`】`、`“`、`”`、`‖`、`『`、`』`、`{`、`}`、`[`、`]`
  * 正则 `[【】“”‖『』\{\}\[\]]`
* 视情况删除所有字母 `[a-zA-Z]`
* `!` -> `！`
* `?` -> `？`
* `  ` -> ` `（两个空格合并为一个，需要重复多次）
* `\n ` -> `\n`
* ` \n` -> `\n`

### Aegisub 处理
（还没写完 QAQ）