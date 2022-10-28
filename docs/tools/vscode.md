## vscode服务版本
- https://vscode.dev/

## 常用皮肤插件
- Shades of Purple
- Noctis
## 配置launch.json
```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "justMyCode": true,
            "cwd": "${workspaceFolder}",
            "args":["--config_mode", "file",  "--run_mode", "cluster"]
        }
    ]
}
```


##  配置Java-Vscode
- log cannot be resolved
```
1) install: Lombok Annotations Support for VS Code
2) relead : view -> command platten -> Java：clean java language server workspace
```

