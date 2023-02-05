## pandas
### 指定列的名称
```python
data = pd.read_csv(file_path, 
                    sep=",", 
                    names=["Sepal_Length", "Sepal_Width",
                        "Petal_Length", "Petal_Width", "label"])
```
### 替换列的值
```python
data["label"].replace("Iris-setosa", 0, inplace=True)
```

## python好用的类库
- pyttsx3
    ```python
    import pyttsx3
    engine = pyttsx3.init()
    engin.say("轻轻地，我走了，正如我轻轻地来....")
    engin.runAndWait()
    ```

