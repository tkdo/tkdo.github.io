### ① 安装Jupyterlab库
```bash
pip install jupyterlab
```
### ② 生成配置文件
```bash
jupyter lab --generate-config
```
### ③ 设置密码
描述：设置密码用于设置服务器配置，以及登录Jupyter。打开Python终端，输入以下：
```python
>>> from notebook.auth import passwd  
>>> passwd()
Enter password: 
Verify password: # 此时会让你两次输入密码，然后就会生成秘钥  
'sha1:5f84cd442b12:707f6ca12c9cfb622f7e79baf74e21fb2c542498'
```
### ④ 设置服务器配置文件
描述：修改配置，vim ~/.jupyter/jupyter_lab_config.py
```
c.NotebookApp.allow_remote_access = True #允许远程连接
c.NotebookApp.ip='*'                     # 设置所有ip皆可访问
c.NotebookApp.password = '＊＊＊＊＊＊＊＊＊＊＊＊'  # 上面的密钥'
c.NotebookApp.open_browser = False       # 禁止自动打开浏览器
c.NotebookApp.port = 8888                 # 设置打开端口,随便设置,保证不冲突即可
c.NotebookApp.notebook_dir = '/home/users/jupyterlab_projects'  #设置Notebook启动进入的目录
```
