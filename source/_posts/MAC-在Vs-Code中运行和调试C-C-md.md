---
title: MAC - 在VsCode中运行和调试C/C++.md
date: 2022-03-19 18:38:58
tags: [VsCode, C/C++, Mac]
author: VsKendo
categories: C/C++
summary: 在Mac上，使用VsCode作为C/C++的IDE，发现还是有不少坑的。
---

# 配置说明

使用2020款m1的13.3英寸mbp，内存16GB。

# 安装VsCode

去[官网](https://code.visualstudio.com/) 下载vscode。

现在vscode已经完美支持m1。

# 安装VsCode的插件

- Reload：非必需。右下角多一个Reload标签，点击以后可以一键重启VsCode。
- C/C++：用于运行C和C++的程序。这插件会安装gcc，这个gcc也是完美支持m1。
- Code Runner：在右上角多出个箭头，可以一键运行。
- CodeLLDB：用于调试，没有它vscode的断点会断不下来。
- tabnine：非必需。强大的自动补全插件，基于AI。免费版每天可以补全5000次。

# 配置插件

在`~/Library/Application Support/Code/User/settings.json`中配置`Code Runner`。

我们配置它的`code-runner.executorMap`中的

```json
"cpp": "cd $dir && g++ -std=c++11 $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
```

这样code runner才会编译成c++11。要不然一堆feature用不了。现在大大小小比赛都完美支持C++11了，部分大型比赛支持C++14。

然后在`code-runner.executorMap`下面，额外增加两行：

```json
"code-runner.saveFileBeforeRun": true,
"code-runner.runInTerminal": true
```

第一行会在运行前自动保存文件，不用每次都command+s保存然后再点运行。

第二行会让程序运行在控制台中，否则cin或者scanf等指令没办法输入。

# 运行代码

写一段代码后点击右上角运行，应该不会有问题。

vscode会自己拉起控制台，然后你就能看到你代码在vscode中的控制台运行了。

# 调试代码

这部分非常坑，我搞了好久。

总的来说，我们只需要在当前目录下的.vscode文件夹下新增2个json文件就好了。

新建tasks.json文件如下

```json
{
    "tasks": [
        {
            "type": "cppbuild",
            "label": "C/C++: g++",
            "command": "/usr/bin/g++",
            "args": [
                "-g",
                "${file}",
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}"
            ],
            "options": {
                "cwd": "${fileDirname}"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "detail": "调试器生成的任务。"
        }
    ],
    "version": "2.0.0"
}
```

新建launch.json文件如下

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "preLaunchTask": "C/C++: g++",
            "type": "lldb",
            "request": "launch",
            "name": "Debug",
            "program": "${fileDirname}/${fileBasenameNoExtension}",
            "args": [],
            "cwd": "${workspaceFolder}"
        }
    ]
}
```

# 开始调试

在你要调试的位置打上断点。

3种方法来调试。

1. 快捷键：shift+command+f11
2. 找到上方的Run，选择Start Debugging
3. 在左边找到Run and Debug选项卡，点进去。可以看到里面有一个绿色的小箭头，点击，也是调试。

# References

1. MAC+VS Code+C/C++调试配置：https://blog.csdn.net/qq_37747262/article/details/81151037
2. 解决Mac上VSCdoe断点失效问题：https://blog.csdn.net/fujz123/article/details/104426121
3. vscode的 code-runner插件运行前保存：https://www.cnblogs.com/yansc/p/13895314.html
