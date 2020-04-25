## 对VS code的C/C++环境进行配置
---
首先我们要有一个mingw,可以自行下载,解压到一个目录后,在我的电脑上右击->属性->高级系统设置->环境变量->系统变量中的Path->编辑->新建->输入解压后的mingw文件夹中bin的目录,如A:\XXXX\mingw64\bin

可在dos中用gcc -v查询是否添加成功

打开VS,打开一个文件夹及文件,在调试页面选择"create a launch.json file",弹出的地方选择第一个C++(GDB/LLDB)和g++.exe(测试文件是cpp类型才有该选项,如果是C文件,仅有gcc,文本内容在后面)

对于生成的launch.json中的代码我们用以下代码进行替换,如果您使用的Vscode版本过老，以下文件中的``"${workspaceFolder}"``(带上引号)可能需要替换为"${workspaceRoot}"
```
{
   "version": "0.2.0",
   "configurations": [
       {
           "name": "(gdb) Launch", // 配置名称，将会在启动配置的下拉菜单中显示
           "type": "cppdbg", // 配置类型，这里只能为cppdbg
           "request": "launch", // 请求配置类型，可以为launch（启动）或attach（附加）
           "program": "${workspaceFolder}/${fileBasenameNoExtension}.exe", // 将要进行调试的程序的路径
           "args": [], // 程序调试时传递给程序的命令行参数，一般设为空即可
           "stopAtEntry": false, // 设为true时程序将暂停在程序入口处，一般设置为false
           "cwd": "${workspaceFolder}", // 调试程序时的工作目录，一般为${workspaceRoot}即代码所在目录 workspaceRoot已被弃用，现改为workspaceFolder
           "environment": [],
           "externalConsole": true, // 调试时是否显示控制台窗口，一般设置为true显示控制台
           "MIMode": "gdb",
           "miDebuggerPath": "C:/Program Files (x86)/mingw64/bin/gdb.exe", // miDebugger的路径，注意这里要与MinGw的路径对应
           "preLaunchTask": "g++", // 调试会话开始前执行的任务，一般为编译程序，c++为g++, c为gcc
           "setupCommands": [
               {
                   "description": "Enable pretty-printing for gdb",
                   "text": "-enable-pretty-printing",
                   "ignoreFailures": false
               }
           ]
       }
   ]
}
```

miDebuggerPath后的值取自己解压mingw路径中bin文件下的gdb.exe的路径

保存后按F5运行

提示"Could not find the task 'g++' " 

选择配置任务,之后选择g++,用以下代码对tasks.json文件中代码进行替换

```

{
    "version": "2.0.0",
    "command": "g++",
    "args": ["-g","${file}","-o","${fileBasenameNoExtension}.exe"],    // 编译命令参数
    "problemMatcher": {
        "owner": "cpp",
        "fileLocation": ["relative", "${workspaceFolder}"],
        "pattern": {
            "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
            "file": 1,
            "line": 2,
            "column": 3,
            "severity": 4,
            "message": 5
        }
    }
}


```


然后按F5有黑框闪过就说明配置成功了,可以下断点或用getchar()语句使运行暂停.

上面的两个文件内容适用于C++,C可以运行,但是不能进行调试,如果要调试C文件,操作与上述内用基本相同,不过除第一步的选择C++,其余部分都选gcc,同时上述两个文件有所不同,如下:


launch.json文件
```
{
    "version": "0.2.0",
    "configurations": [

        {
            "name": "(gdb)c_launch",
            "type": "cppdbg",
            "request": "launch",
            "targetArchitecture": "x64",
            "program": "${workspaceFolder}/${fileBasenameNoExtension}.exe",
            "miDebuggerPath": "E:/VS_code/mingw/mingw64/bin/gdb.exe",//自己的gdb.exe目录
            "MIMode": "gdb",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceRoot}",
            "environment": [],
            "internalConsoleOptions": "openOnFirstSessionStart",
            "externalConsole": true,
            "preLaunchTask": "gcc"
        }
    ]
}
```


tesks.json文件
```
{
    "version": "2.0.0",
    "command": "gcc",
    "args": ["-Wall", "-g", "${file}", "-o", "${fileBasenameNoExtension}.exe"],
    "echoCommand": true,
    "problemMatcher": {
        "owner": "cpp",
        "fileLocation": ["relative", "${workspaceFolder}"],
        "pattern": {
            "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
            "file": 1,
            "line": 2,
            "column": 3,
            "severity": 4,
            "message": 5
        }
    }
}
```

最后还有一个c_cpp_properties.json文件,如果上述两个文件配置完成没有提示找不到头文件,这个文件不要也罢
```
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}"
               
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "compilerPath": "D:/Program Files/mingw64/bin/gcc.exe",
            "intelliSenseMode": "msvc-x64",
            "browse": {
                "path": [
                    "${workspaceRoot}"     
                ],
                "limitSymbolsToIncludedHeaders": true,
                "databaseFilename": ""
            }
        }
    ],
    "version": 4
}
```