## vscode服务版本
- https://vscode.dev/

## 常用皮肤插件
- Noctis
- Snazzy Operator


## 配置python-vscode
- setting.json
```json
{
"python.pythonPath": "/path/to/python"
}
```
- 配置python解释器
    
    在命令面板中，输入“Python: Select Interpreter”并选择该命令。


- launch.json
```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Anaconda (colossal)",
            "type": "python",
            "request": "launch",
            "pythonPath": "/pangu/env/anaconda3/envs/colossal/bin/python",
            "program": "${file}",
            "console": "integratedTerminal",
            "env":{},
            "cwd":"${workspaceFolder}"
        }
    ]
}

{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python:xm",
            "type": "debugpy",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "env": {
                "HERMES_HOME": "/root/data/workspace/xm",
                "xm_HOME": "/root/data/workspace/xm"
            },
            "justMyCode": false,
            "cwd": "${workspaceFolder}",
            "python": "/data/env/miniconda3/envs/env_py39_dev/bin/python",
            "args": [
                "6000"
            ]
        },
        {
            "name": "Python: Pytest",
            "type": "debugpy",
            "request": "launch",
            "module": "pytest",
            "console": "integratedTerminal",
            "env": {
                "HERMES_HOME": "/root/data/workspace/xm",
                "xm_HOME": "/root/data/workspace/xm"
            },
            "justMyCode": false,
            "python": "/data/env/miniconda3/envs/env_py39_dev/bin/python",
            "args": [
                "-s"
            ]
        }
    ]
}
```
## settings.json
```
{
    "editor.tabSize": 2,
    "editor.wordWrap": "on",
    "editor.wordWrapColumn": 80,
    "editor.fontFamily": "Consolas",
    "editor.fontWeight": "400",
    "editor.fontSize": 20,
    "editor.lineHeight": 22,
    "editor.rulers": [
        80
    ],
    "files.autoSave": "afterDelay",
    "files.autoSaveDelay": 1000,
    "git.confirmSync": false,
    "workbench.tree.indent": 20,
    "workbench.fontsize": 40,
    "window.zoomLevel": 0
}
```

##  配置Java-Vscode
- debug插件
```
Java Extension Pack
```

- log cannot be resolved
```
① install: Lombok Annotations Support for VS Code
② relead : view -> command platten -> Java：clean java language server workspace
```

