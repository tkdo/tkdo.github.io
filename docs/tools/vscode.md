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
            "name": "CodeLens (Launch) - RecallApplication",
            "request": "launch",
            "mainClass": "com.xx.RecallApplication",
            "projectName": "xx-proj"
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
"maven.settingsFile": "env/apache-maven-3.8.6/conf/settings.xml",
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
"maven.view": "hierarchical",
// "java.externalDependencies": [
//       "junit:junit:jar:4.12:test", // Maven format
//       "junit:junit:4.12" // Gradle-style format is also allowed
//   ]
"java.jdt.ls.java.home": "/usr/lib/jvm/java-1.8.0",
"java.configuration.runtimes": [
  {
    "name": "JavaSE-1.8",
    "path": "/usr/lib/jvm/java-1.8.0"
  },
  {
    "name": "JavaSE-11",
    "path": "/usr/local/jdk-11.0.3",
    "sources" : "/usr/local/jdk-11.0.3/lib/src.zip",
    "javadoc" : "https://docs.oracle.com/en/java/javase/11/docs/api",
    "default":  true
   },
   {
    "name": "JavaSE-12",
    "path": "/usr/local/jdk-12.0.2"
   },
   {
    "name": "JavaSE-13",
    "path": "/usr/local/jdk-13"
   }
],
"spellright.notificationClass": "hint",
"spellright.spellContext": "body comments",
"java.trace.server": "verbose",
"java.configuration.updateBuildConfiguration": "automatic"
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
