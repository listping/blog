## 腾讯开源图计算框架Plato使用

github地址：https://github.com/Tencent/plato

##初始化依赖的编译环境，和构建连接库
进入plato-master执行命令如下
* install compile dependencies.

sh ./docker/install-dependencies.sh
* download and build staticlly linked libraries.

sh ./3rdtools.sh distclean && ./3rdtools.sh install

##必须安装的如下所示
*GCC
At least 4.8.5 for C++11 support.
*MPICH-3
Required for compiling and run Plato.
*OpenMP
Required for compiling and run Plato.
*Bazel-0.26
Required for compiling.

##对C++源码编译(Build)

BAZEL_LINKOPTS=-static-libstdc++ CC=/your_mpi_location/mpicxx bazel build example/...

##TEST
BAZEL_LINKOPTS=-static-libstdc++ CC=/your_mpi_location/mpicxx LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:${PWD}/3rd/hadoop2/lib bazel test --test_env=LD_LIBRARY_PATH plato/...

##修改run_pagerank.sh需要的hadoop环境

*指定jar包地址：
export CLASSPATH=${HADOOP_HOME}/etc/hadoop:`find ${HADOOP_HOME}/client/ | awk '{path=path":"$0}END{print path}'`
*指定libhdfs.so.0.0.0地址
export LD_LIBRARY_PATH="${plato-master}/bazel-bin/3rd/hadoop2.7.4/lib":${LD_LIBRARY_PATH}

##RUN

sh ./scripts/run_pagerank.sh

参考：https://www.xgithub.com/2019/11/08/tencent-plato-%E8%85%BE%E8%AE%AF%E9%AB%98%E6%80%A7%E8%83%BD%E5%9B%BE%E8%AE%A1%E7%AE%97%E6%A1%86%E6%9E%B6plato/



