---
title: Is It Possible to Run CUDA on AMD GPUs?
tags:
  - MATLAB
categories: 技术
index_img: /img/20200405-CUDA.jpg
typora-root-url: ..
abbrlink: 10003
date: 2020-04-05 18:29:59
---

MATLAB工具箱之GPU计算之AMD or NVIDIA

<!-- more -->

---

奶盖高中同学今晚突然找她问我是不是用MATLAB，打算让我帮他看一下代码，虽然我是个辣鸡，但本着试试看的心态我就答应了，结果一运行，出现了这么一个问题

![图1](/ass/20200405-CUDA1.png)

他说他朋友电脑可以跑，有点郁闷。

先看错误行`bone=zeros(NX,NY,'single','gpuArray');`，出现了gpu字样，不懂什么东西，先help一下`help zeros`，然后在参考页最下方的“扩展功能”处有“GPU数组”，让我参阅**Parallel Computing Toolbox**文档中的zeros，工具箱？有意思，老早就听说过MATLAB的工具箱了，一直没机会尝试，瞬间兴趣来了，进去看看

![图2](/ass/20200405-CUDA2.jpg)

出现了！`gpuArray`与报错一致，而这是**<abbr title= "Parallel Computing Toolbox">PCT</abbr>**的类型名，应该要安装这个并行运算工具箱，ok，回到MATLAB主界面，工具栏APP，获取更多APP，瞬间乡下人进村，竟然还有Super Mario，可以可以。

![图3](/ass/20200405-CUDA3.png)

先去找**PCT**，途中看到一个GPU Coder，心想搞不好一会还要用到它，暂时先不管。下载安装**PCT**，运行程序

![图4](/ass/20200405-CUDA4.png)

又错了。。。什么原因？缺少**CUDA**驱动，哎，等等，**CUDA**好像在哪见过，刚才的GPU Coder是干啥的来着？

![图5](/ass/20200405-CUDA5.png)

产生**CUDA**代码，好，不管其他的了，先试试再说。进去之后，还需要安装两个东西，一个刚才已经安装了**PCT**，还有一个MATLAB Coder，一并装上，万事大吉，激动人心的时刻就要到来了，运行代码

<center>😭</center>

依然是一样的错误，

> The CUDA driver could not be loaded. The library name used was 'nvcuda.dll'. The error was: 找不到指定的模块。

行吧，逼我上Google。一番搜索之后，锁定一个2016年来自MathWorks官方 MathWorks Support Team 的[优质回答](https://www.mathworks.com/matlabcentral/answers/278938-why-can-i-not-use-my-gpu-within-matlab-for-gpu-computing)，

> \1.   Make sure your license of Parallel Computing Toolbox works
>
> In MATLAB you can run the following command to check your license:
>
> `license checkout Distrib_Computing_Toolbox`
>
> You will receive the output "**ANS=1**" if the license is working. Otherwise, you will see a license manager error. In that case, searching for the license error message should give a solution for how to solve your issue.
>
> \2.   Check you have a supported GPU card, see **NVIDIA website** to check compute capability (<https://developer.nvidia.com/cuda-gpus> for older cards consult https://developer.nvidia.com/cuda-legacy-gpus). Compute capability must be 1.3 or higher for versions R2014a and earlier and 2.0 or higher for versions R2014b or later and 3.0 or higher for convolution neural networks (introduced in R2016a).
>
> \3.   Update your NVIDIA graphics drivers to their latest version for your card (<http://www.nvidia.com/Download/index.aspx>). It is not necessary to download the CUDA SDK for built-in GPU functionality in MATLAB. The CUDA SDK is required if you wish to compile your own CUDA code to use with MATLAB.
>
> \4.   If you still cannot access your CUDA enabled device from within MATLAB check whether the device is correctly identified by your operating system using a program like NVIDIA-smi. If it is not seek advice from your sysadmin or NVIDIA support.
>
> To run NVIDIA-smi:
>
> Windows: Navigate to "C:\Program Files\NVIDIA Corporation\NVSMI\" and execute nvidia-smi in a command window.
>
> Linux: nvidia-smi from terminal window. Normally installed into /usr/bin/
>
> Mac: Not available.
>
> \5.   In some environments such as computing clusters access to CUDA capable devices on machines are handled at a system level by techniques like control groups or general resource allocation. Checking for the presence of these systems can explain why GPUs are visible but restricted in access for other programs.
>
> CUDA_VISIBLE_DEVICES is an environment variable which when set will only allow access to the valid CUDA devices listed in the variable. The environment variable acts as a mask on any underlying CUDA devices.
>
> CUDA_VISIBLE_DEVICES = '0'             %Only device 0 is accessible and usable.
>
> CUDA_VISIBLE_DEVICES = '0, 1'           %Devices 0 and 1 are accessible and usable.
>
> CUDA_VISIBLE_DEVICES= 'NoDevApps'      %(or any other invalid input) no CUDA devices are accessible.
>
> If CUDA_VISIBLE_DEVICES is unset then all CUDA devices are unrestricted.
>
> In a cgroup (Linux) setup resource accessibility is controlled at a kernel level. This is a sysadmin level setting. This is more likely to exist on controlled HPC clusters.
>
> If the steps in this support solution have not helped you resolve the problem, please contact MathWorks support directly:
>
> http://www.mathworks.com/support/contact_us/

第一步check，第二步为什么去NVIDIA网站看？AMD不行吗？搜一下吧，Google “AMD Parallel Computing Toolbox”，

有这么几个答案

> https://www.mathworks.com/matlabcentral/answers/477269-is-native-macintosh-amd-opencl-support-available
>
> https://www.mathworks.com/matlabcentral/answers/96468-are-amd-graphics-cards-supported-for-gpu-computation-with-the-parallel-computing-toolbox-5-2-r2011b
>
> https://stackoverflow.com/questions/12828268/is-it-possible-to-run-cuda-on-amd-gpus

而答案就是==Nope==.

折腾一晚上，竟然是AMD和NVIDIA的差别，那我没办法了​​。看答案说用OpenCL替代，但是这要麻烦的多，让他用朋友电脑跑吧。继续研究下去就又是一个故事了。

---

总结：以后买电脑要NVIDIA显卡的。

---