# BoostPython
How to use boostPython

首先从boostPython 下载源文件， 把Python2.7的安装路径添加到环境变量， 用cmd命令执行bootstarp.bat 如果报错找不到tlhelp.h文件
则在include环境变量中加入C:\Program Files (x86)\Windows Kits\8.0\include\um;
若报错找不到ntverp.h文件则在环境变量中加入C:\Program Files (x86)\Windows Kits\8.0\Include\shared

此时应该可以正常的执行bootstrap.bat,结果会生成b2.exe和bjam.exe两个exe文件，以及一个project-config.jam,若不懂则不动这个文件

用b2 --build-type=complete 命令编译boost,其他配置不要动，默认生成的库文件以及dll文件在stage文件夹下。。

导出上述testboostpython文件给python用的时候报错：fatal error LNK1104: cannot open file 'boost_pythonPY_MAJOR_VERSIONPY_MINOR_VERSION-vc141-mt-x64-1_67.lib'
上诉报错可以把boost/python/detail中的config.hpp文件修改如下：

注释这一行
// annotation by OuYang
//#define BOOST_LIB_NAME boost_python##PY_MAJOR_VERSION##PY_MINOR_VERSION

添加如下
// Add by ouyang
#define _BOOST_PYTHON_CONCAT(N, M, m) N ## M ## m 
#define BOOST_PYTHON_CONCAT(N, M, m) _BOOST_PYTHON_CONCAT(N, M, m)
#define BOOST_LIB_NAME BOOST_PYTHON_CONCAT(boost_python, PY_MAJOR_VERSION, PY_MINOR_VERSION)


#include <boost/config/auto_link.hpp>
#endif  // auto-linking disabled
之后加如下文件
// add by ouyang
#undef BOOST_PYTHON_CONCAT
#undef _BOOST_PYTHON_CONCAT

至此完成。。。。搞了两天。。。网上都是乱说，，杂而不全。。。在这里记下，希望能够帮助与我遇到过同样问题的人