- launsh.json
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "java",
            "name": "CodeLens (Launch) - App",
            "request": "launch",
            "mainClass": "org.example.App",
            "projectName": "tk-cookie"
        }
    ]
}
```
- settings.json
```json
{
"java.home": "/usr/lib/jvm/java-1.8.0", 
"maven.executable.path": "/env/apache-maven-3.8.6/bin/mvn",
// 配置maven的settings.xml
"maven.settingsFile": "/env/apache-maven-3.8.6/conf/settings.xml",
"java.configuration.maven.userSettings": "/env/apache-maven-3.8.6/conf/settings.xml",
"maven.terminal.customEnv": [
    {
      "environmentVariable": "JAVA_HOME",
      "value": "/usr/lib/jvm/java-1.8.0"
    }
  ],
"java.debug.settings.forceBuildBeforeLaunch": false,
"extensions.autoUpdate": false,
"java.dependency.packagePresentation": "hierarchical",
"java.dependency.syncWithFolderExplorer": true,
"maven.view": "flat"
}
```
- task.json
```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "package",
            "type": "shell",
            "command": "mvn package",
            "group": "build"
        },
        {
            "label": "test",
            "type": "shell",
            "command": "mvn -B test",
            "group": "test"
        }
    ]
}
```



## vscode在debug的时候无反应
- vscode 进行debug，没有任何反映，比如3.6.1的python环境安装vs安装（python=2023年就有问题，如果202204就没啥问题）IntelliSense (Pylance), Linting, Debugging

## vscode 常用的快捷键
- 折叠相关
    常用的两类场景(注意要操作的范围)：
    要操作光标所在文件中的所有代码块：
    折叠所有 Ctrl+K+0
    展开所有 Ctrl+K+J
    仅仅操作光标所处代码块内的代码：
    折叠 Ctrl+Shift+[
    展开 Ctrl+Shift+]
- 查找相关
    查找 Ctrl+F
    查找替换 Ctrl+H
    整个文件夹中查找 Ctrl+Shift+F
- 显示相关
    全屏 F11
    ZoomIn/ZoomOut Ctrl +/-
    侧边栏显/隐 Ctrl+B
    显示资源管理器 Ctrl+Shift+E
    显示搜索 Ctrl+Shift+F
    显示Git Ctrl+Shift+G
    显示Debug Ctrl+shift+D
    显示Output Ctrl+shift+U
- 重构代码
    找到所有引用：Shift+F12
    同时修改本文件中所有匹配的：Ctrl+F12
    重命名：比如要求改一个方法，可以选中后按F12，输入新的名字，回车，发现所有文件都修改了
    查看diff：在explorer里选择文件右键set file to compare，然后需要对比的文件右键选择 compare with file_name_you_chose

## vscode server mac
    macOS v11.6.5
    Installing latest from Homebrew.
        brew install code-server
    Brew release has been installed.
    Run with:
        code-server

    curl -fsSL https://code-server.dev/install.sh | sh
    ./code-server
    sudo systemctl enable --now code-server@$USER
    访问http://127.0.0.1:8080即可。
    登陆密码记录在~/.config/code-server/config.yaml

## cpp debug
- launch.json
```json
{
    //Use Intellisense to learn about possible attributes.
    //Hover to view descriptions of existing attributes.
    //For more information,visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version":"0.2.0",
    "configurations":[
        {
            "name":"main : makefile build and debug",    //name随便取，用来在定义了多个configuration的时候区分执行的是哪一套参数。
            "type":"cppdbg",        //这个一般就不要改了，这个我还不了解其他的参数配置
            "request":"launch",    //这个一般就不要改了，这个我还不了解其他的参数配置
            "program":"${workspaceFolder}/build/main",    //执行这个文件，相当于命令行中输入这行
            "args":[],    //program的参数，相当于命令行中在program后输入用空格隔开的这些参数。我这里前两个是执行文件自带的参数设置，后三个是将标准输出和标准错误输入到文件out中。
            "stopAtEntry":false,    //大概是表示是否在main函数处停下，类似在main上打断点。
            "cwd":"${workspaceFolder}",    //表示在哪个目录下执行program，相当于命令行中输入cd 该行
            "environment":[],          //不知道干嘛的
            "externalConsole":false,    //如果使用控制台则需要打开这个。我这边加了这句后会有不知道在使用哪个控制台的问题，反正会很奇怪，就注释掉了。
            "MIMode":"gdb",            //使用哪个工具作为调试工具，只有两个选择gdb或lldb。lldb我不了解，和我一样不了解的人就写gdb吧。
            "setupCommands":[    //gdb相关的一些设置命令，相当于在执行gdb后，对gdb下命令行的设置。gdb相关的初始化配置，要在~/.gdbinit文件中配置，或者在这配置，我另开一篇文章写，涉及下面enable-pretty-printing的，不一定开了就有用了，还涉及其他的问题，这里先放个TODO。
                {
                    "description":"for gdb start all print",    //给人看的描述
                    "text":"-enable-pretty-printing",        //gdb下执行的命令，启用整齐打印，就是让gdb打印的东西好看一点。
                    "ignoreFailures":true        //并不清楚这个实际用来干嘛
                }
            ],
            "preLaunchTask": "Build",
            //"preLaunchTask":"C/C++ g++ build active file",    //执行program前需要执行的task，这里需要和task.json中要执行的task的label一致
            "miDebuggerPath": "/usr/bin/gdb" 
        }
    ]
}
```
- tasks.json
```json
{   
    "version": "2.0.0",
    "options": {
        "cwd": "${workspaceFolder}/build"
    },
    "tasks": [
        {
            "type": "shell",
            "label": "cmake",
            "command": "cmake",
            "args": [
                ".."
            ]
        },
        {
            "label": "make",
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "command": "make",
            "args": [

            ]
        },
        {
            "label": "Build",
			"dependsOrder": "sequence",
            "dependsOn":[
                "cmake",
                "make"
            ]
        }
    ]

}
```
