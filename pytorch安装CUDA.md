## pytorch安装CUDA
### 1.cuda下载安装
 * [CUDA10.1安装包下载](https://www.cnblogs.com/imper/p/11976077.html)
 * [cuda、cuDNN安装](https://zhuanlan.zhihu.com/p/158900161)
 
### 2.安装pytorch

去pytorch官网选择合适的版本：

*[选择合适版本](https://pytorch.org/get-started/locally/)

然后按照官网的找到自己的安装方式：

pip install torch==1.5.1+cu101 torchvision==0.6.1+cu101 -f https://download.pytorch.org/whl/torch_stable.html

由于从官方网站安装了一天，总是安装不成功，报错如下：

 HTTPSConnectionPool(host='download.pytorch.org', port=443): Read timed out.
 
 因此本人最终决定下载在本地再进行安装
 
 *[各种版本下载地址](https://download.pytorch.org/whl/torch_stable.html)
 * 1）找到torch-1.5.1+cu101-cp36-cp36m-win_amd64.whl和 torchvision-0.6.1+cu101-cp36-cp36m-win_amd64.whl的下载地址，然后用迅雷下载，下载速度飞快
 * 2）cmd命令进入下载目录 D:\download\迅雷下载
 * 3）从本地进行安装，安装命令如下：
 
 pip install -i https://pypi.tuna.tsinghua.edu.cn/simple torch-1.5.1+cu101-cp36-cp36m-win_amd64.whl
 
 pip install -i https://pypi.tuna.tsinghua.edu.cn/simple torchvision-0.6.1+cu101-cp36-cp36m-win_amd64.whl
 


