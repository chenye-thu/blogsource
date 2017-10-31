title: "MATLAB调用C++动态链接库（DLL）"
date: 2017-10-31 15:00:00 +0800
update: 2017-10-31 21:00:00 +0800
author: me
# cover: "-/images/sangshen.jpg"
tags:
    - 编程
    - MATLAB
    - C++
    - DLL
preview: 编程记录：MATLAB调用C++动态链接库（DLL）：loadlibrary方法；mex方法。

---

> 2017-10-31 周二 晴 北京 清华大学

## MATLAB调用C++动态链接库 ##
### 方法1：loadlibrary ###
参考这篇文章：
[Matlab调用C++ dll实例](http://www.jianshu.com/p/e910ca82b542)

首先生成DLL文件，这里使用的是VS2010。**特别要注意的是，如果是64bit平台，需要把编译平台改为64bit。**然后把生成的DLL文件和.h文件复制到MATLAB工作目录。

```
%MATLAB代码：
%加载matlabDllTest.DLL
loadlibrary('matlabDllTest','matlabDllTest.h');
%full选项会列出导出函数的详细输入和输出参数
libfunctions matlabDllTest -full
%调用函数，add是函数名，后面是函数的参数
calllib('matlabDllTest', 'add', 1.3, 4.6)
%停止加载DLL
unloadlibrary matlabDllTest
```


### 方法2：mex ###
参考这篇文章：
[MATLAB调用C++动态库的方法](https://wenku.baidu.com/view/3427724fe45c3b3567ec8b4b.html)

方法1比较简单，如果想得到矩阵之类的结果怎样做？貌似使用mex的方法是可以的。

把DLL文件、.h文件和.lib文件都复制到MATLAB工作目录，然后在此目录下新建一个**.cpp**文件，内容如下。重点是要包含***mex.h***，并且必须按照格式添加函数`mexFunction`。对于`mexFunction`函数，其四个参数意义为：nlhs是输出参数个数，plhs是输出参数指针，nrhs是输入参数个数，mxArray是输入参数指针。其用法可以参考下面的代码。在下面的代码中，输入参数为2个，输出的结果是两个1*2的矩阵（向量）。

```cpp
#include "mex.h" 
#include "matlabDllTest.h" 
#pragma comment(lib,"matlabDllTest.lib")
void mexFunction(int nlhs, mxArray *plhs[], int nrhs, const mxArray *prhs[])
{
	if( nrhs != 2)  //判断输入参数的个数
	{
		mexErrMsgTxt("输入参数个数不对!");
	}
	//得到传入的第一个参数，并转换了double类型       
	double p1 = *((double*)mxGetPr(prhs[0])); 
	//得到传入的第二个参数，并转换了double类型       
	double p2 = *((double*)mxGetPr(prhs[1]));       
	//创建结果为两个1*2的实时double类型的矩阵 
	plhs[0] = mxCreateDoubleMatrix(1, 2, mxREAL);
	plhs[1] = mxCreateDoubleMatrix(1, 2, mxREAL);
	//得到输出的参数的指针
	double *output = (double*)mxGetPr(plhs[0]); 
	double *output1 = (double*)mxGetPr(plhs[1]); 

	//调用matlabDllTest.dll定义的函数实现功能，并将返回值给输出参数      
	output[1] = add(p1, p2);
	output1[0] = add(p1, p2);
	output1[1] = p1 - p2;
}
```

然后，在MATLAB中，输入以下命令，注意`filename`是cpp文件名。

```
mex filename.cpp
```

这样你就会在目录里生成一个***filename.mexw32***或者***filename.mexw64***文件（取决于32位还是64位），这就是MEX文件。在此目录下，命令`filename`就会运行这当中的函数。

运行如下命令`filename(1,2)`会将第一个结果返回，如果需要看两个结果，可以执行`[a b] = filename(1,2)`，这样两个结果就分别赋值给`a`和`b`了。
