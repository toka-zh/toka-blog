---
draft: true # 是否为草稿
title: Clion下运行多个main函数
status: public
date: 2020-09-28 10:48:45
tags:
categories:
---

在CMakeList.txt后面添加下面代码

```
# 遍历项目根目录下所有的 .cpp 文件
file` `(GLOB files *.cpp)
foreach (``file` `${files})
 ``string(REGEX REPLACE ``".+/(.+)\\..*"` `"\\1"` `exe ${``file``})
 ``add_executable (${exe} ${``file``})
 ``message (\ \ \ \ --\ src/${exe}.cpp\ will\ be\ compiled\ to\ bin/${exe})
endforeach ()
```



```
# 如果你只需要根目录下的 test 文件夹的所有 .cpp 文件
file (GLOB files test/*.cpp)
# 如果你只有两层目录的话
file (GLOB files *.cpp */*.cpp)
# 同理，三层的话
file (GLOB files *.cpp */*.cpp */*/*.cpp)
 
# 官方提供了一种递归的方法
# 这样在运行框会多一个 CMakeCXXCompilerId，不过无伤大雅
file (GLOB_RECURSE files *.cpp)
```

