## java环境变量
```bash
export JAVA_HOME=/usr/lib/jvm/java-1.8.0
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

## arraycopy
```java
    List<float[]> embeddings = new ArrayList<float[]>();
    float cur = 0;
    for(int i=0;i < 3; i++){
        float[] tmp =new float[10];
        for(int j=0; j<10; j++){
            cur = cur + 1;
            tmp[j] = cur;
        }
        embeddings.add(tmp);
    }
    System.out.println(embeddings);

    int dim = embeddings.get(0).length;
    int num = embeddings.size();
    float[] vector =new float[num * dim];
    for (int i=0; i< embeddings.size(); i++) {
        System.arraycopy(embeddings.get(i), 0, vector, i * userEmbDim, userEmbDim);
    }
    System.out.println(userMultiVector);
```
