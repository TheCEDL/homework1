# CEDL - Homework1 
Introduce a New NN with Memory
## Paper Title
**Convolutional RNN: an Enhanced Model for Extracting Features from Sequential Data** <br>
from [arXiv:1602.05875] ( https://arxiv.org/abs/1602.05875 )

## Paper Summary
此篇論文提出了 Convolutional RNN 的方式，用於提取批次的 sequence data 特徵之應用，主要實作動機為傳統 CNN 作法是利用 affine function 接上non-linearity 來提取特徵（ 下方表格，左 ），而這樣的處理較為簡易，無法駕馭sequence data中過去或未來之時域資訊，因此作者希望利用 RNN/ LSTM/ BLSTM 來強化其網路架構（ 下方表格，左 ），如此能夠提取出較傳統CNN作法有價值的特徵。

>以下示意圖將每個frame資料視為一維向量，而每次處理完window（圖例window size為五個frame）<br>中的frame後，stride指定frame數並產生對應的一維向量輸出 

| Standard Convolutional Layer | CRNN Layer |
| :---: | :---: |
|<img src=/image/1.jpg width=500 />|<img src=/image/2.jpg width=440 />|
|利用多個weight matrix對每個window作element-wise的乘法，<br>然後將輸出之個別矩陣的所有element加總，最後將個別的結果<br>combine起來加上bias產生輸出。|將window之中每個frame資料，分別丟入RNN最為輸<br>入，最後再經過不同的pooling機制，產生出對應輸出<br>PS： 將RNN改為LSTM/BLSTM即為CLSTM/CBLSTM。|

關於CLSTM及CBLSTM中的數學細節如下：


論文中也提出了CLSTM額外延伸架構稱為Extended CLSTM，不同之處在於，一般的LSTM使用相同之weight matrices來處理每個frame資料，而Extended CLSTM則是對不同position的frame皆採去特定weight matrices作為處理，作者認為這樣便可以加強特定層次資訊的學習。
 
而在實驗之中作者將此架構應用於語音辨識的分類上，並都得到了效能上的進步。

## Discusion


## Participation
| Name | Do |
| :---: | :---: |
| 郭士鈞 | 撰寫主體 |
| 黃冠諭 | 撰寫內容細節、排版 |
| 蘇翁台 | 撰寫內容細節 |
