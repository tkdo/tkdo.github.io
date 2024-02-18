## python好用的类库
- pyttsx3
    ```python
    import pyttsx3
    engine = pyttsx3.init()
    engin.say("轻轻地，我走了，正如我轻轻地来....")
    engin.runAndWait()
    ```
## gym

- install
```shell
conda install -n env-py37-t15 -c conda-forge gym==0.25
```

## uvicorn
- 管理
    ```shell
    uvicorn api1:app --reload --workers 4 --host 0.0.0.0 --port 8000
    kill -TERM pid_of_uvicorn_server
    ```