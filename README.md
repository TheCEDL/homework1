# CEDL - Homework1 
## Paper Title
**Convolutional RNN: an Enhanced Model for Extracting Features from Sequential Data** <br>
from [arXiv:1602.05875] ( https://arxiv.org/abs/1602.05875 )

## Paper Summary
此篇論文主要提出以convolution方式對輸入的sequence data提取特徵，並結合RNN/LSTM/BLSTM新的網路架構來簡化特徵，主要實作動機為傳統CNN對於使用Window (或稱作kernel)來提取特徵時只能處理較少的sequence data，並且無法記憶過去或未來的sequence data資訊，因此希望藉由結合RNN/LSTM/BLSTM具有記憶性的架構來壓縮sequence data短時間或長時間有用的資訊，來提取更有用和簡短的特徵
 
(另外，傳統CNN會藉由使用多層convolutional layer的計算架構來提取更好的representation，然而後一層的輸入為將前一層的不同sequence data做混和可能會造成我們不想要的提取特徵結果)

| Standard Convolutional Layer | CRNN Layer |
| :---: | :---: |
|<img src=/image/1.jpg width=500 />|<img src=/image/2.jpg width=440 />|

首先在影片的sample中，對每一個sample裡的每一個不同時間點的frame取one-dimensional的特徵，如下圖所示，圖片以每一個frame提取5擺1的維度做舉例，其後會使用一定時間寬度的time Window將框選到的one-dimensional的特徵輸入到RNN或是LSTM或是BLSTM之中並每次移動兩個frame來移動time Window的位置
 
在RNN/LSTM/BLSTM的架構之中會將time Window所框選到的個別輸入輸入到RNN/LSTM/BLSTM裏面，計算出在hidden states的one-dimensional 特徵結果，對每個結果做max、mean或取以sequence的last element作為最後簡化特徵的結果，另外作者也試著在LSTM/BLSTM以output或是cell states的one-dimensional 特徵結果做一樣的處理來得到簡化解果。
 
(至於論文做了整個架構額外的延伸稱為Extended CLSTM，在一開始的LSTM參數所有參數都是一樣的並作複製，但他主要希望LSTM的不同 layer可以利用在time window每一個frame的不同位置(不同時間點)作為額外的資訊，來做不同的初始化及更新。)
 
而在實驗之中其架構主要在語音辨識的分類應用之上

## Discusion
## Participation
| Name | Do |
| :--- | :--- |
| 郭士均 | 撰寫主體 |
| 黃冠諭 | 修改內文、排版 |
| 蘇翁台 | ~ |
