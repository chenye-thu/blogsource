title: "coding记录-20170516"
date: 2017-05-16 22:40:00 +0800
update: 2017-05-17 19:40:00 +0800
author: me
# cover: "-/images/sangshen.jpg"
tags:
    - 编程
    - Qt
    - PanEngine
preview: 编程记录:在Qt中使用PanEngine记录。

---

> 2017-05-16 周二 晴 北京 院里

## 在Qt中使用PanEngine记录
1. PanEngine 2014 demo工程在**VS2010**中成功编译运行。
2. 首先在**Qt4.8.1**中使用PanEngine，报错一大堆，失败。
3. 换成**Qt5.7.0**，构建成功，可以运行。但是在执行具体示例时出错，程序崩溃。
4. 调试发现是执行`PanEngine`类内的虚函数时出错。（该类是从dll中调用的）
``` cpp
#define PANENGINE_API _declspec(dllexport)
...
class PANENGINE_API PanEngine {
public:
	PanEngine(void);
	virtual string pe_set_configuration(map<string, string>& config) = 0;
...
}
```

5. 但是同样在`PanEngineX.h`内使用如下代码调用的函数可以执行成功。
``` cpp
#define PANENGINE_API _declspec(dllexport)
...
extern "C" PANENGINE_API PanEngine* definePanEngineUser(char* msg);
```

6. 因此怀疑是调用dll中的类出错。查了一下感觉没有错。
7. 使用如下方式调用，仍然出错。
``` cpp
typedef PanEngine* (*GetPanEngine)(char *);
...
	PanEngine* user;
	char msg[256];
    QLibrary myLib("PanEngineX.dll");
    if(myLib.load())
    {
        GetPanEngine getPanEngine = (GetPanEngine)myLib.resolve("definePanEngineUser");
        if(getPanEngine)
        {
            user = getPanEngine(msg);
        }
    }
```

8. 怀疑是`include`的文件不一样导致的，修改了两个，还是不行。毕竟VS和Qt都各有一套头文件，所以尝试把所有`include`的文件都修改一遍试试。还是不行，因为各自的文件是一层一层引用的，会出问题。
9. 尝试安装了VS2010的Qt插件，编译成功，但是例子运行时依然会崩溃，实在找不到问题所在了。

***失败了...***
