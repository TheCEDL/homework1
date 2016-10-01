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

**關於 CRNN/CLSTM/CBLSTM 中的數學細節如下：**


CRNN當中的hidden states以及 outputs定義如同傳統RNN，σ採用sigmoid function。
> <img src=/image/math12.jpg width=400 />

CLSTM的版本則是將(1)式的細節改為下面的定義，<br>
input/forget/ouput gates的input當中有參考到cell state的資訊。
> <img src=/image/math34567.jpg width=500 />

CBLSTM因為要考量到順向以及逆向的hidden state資訊，<br>
所以必須將其weighted combine出輸出。
> <img src=/image/math8.jpg width=400 />

而CBLSTM作者這邊除了使用上述的hidden state作為輸出的輸入之外，<br>
也額外的作了使用cell state的版本，實驗中將比較其差異。
> <img src=/image/math9.jpg width=400 />

論文中也提出了CLSTM額外延伸架構稱為Extended CLSTM，不同之處在於，一般的LSTM使用相同之weight matrices來處理每個frame資料，而Extended CLSTM則是對不同position的frame皆採去特定weight matrices作為處理，作者認為這樣便可以加強特定層次資訊的學習。
 
而在實驗之中作者將此架構應用於語音辨識的分類上，並都得到了效能上的進步。

## Experiment Discusion
### A. Experiments on emotion classification: FAU-Aibo corpus 

FAU Aibo Emotion Corpus這個資料集主要紀錄小孩子和Sony’s pet robot Aibo互動之下的聲音，互動之中為德語發音並紀錄發音當下小孩的表情，其中共紀錄了9.2小時的語音時間，訓練集包含9959個examples並有13個男聲和13個女聲，測試集包含8257個examples並有8個男聲和17個女聲，而有關於對資料集的標示目標標籤主要分為兩種，第1種為5個標籤版本，分為: ANGER, EMPHATIC, NEUTRAL, POSITIVE, REST，第2種為2個標籤版本，分為NEGATIVE or IDLE，由於每個標籤之下的訓練資料數量不同，其會使用亂數取樣資加資料量來讓每一個類別的訓練資料數量相同。

測試結果：

> RESULTS ON EMOTION CLASSIFICATION TASK (BEST RESULTS IN BOLD)
> <img src=/image/table1.jpg width=500 />

CLSTM和Extended CLSTM的分類結果比以前再這個資料庫上最好的傳統方法和一般的CNN分類結果都還要好

### B. Experiments on age and gender classification: aGender corpus

The aGender corpus包含了個重年齡層及性別的語音資料，資料集的標示目標標籤主要分為年齡和性別兩種，第1種為CHILD, YOUTH, ADULT, SENIOR，第2種為 FEMALE, MALE and CHILD，其中訓練資料包含由471位說話的人組成32527個examples共2343個小時，測試集包含299位說化的人組成20549個examples共14.73個小時

測試結果：

> RESULTS ON GENDER CLASSIFICATION (BEST RESULTS IN BOLD)
> <img src=/image/table2.jpg width=500 />

> RESULTS ON AGE CLASSIFICATION (BEST RESULTS IN BOLD)
> <img src=/image/table3.jpg width=500 />

一樣這篇文章的分類結果比以前的傳統方法和一般的CNN分類結果都還要好，尤其是CBLSTM表現更為出色。

### C. Comparing Different CLSTM architectures

這邊作者做了不同CLSTM六種型態下之效能差異之實驗：<br>
1.較使用combine hidden/cell states作為輸出情況，<br>
2.使用mean/max/last pool方法之結果。<br>
PS： last pool 為使用最後一個time step之結果產生輸出。

DIFFERENT IMPLEMENTATIONS OF CLSTM (BEST RESULTS IN BOLD).
<img src=/image/table4.jpg width=700 />

可以從最後排平均結果看出，採用cell state資訊會比hidden state資訊較為優異，然而其中一個實驗中能然出現了相反之情形，另外max/mean/last pool方面也是相當模稜兩可，對此作者沒有多加解釋，只下了使用max可以有些微優勢之結論。


本篇提出一個CRNN model來提升特徵擷取的方法，利用額外的temporal structure以window frame-by-frame的方式輸入到recurrent layer之中，而recurrent layer的 hidden states 或 outputs states，以及在LSTM架構之下的cell states被用來提取window之中的簡化特徵，再分類的結果與傳統CNN相比來的更好。

### The unique properties of the new NN and where it can be applied to take advantage of the properties.

此篇論文提出的新的神經網路主要利用convolutional window的方法擷取feature之外，並利用有memory的RNN附加記憶能力來產生簡化特徵，因此這篇文章的特點為對傳統CNN將輸入特徵提取方法做沿用並加入記憶效應在產生特徵的過程，可以視為是將傳統CNN提取特徵的方法做改良，優點在於記憶能力可以對於輸入frame之間的前後關係去做特徵的學習。

## Participation
| Name | Do |
| :---: | :---: |
| 郭士鈞 | 撰寫主體綱要 |
| 黃冠諭 | 撰寫內容細節、排版 |
| 蘇翁台 | 撰寫內容細節 |
