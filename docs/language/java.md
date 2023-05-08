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

```java
    /**
     *
     * @param recallContext
     * @param i2IValuesRecall
     * @param expectTextResCnt
     * @param expectVideoResCnt
     * @param isDebug
     * @return
     */
    public List<Item> scoreFusion(RecallContext recallContext, Map<String, List<Item>> i2IValuesRecall,
                                  int expectTextResCnt, int expectVideoResCnt, boolean isDebug) {
        List<Item> result = new ArrayList<>();
        List<Item> resText = new ArrayList<>();
        List<Item> resVideo = new ArrayList<>();
        for(int i=0; i<expectTextResCnt + expectVideoResCnt; i++){
            for(String keyNewsId : i2IValuesRecall.keySet()){
                List<Item> i2iValues = i2IValuesRecall.get(keyNewsId);
                List<Item> i2iTexts = new ArrayList<>();
                List<Item> i2iVideos = new ArrayList<>();
                for(int j=0; j<i2iValues.size(); j++){
                    Item item = i2iValues.get(j);
                    if("0".equals(item.getType())) i2iTexts.add(item);
                    else i2iVideos.add(item);
                }
                if(resText.size() >= expectTextResCnt  && resVideo.size() >= expectVideoResCnt) break;
                if(i < i2iTexts.size() && resText.size() < expectTextResCnt)
                    resText.add(i2iTexts.get(i));
                if(i< i2iVideos.size() && resVideo.size() < expectVideoResCnt)
                    resVideo.add(i2iVideos.get(i));
            }
        }
        result.addAll(resText);
        result.addAll(resVideo);
        return result;
    }
    return result;
    }
```