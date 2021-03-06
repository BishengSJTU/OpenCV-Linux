# Linux下　OpenCV的安装及contrib模块的安装
**PCL1.7/1.8 opencv2/3/4 编译安装共存**

为了使用cuda和pcl共同编程，而系统带的pcl1.7不带gpu模块，故编译安装pcl完全版，与系统pcl1.7共存，不同分发版本Ubuntu应该没有什么区别，不同版本pcl编译和使用道理也都基本相同，opencv也是一样的道理。
PCL 源码编译安装：
这里以pcl-1.8为例，其他版本可在GitHub上pcl代码仓库的release处下载源码。
从GitHub克隆源码，建立build文件夹准备编译：

    git clone https://github.com/PointCloudLibrary/pcl.git
    cd pcl
    mkdir build
    cd build

一般编译（然而我们不这么编译，看下一条）：

    cmake -D　CMAKE_BUILD_TYPE=Release ..

指定gpu功能的编译，并指定安装文件位置为“/home/ziqi/pcl-1.8”：

    cmake -D　CMAKE_BUILD_TYPE=Release -D　CMAKE_INSTALL_PREFIX=/home/ziqi/pcl-1.8 -D　BUILD_GPU=ON -D　BUILD_apps=ON -D　BUILD_examples=ON ..

根据cpu核心数目不同，选择编译使用的-j选项，如使用6个核心编译：

    sudo make 

编译后安装到之前指定的文件夹（即“/home/ziqi/pcl-1.8”）

    sudo make install

因为我们把pcl-1.8安装到了 “/home/ziqi/pcl-1.8”，所以任何程序调用pcl-1.8库，都要在CMakeLists.txt里设置PCLConfig.cmake这个文件所在目录，之后才能在CMakeLists.txt里面使用find_package()，其中，PCL_DIR的值是PCLConfig.cmake这个文件所在的目录。

    set(PCL_DIR "/home/ziqi/pcl-1.8/share/pcl-1.8")
    find_package(PCL 1.8 REQUIRED COMPONENTS)
    include_directiories(${PCL_INCLUDE_DIRS})
    link_directories(${PCL_LIBRARY_DIRS})
    add_definitions(${PCL_DEFINATIONS})

因为我们把pcl-1.8安装到了一个新的位置，没有覆盖安装系统原来的pcl-1.7，因此是可以共存的。使用源码编译安装的pcl-1.8的时候，只需要如上修改PCL_DIR即可。
cuda pcl混合编程的一个例子可以使用我测试使用的代码：<a href="https://github.com/ChaiZQ/pcl-with-cuda.git" rel="nofollow" target="_blank">测试代码</a>。</p>

**opencv3.2&amp;opencv3.2-contribute 源码编译安装：**

<p>下载源码，解压缩，比如解压后的文件夹为“opencv-3.2.0”：<br>
建立build文件夹准备编译：</p>

    cd /opencv-3.2.0
    rm -rf build
    mkdir build
    cd build

编译两个包，指明安装位置，默认为/usr/local

    cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local/opencv320 -D WITH_CUDA=OFF -D CUDA_GENERATION=Kepler ..
    cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local/opencv320 -D WITH_CUDA=OFF -D CUDA_GENERATION=Kepler -D OPENCV_EXTRA_MODULES_PATH=(opencv_contrib路径)/opencv_contrib-3.3.1/modules/ ..

如果选择安装opencv_contrib模块则选择下面一种编译方式
编译错误解决方法：
法１．if you meet the fatal error: boostdesc_bgm.i: No such file or directory or fatal error: vgg.i: No such file or directory click here: https://github.com/opencv/opencv_3rdparty/branches/stale enter the "contrib_xfeatures2d_vgg_20160317" and "contrib_xfeatures2d_boostdesc_20161012" and then download the all ".i" . put the all ".i" in 'opencv_contrib/modules/xfeatures2d/src/' than, make

added this path "/home/cw/mytool/opencv-3.4.6/opencv_contrib-3.4.6/modules/xfeatures2d/include/opencv2/xfeatures2d/cuda.hpp" replacing 52 line that is "opencv2/xfeatures2d/cuda.hpp" in file which path is "opencv-3.4.0/modules/stitching/include/opencv2/stitching/detail/matchers.hpp"

法2.-D BUILD_opencv_xfeatures2d=OFF

根据cpu核心数目不同，选择编译使用的-j选项，如使用6个核心编译：

    sudo make 

安装：

    sudo make install

如果没有安装在默认/usr/local，比如安装在/usr/local/opencv320，则任何程序调用opencv库，都要在CMakeLists.txt里设置OpenCVConfig.cmake这个文件所在目录，之后才能在CMakeLists.txt里面使用

    set(OpenCV_DIR  "/usr/local/opencv320/share/OpenCV/")
    find_package(OpenCV)
    include_directiories(${OpenCV_INCLUDE_DIRS})
    link_directories(${OpenCV_LIBRARY_DIRS})
    add_definitions(${OpenCV_DEFINATIONS})

