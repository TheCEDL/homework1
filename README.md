## Homework1 - Introduce a New NN with Memory 
Please complete each homework for each team, and <br>
mention who contributed which parts in your report.

## Member
* captain - <a href="https://github.com/maolin23?tab=repositories">林姿伶</a>: 104062546
* member -  <a href="https://github.com/hedywang73?tab=repositories">汪叔慧</a>: 104062526
* member - <a href="https://github.com/yenmincheng0708?tab=repositories">嚴敏誠</a>: 104062595

`Finish this report all together.`<br>
```
敏誠 typed first version of this report, 姿伶 and 叔慧 add some detail to finish this report.
Finally, 姿伶 tyoed the final report on github.
```

## Paper title
<a href="https://arxiv.org/pdf/1508.03790v4.pdf">Depth-Gated LSTM</a>

## Brief introduction
　Due to the ubiquity of deep learning, it has been applied into many kinds of fields, such as computer vision and natural language processing, and also has been designed into different architectures. For example, RNNs have been proposed to handle sequential data. This kind of architecture suffers from the gradient vanishing. So LSTM has been proposed to prevent the gradient vanishing from recursive networks. However, the problem of gradient explosion still exists when training deeper networks. So this paper proposes a modified version of the LSTM called **Depth-Gated LSTM(DGLSTM)** to alleviate the problem of gradient explosion in deeper networks. Traditional stacked LSTM layers communicate to each layers through the input and output which means there is no information sharing between memory cells (Fig. 1(a)). Therefore, the author introduces a new depth gate which connects the memory cell between layers (Fig. 1(b)). By doing so, different layers can share the information of the content of the memory cell. In the experiment, the author applied **DGLSTM** on Machine translation and Language modeling and the result indeed showed the improvement.

　　　　![Fig. 1](https://github.com/maolin23/homework1-1/blob/master/Fig_1.JPG)<br>
　　　　　　　　　　　　　　　　**Fig .1** The architecture of two different versions of LSTM

## Unique properties
　Although the problem of gradient explosion from training simple RNNs has been alleviated by LSTM, GRU…etc., the problem of gradient explosion still exists when training deeper networks. So the authors designed a gate called depth-gate between different layers to alleviate the problem of gradient explosion when training deeper network. Combine this gate with LSTM, the DGLSTM can also alleviate the problem of gradient explosion from recursive and deeper neural network.
 
1.	The author introduces a depth-gate function to connect the memory cells between each layers which is different from the traditional LSTM.
2.	The depth-gate can be thought of as the peephole which can take advantage of the low-level information.
3.	The DGLSTM can prevent the gradient explosion in deeper networks.

## Application
`The input of these tasks are sequential data which have the temporal relation.`
* Machine translation <br>
* Language modeling<br>
* Emotion recognition<br>
* Image captioning <br>

`The input are images which have spatial relation`
* Scene parsing   <br>

