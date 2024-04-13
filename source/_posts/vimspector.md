---
title: vimspector
date: 2023-03-25 11:17:31
categories:
- 工具
tags:
- vim
- debugger
---


### 安装

1. 使用vim插件管理器plug.vim安装，添加 `Plug 'puremourning/vimspector'`
2. 安装debug adapters（gadgets），实际在后台工作的程序，vimspector只是一个前台交互wrapper
3. 配置项目的debug配置文件 `.vimspector.json` 

>[!NOTE]
>
>步骤二中需要设置在terminal中操作，因为需要代理网络访问。
>- $env:HTTPS_PROXY = "xxxx"   [设置环境变量HTTPS_PROXY,用于python网络接口]
>- python .\install_gadget.py --enable-c --enable-python


![](/images/adaptor.png)

可以看到安装adaptor有两种方式，一种通过vim中的命令（不推荐）会由于网络导致安装失败，并且该失败会导致后续通过脚本成功安装之后检测不到成功安装。另一种是通过 `install_gadget.py` 脚本来安装adapter（推荐）


### 配置

官网给出了一些简单的vimspector.json的配置：[puremourning/vimspector: vimspector - A multi-language debugging system for Vim (github.com)](https://github.com/puremourning/vimspector#debug-profile-configuration)

配置文件中具体的手册参考文档：[Configuration | Vimspector Documentation (puremourning.github.io)](https://puremourning.github.io/vimspector/configuration.html)

```json
//vimspector.json中可以有配置，且vimspector官方对该json配置做了一些扩展支持符号扩展等特性

{                                                                                                      
    "configurations": {
        "cpp_launch": {
            "adapter": "CodeLLDB",
            "configuration": {
                "name": "cpp:launch",
                "stopAtEntry": true,
                "type": "cppdbg",
                "request": "launch",
                "program": "${workspaceRoot}/ninja_build/out/decoder.exe",
				"args": [ "*${CommandLineArgs}" ],
                "cwd": "${workspaceRoot}/ninja_build/out",
                "environment": [], 
                "externalConsole": true,
                "MIMode": "gdb"
           }   
        } 
		"python_Launch": {
			"adapter": "debugpy",
			"configuration": {
				"name": "python3: Launch",
				"type": "python",
				"request": "launch",
				"cwd": "${workspaceRoot}",
				"stopOnEntry": true,
				"console": "externalTerminal",
				"debugOptions": [],
				"program": "${script:${file\\}}"
			}
		}


   }   
}

```

### 使用

1. 首先是快捷键映射，官方已经有两套默认配置，只需设置全局变量 `let g:vimspector_enable_mappings = 'VISUAL_STUDIO'` 或 `let g:vimspector_enable_mappings = 'HUMAN'` 即可使用。所有可用映射参见官网[puremourning/vimspector: vimspector - A multi-language debugging system for Vim (github.com)](https://github.com/puremourning/vimspector#mappings)

2. vim界面上可以点击（winbar），不需要自己重写打乱自己的映射