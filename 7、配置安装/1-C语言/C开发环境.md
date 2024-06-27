## 1、<span style="color:brown">前置准备：</span>

**1.1、Vscode**

在VsCode官网下载安装包，安装至本地，之后安装各类插件：

- **“C/C++ Extension Pack”插件**
- **“code runner”插件** 

**1.2、minGW64**

​	在官网下载：https://sourceforge.net/projects/mingw-w64/files/，在`MinGW-W64 GCC-8.1.0`大类下选择`x86_64-win32-sjlj`，之后在<u>**D盘或其他盘符**</u>解压该文件。解压后，需要**配置系统环境变量**，将安装路径：`D:\Ming64\mingw64\bin`，配置到`Path`中。之后在`CMD`中，执行：

- gcc -v

- gcc --version

查看是否安装成功，若成功安装，则会显示GCC的版本信息！！！



## 2、<span style="color:brown">文件配置：</span>

**2.1、.vscode创建**

**<u>方式一：</u>**

​	直接在你打开的文件夹目录下，创建一个**`.vscode`文件**；

**<u>方式二：</u>**

​	点击`Terminal(终端)`，选择`Configuration Tasks(配置任务)`，然后再导航栏中选择<u>**使用模板创建`tasks.json`文件**</u>，再选择`Others`，就能创建一个<u>自带`tasks.json`的`.vscode`文件</u>；

**2.2、launch.json文件**

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch", 
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/${fileBasenameNoExtension}.exe",
            "args": [], 
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": true, 
            "MIMode": "gdb",
            "miDebuggerPath": "C:\\mingw64\\bin\\gdb.exe", // 修改成自己的路径
 
    //**********上面这行，要修改为你的编译器所在的路径，形如 c:\\***\\bin\\gdb.exe
 
            "preLaunchTask": "gc", //这里注意一下这个名字一会儿还要用到
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

**2.3、tasks.json文件**

```json
{
    "version": "2.0.0",
    "command": "gc",  //这里的名字 注意一下
    "args": [
        "-g",
        "${file}",
        "-o",
        "${fileBasenameNoExtension}.exe"
    ], // 编译命令参数
    "problemMatcher": {
        "owner": "cpp",
        "fileLocation": [
            "relative",
            "${workspaceFolder}"
        ],
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

