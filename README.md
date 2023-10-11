并行云平台采用任务提交式的方式运行程序，使用起来较为复杂。并行云平台适合利用脚本直接运行程序。故大家可以在组内服务器上先行debug，保证代码正确后再利用脚本提交运行。

为了方便同学们使用并行云平台，现在提供一个简单的例子。（更加详细的使用指南请查看![北京超级云计算中心N32-H分区用户手册v2.7.pdf](https://github.com/zcfinal/PARA_Example/blob/master/%E5%8C%97%E4%BA%AC%E8%B6%85%E7%BA%A7%E4%BA%91%E8%AE%A1%E7%AE%97%E4%B8%AD%E5%BF%83N32-H%E5%88%86%E5%8C%BA%E7%94%A8%E6%88%B7%E6%89%8B%E5%86%8Cv2.7.pdf)）

我们的例子是希望在平台上跑手写数字识别任务。

我们连接北京超级云scx6387的超算账号，这个账号下有8张40G A100的机器。

我们需要将数据、代码上传，并配置环境、提交程序，最后查看结果。

1. 上传数据。

打开快传应用，选择指定目录位置，点击上传按钮，将数据集上传解压。

2. 上传代码。

与上传数据一致，或者使用git等。

3. 配置环境。

打开ssh应用，连接服务器。

第一步，要配置软件环境（cuda、anaconda等）。

使用module avail查看已有的软件。

![Alt text](image.png)

使用module load 加载软件。module list查看已加载的软件。

![Alt text](image-1.png)

第二步，利用anaconda配置python环境。

conda create --name zhangchao python=3.8

![Alt text](image-2.png)

module加载anaconda软件后，source/conda activate zhangchao，激活环境。

北京超算：（

由于该分区机器是arm架构，使用关于gpu的包不能利用pip直接安装。

安装pytorch方式：pip install /home/bingxing2/apps/package/pytorch/1.13.1+cu117_cp38/*.whl (可能容易产生下载超时的报错，需要手动换源安装其他依赖包，比较麻烦。可以先使用一些已经安装配置好的conda环境，例如：py39_torch1.9.1_cu111。如果环境一直存在问题，可以联系各小组联络人。联络人会向平台工程师反馈，让其协助安装。)

安装对应版本的pytorch，需要源码编译。

cat /home/bingxing2/apps/package/pytorch/1.13.1+cu117_cp38/env.sh

安装完成后可在对应目录下查看env.sh信息。

不需要GPU的模块可以直接pip或conda安装。）

![Alt text](image-3.png)

4. 提交程序。

sbatch --gpus=1 -p vip_gpu_scx6387 run.sh

parajobs查看状态

5. 查看结果

查看*.out的文件。

![Alt text](image-4.png)

可以修改日志的保存路径

sbatch -o 日志文件名

sbatch -e 错误文件名



另外的一些命令：

1. scontrol show job 作业号，查看提交脚本以及运行环境
