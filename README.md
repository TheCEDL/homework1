# CEDL Homework1 - Introduce a New NN with Memory 

## Paper Title
**Gated Feedback Recurrent Neural Networks** <br>
https://arxiv.org/pdf/1502.02367v4.pdf

## Paper Summary
Recurrent Neural Networks (RNN) 已經被廣泛運用在sequence model相關的研究上，特別是輸入、輸出長短不一的任務，但隨著資料長度變長，RNN並不太能處理long-term dependency的問題，因此發展出gated activation function: Long Short-Term Memory (LSTM) 和 Gated Recurrent Unit (GRU)，可以處理long-term dependency和short-term dependency等問題。

本篇論文為RNN發展出新的設計Gated-feedback RNN (GF-RNN)，使用在stack RNN上面可以連接前一個時間點的N個hidden state而不是只有1個，也就是可以讓上層的資訊也能回留給下層，因此每個Unit則會得到前一個時間點所有層的Unit的資訊，如下圖所示。
	
  <img src=/image/1.png width=700 />
  
GF-RNN也新增了global reset gate來控制全連接的強度，不只有前一時間點的一個state會影響global reset gate，而是前一時間許多層的statet可以影響global reset gate，其數學式為

  <img src=/image/2.png width=500 />
  
其中將global reset gate加入stacked RNN (LSTM、GRU)的方法分別如下

  <img src=/image/3.png width=700 />
  <img src=/image/4.png width=700 />
  <img src=/image/5.png width=700 />

## Experiment Discusion
### 1. LANGUAGE MODELING
使用在language model上面的話，作者會比tanh、GRU以及LSTM分別作用在參數量相近Single、Stacked、Gated Feedback上面的結果，Gated Feedback L是指取和Stacked相同的層數和Unit數，所以參數量比較多。

  <img src=/image/6.png width=700 />

從圖中可以看出，應用GF-RNN後，即使參數量比較多，收斂速度並沒有比較慢。

  <img src=/image/7.png width=700 />
  
除了tanh外，LSTM和GRU效果都有進步 

  <img src=/image/8.png width=700 />
  
另外，作者也比較是否使用了global reset gate真的有幫助，發現LSTM的部分未使用時的BPC為1.854(圖表中階有使用global reset gate)，雖然提升的幅度似乎不大。


  
### 2. PYTHON PROGRAM EVALUATION
  
給最左邊的程式作為Seed，然後預測輸出接下來的部分，GF-RNN在這部分也輸出了正確的程式語法。

  <img src=/image/9.png width=700 />
  
為求得更好的精確度，這裡使用了更多的層(五層)，每一層皆有700個LSTM Units，效果比之前的更佳。
 
  <img src=/image/10.png width=700 />
  
使用熱度圖來看，發現當nesting與target length越大的時候accuray會逐漸下降，而圖(6-c)比較了使用gated feedback前後的正確率差距，也明顯看到使用gated feedbacck後正確率明顯提高了。

  <img src=/image/11.png width=700 />
  

### The unique properties of the new NN and where it can be applied to take advantage of the properties.


  
  
  
  
  
  
  
  在language modeling以及python program evaluation的實驗結果裡證明了改良後的GF-RNN可以獲得較佳的成果。
	





