# CEDL Homework1 - Introduce a New NN with Memory 

## Paper Title
**Gated Feedback Recurrent Neural Networks** <br>
https://arxiv.org/pdf/1502.02367v4.pdf

## Paper Summary
Recurrent Neural Networks (RNN) 已經被廣泛運用在sequence model相關的研究上，特別是輸入、輸出長短不一的任務，但隨著資料長度變長，RNN並不太能處理long-term dependency的問題，因此發展出gated activation function: Long Short-Term Memory (LSTM) 和 Gated Recurrent Unit (GRU)，可以處理long-term dependency和short-term dependency等問題。

本篇論文為RNN發展出新的設計Gated-feedback RNN (GF-RNN)，使用在stack RNN上面可以連接前一個時間點的N個hidden state而不是只有1個，也就是可以讓上層的資訊也能回留給下層，因此每個Unit則會得到前一個時間點所有層的Unit的資訊，如下圖所示。
	
  <img src=/image/1.png width=800 />
  
GF-RNN也新增了global reset gate來控制全連接的強度，不只有前一時間點的一個state會影響global reset gate，而是前一時間許多層的statet可以影響global reset gate，其數學式為

  <img src=/image/2.png width=500 />
  
其中將global reset gate加入stacked RNN (LSTM、GRU)的方法分別如下

  <img src=/image/3.png width=500 />




  
  
  
  
  
  
  
  在language modeling以及python program evaluation的實驗結果裡證明了改良後的GF-RNN可以獲得較佳的成果。
	





