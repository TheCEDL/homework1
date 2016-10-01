# CEDL Homework1 - Introduce a New NN with Memory 

## Paper Title
**Gated Feedback Recurrent Neural Networks** <br>
https://arxiv.org/pdf/1502.02367v4.pdf

## Paper Summary
Recurrent Neural Networks (RNN) 已經被廣泛運用在sequence model相關的研究上，特別是輸入、輸出長短不一的任務，但隨著資料長度變長，RNN並不太能處理long-term dependency的問題，因此發展出gated activation function: Long Short-Term Memory (LSTM) 和 Gated Recurrent Unit (GRU)，可以處理long-term dependency和short-term dependency等問題。

本篇論文為RNN發展出新的設計Gated-feedback RNN (GF-RNN)，使用在stack RNN上面可以連接前一個時間點的N個hidden state而不是只有1個，也就是可以讓上層的資訊也能回留給下層，因此每個Unit則會得到前一個時間點所有層的Unit的資訊，如下圖所示。
	
  <img src=/image/1.png width=700 />
  
  
GF-RNN也新增了global reset gate來控制全連接的強度，不只有前一時間點的一個state會影響global reset gate，而是前一時間許多層的statet可以影響global reset gate，其數學式為

  <img src=/image/2.png width=400 />
  
其中將global reset gate加入stacked RNN (LSTM、GRU)的方法分別如下

  <img src=/image/3.png width=700 />
  <img src=/image/4.png width=700 />
  <img src=/image/5.png width=700 />

## Experiment Discusion
### 1. Language Modeling
使用在language model上面的話，作者會比tanh、GRU以及LSTM分別作用在參數量相近Single、Stacked、Gated Feedback上面的結果，Gated Feedback L是指取和Stacked相同的層數和Unit數，所以參數量比較多。

  <img src=/image/6.png width=400 />

從圖中可以看出，應用GF-RNN後，即使參數量比較多，收斂速度並沒有比較慢。

  <img src=/image/7.png width=700 />
  
除了tanh外，LSTM和GRU效果都有進步 

  <img src=/image/8.png width=500 />
  
另外，作者也比較是否使用了global reset gate真的有幫助，發現LSTM的部分未使用時的BPC為1.854(圖表中階有使用global reset gate)，雖然提升的幅度似乎不大。


  
### 2. Python Program Evaluation
  
給最左邊的程式作為Seed，然後預測輸出接下來的部分，GF-RNN在這部分也輸出了正確的程式語法。

  <img src=/image/9.png width=500 />
  
為求得更好的精確度，這裡使用了更多的層(五層)，每一層皆有700個LSTM Units，效果比之前的更佳。
 
  <img src=/image/10.png width=500 />
  
使用熱度圖來看，發現當nesting與target length越大的時候accuray會逐漸下降，而圖(6-c)比較了使用gated feedback前後的正確率差距，也明顯看到使用gated feedbacck後正確率明顯提高了。

  <img src=/image/11.png width=700 />
  


### The unique properties of the new NN and where it can be applied to take advantage of the properties.

Gated feedback使用在Stacked LSTM以及GRU上面，的確提高了正確率，讓結果變得更好。global reset gate則是控制前層資訊的強度，因為這些資訊對於下個時間點可能無關或影響很大，所以加入gate來限制他們，經由作者的實驗也可知，使用了global reset gate比沒使用時效果更好。另外，加入了全連接後，參數量勢必大增，但實驗也顯示，應用了GF-RNN的結果和一般堆疊的GRU和LSTM比較下，相近參數的架構，及相同層數而且相同Unit數的架構，其收斂速度並沒有比較慢，甚至結果還更好，由此可知GF-RNN雖然參數量多，但訓練的過程快，代表參考了更多的資訊後，參數更新的方向可能也比較明確。

本篇論文本身即是提出一個很容易應用及加入的方法。一些像是QA方面的work，在問題也就是句子部分的feature，有時候使用CNN的表現比LSTM好，或許應用了堆疊的LSTM，並全連接不同時間的Unit後，能取出更好的特徵，也就是本篇論文的做法，在處理language model的問題或其他序列性的資料的問題時，能帶來更好的效果。目前deep learning深受世人的重視，而有蓬勃的發展，並持續改良現有的架構，試著讓deep learning更接近人腦，也期望未來會有更多良好的架構與機制被發展出來，讓我們真正迎向AI的未來。



## Participation
| Name | Do |
| :---: | :---: |
| 張哲嘉 | 尋找論文、簡介、合併|
| 林暘竣 | 尋找論文、特點、排版|

  
  
	





