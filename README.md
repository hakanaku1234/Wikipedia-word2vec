## 維基百科詞向量model

* 語料來源：<a herf="https://zh.wikipedia.org/wiki/Wikipedia:%E6%95%B0%E6%8D%AE%E5%BA%93%E4%B8%8B%E8%BD%BD">維基百科資料庫</a>
* 使用相關庫：
  * word2vec套件：<a href="https://github.com/RaRe-Technologies/gensim">gensim</a>
  * 斷詞套件：<a href="https://github.com/fxsjy/jieba">結巴中文分詞</a>, <a href="https://github.com/ldkrsi/jieba-zh_TW">結巴_TW</a>
* 若github無法開啟ipython notebook，請使用<a href="http://nbviewer.jupyter.org/">nbviewer</a>




## 簡體翻繁體

使用<a href="https://github.com/BYVoid/OpenCC">OpenCC</a>將簡體文字轉成繁體文字：

```
opencc -i wiki_text.txt -o zh_wiki_text.txt -c s2tw.json
wiki_text.txt -> zh_wiki_text.txt
```



## gensim word2vec參數

```python
class gensim.models.word2vec.Word2Vec(sentences=None, size=100, alpha=0.025, window=5, min_count=5, max_vocab_size=None, sample=0.001, seed=1, workers=3, min_alpha=0.0001, sg=0, hs=0, negative=5, cbow_mean=1, hashfxn=<built-in function hash>, iter=5, null_word=0, trim_rule=None, sorted_vocab=1, batch_words=10000)
```

* sentence：語料訓練集
* size：訓練出的詞向量（特徵向量）維度
* alpha：學習速率
* window：當前詞與預測詞在一個句子中的最大距離
* sg：設置訓練演算法，0=CBOW，1=skip-grim
* min_count：詞頻少於min_count的數會被丟掉
* workers：執行緒數目
* negative：若>0，則會採用Negative Sampling（負採樣）



## Load Model

```python
from gensim.models import word2vec
model = word2vec.Word2Vec.load("model/20180309wiki_model.bin")
```



## gensim model方法

1. 尋找相似詞

   ```python
   query = input()

   print('相似詞前十排序')
   res = model.most_similar(query, topn=10)

   for item in res:
       print(item[0] +','+ str(item[1]))
   ```

2. 計算兩個詞的餘弦相似度（cosine）

   ```python
   query = input()
   q_list = query.split()

   print('計算 Cosine 相似度 ')
   res = model.similarity(q_list[0], q_list[1])
   print(res)
   ```

3. 輸入三個詞，做類比推理

   ```python
   query = input()
   q_list = query.split()

   print("%s之於%s，如%s之於" % (q_list[0], q_list[2], q_list[1]))
   res = model.most_similar([q_list[0], q_list[1]], [q_list[2]], topn= 100)
   for item in res:
       print(item[0] +","+ str(item[1]))
   ```



## 訓練好的model下載

20171201版本（使用jieba）：<a href="https://drive.google.com/file/d/1WEHTrBbOnmdUyTz-Xy1yqlFvkLQuir8J/view?usp=sharing">Google Drive</a>

20180309版本（jieba_tw + stopwords）：<a href="https://drive.google.com/file/d/12XJYBSgiqRQznDigr2O360RCzntJ-Uif/view?usp=sharing">Google Drive</a>
