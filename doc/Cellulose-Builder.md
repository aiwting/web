# Cellulose-Builder
## 一、简介
Cellulose-Builder 是一个 bash 脚本，可以在任何类 Unix 平台上运行，由坎皮纳斯州立大学(UNICAMP)斯卡夫化学研究所开发。该程序可以构建不同尺寸和几何形状的纤维素晶体结构，为指定纤维素结构的所有原子生成笛卡尔坐标，适合用作分子动力学模拟和其他计算中的起始结构。
从他们发布的网站下载需要魔法。(https://code.google.com/archive/p/cellulose-builder/downloads)  
这里提供下载好的脚本作为参考和学习：[>>百度网盘<<](https://pan.baidu.com/s/1L6uzSwMhFk2RGb41h9G_ew?pwd=ydem)

## 二、软件依赖
该程序的运行依赖于三个程序包：
>1. octave
>2. VMD
>3. psfgen

下面以Debian系统为例，展示Cellulose-Builder的配置及使用。（如果你是Ubuntu，那么同样适用，如果你是Red Hat系，替换一些必要命令即可）。
首先移动到工作目录，下面的所有软件都安装在工作目录中，这里我的工作目录为账户主目录（/home/youraccountname）  
`cd ~`

2.1 octave 是一种编程语言，用于工程、科学计算，在这里用于建模过程中的数值计算。在Debian系统中可以使用以下命令安装：
`sudo apt-get install octave`

2.2 [VMD](https://www.ks.uiuc.edu/Research/vmd/) 是一款专门用于可视化分子模拟的软件工具，在官网注册登录即可免费下载。选择Linux平台最新版本下载即可，例如：  
*【Version 1.9.4 LATEST ALPHA (2022-04-27) Platforms:LINUX_64 (RHEL 7+) OpenGL, CUDA, OptiX RTX, OSPRay (Linux (RHEL 7+) 64-bit Intel/AMD x86_64 SSE/AVX+ with CUDA 10, OptiX6.5 RTX, OSPRay)】*  
将安装包“vmd-1.9.4a57.bin.LINUXAMD64-CUDA102-OptiX650-OSPRay185.opengl.tar.gz”移动到工作目录解压安装：
```
tar -xzvf vmd-1.9.4a57.bin.LINUXAMD64-CUDA102-OptiX650-OSPRay185.opengl.tar.gz
cd vmd-1.9.4a57
./configure LINUXAMD64
./configure
cd src
make install
```
2.3 psfgen 是一种在分子模拟和建模中广泛使用的工具，特别是在处理蛋白质和其他生物大分子的结构文件时。它主要用于生成和修改分子动力学模拟所需的文件，特别是PSF（Protein Structure Files，蛋白质结构文件）和PDB（Protein Data Bank，蛋白质数据库文件）文件。由于[NVMD](https://www.ks.uiuc.edu/Research/namd/)中包含了该程序，因此安装NVMD即可。选择Linux平台cuda版本，例如：  
*【Version 3.0 (2024-06-14) Platforms:Linux-x86_64-multicore-CUDA (NVIDIA CUDA acceleration)】*  
请使用CUDA版本的NAMD中的psfgen，否则运行时可能无法生成.psf和.pdb文件。将安装包“NAMD_3.0_Linux-x86_64-multicore-CUDA.tar.gz”移动到工作目录解压即可：
`tar -xzvf NAMD_3.0_Linux-x86_64-multicore-CUDA.tar.gz`

2.4 将提供的“cellulose-builder_July_2013.zip”复制到工作目录，解压缩即可：
`unzip cellulose-builder_July_2013.zip`

2.5 最后，还需要将所有依赖软件的可执行文件放在PATH中，以便程序调用。主要是将psfgen添加到PATH变量中：  
编辑文件.profile文件
`vim ~/.profile`
在文件末尾添加一行文本
`export PATH="$PATH:/home/zz/NAMD_3.0b6_Linux-x86_64-multicore-CUDA/"`
刷新shell环境
`source ~/.profile`
检查是否可以运行
```
cd cellulose-builder_July_2013
./cellulose-builder.sh
```
对于VMD和NVMD的下载注意：
1. 如果你的Linux安装了可是化桌面，直接使用浏览器下载安装包即可。
2. 如果你使用的是Windows平台的WSL，使用Windows的浏览器下载即可。
3. 如果你是纯命令行的Linux，首先在另一台可登录网页的设备上注册登录下载，然后在下载界面右键复制获取下载链接url，再使用wget命令即可在命令行中下载：
`wget \<url\>`

## 三、cellulose-builder命令
cellulose-builder 接受以下三种指令输入：
```
./cellulose-builder [integer] [integer] [integer]
./cellulose-builder fibril [integer]
./cellulose-builder origin|center|monolayer [integer] [integer]
```
3.1 第一个生成平行六面体纤维素晶体：用户必须提供三个整数，分别代表每个晶轴上的晶胞复制重复的数量（及链的层数、每层链数、每条链的纤维二糖数）。前两个整数必须大于1，而最后一个整数必须大于0。例如下面的指令将会生成一个(2*2-1)=3层纤维素平面，每个纤维素平面由4（或3）条纤维素单链构成，每个纤维素单链包含3个纤维二糖的平行六面体晶体：
`./cellulose-builder.sh 2 4 3`

3.2 第二个形成36链纤维素原纤维：必须提供字符串fibril作为第一个参数，同时提供一个大于0的整数作为第二个参数，代表每个纤维素链中纤维二糖的数量。

3.3 第三个生成单层的纤维素平面：必须输入origin或center或monolayer作为第一个参数，然后输入两个大于1的整数，分别代表单层中包含的纤维素链的数量和每条链中纤维二糖的数量。其中origin的纤维二糖为I-beta构象，center为II-beta构象，monolayer为I-alpha构象。

3.4 输出：成功运行命令会得到一个名为crystal的文件夹，里面包含：crystal.pdb, crystal.psf, crystal.xyz, psfgen.sh, psfgen.log；其中.pdb, .psf, .xyz是相应的结构文件，另外两个是程序运行过程中生成的。

3.5 其它自定义：cellulose-builder还支持构建多种类型的结晶纤维素，可以通过编辑输入文件input.inp来控制。input.inp内容如下：
```
PHASE=I-BETA   # Accepted values are: I-ALPHA , I-BETA , II , III_I .
PBC=NONE       # Accepted values are: NONE (default), A , B , ALL .
PCB_c=TRUE    # Accepted values are: FALSE , TRUE .
```
>1. PHASE 为可选的纤维素构象。
>2. PBC 控制平移对称性：即是否沿晶体学方向a、b构建具有平移对称性的微晶，使其暴露不同的晶面。
>3. PCB_c为纤维素链周期性共价键设置：PCB_c=TRUE 表示启用周期性，此选项将阻止在链末端添加 H 和 OH 原子，使纤维素链末端能够与相邻的周期图像形成糖苷键。PCB_c=FALSE 表示纤维素链不会周期性地与相邻图像共价键合，每条纤维素链都带有一个额外的水分子（一个 H 原子在一个末端，一个 OH 基团在另一个末端）。

更多的使用展示、配置、及文件含义可以从他们的论文中获得，DOI:10.1002/jcc.22959  
论文链接：https://onlinelibrary.wiley.com/doi/abs/10.1002/jcc.22959  
参考链接：[Cellulose Builder 用户指南-CSDN博客](https://blog.csdn.net/CocoCream/article/details/130149772?spm=1001.2014.3001.5501)  

写于2024年7月

[<<首页](/)