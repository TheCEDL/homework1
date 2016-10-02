# Grid Long Short-Term Memory
Nal Kalchbrenner & Ivo Danihelka & Alex Graves, from Google DeepMind

## Overview
- Introduction
  - Motivation
  - Architecture
  - Experiment
- Discussion
- Question

## Introduction
Grid-LSTM is proposed in ICLR 2016 from google deepmind. One of the author, Alex Graves, is from University of Toronto and is profecient in recurrent neural network. Grid-LSTM has received multiple success in number recognition, mathmetical algorithm, and memorization. Okay, after some brief introduction, let's dive into the GridLSTM:   

Grid LSTM is a variant of LSTM cell, so it's good for us to revisit the LSTM cell first:   
Vanilla RNN is known to suffer gradient vanishing promblem and LSTM is one of the RNN cell that created to mitigate this problem. LSTM cantains content, updata, forget, output gates, which is formed with some non-linear functions, such as sigmoid and tanh. **Capturing long-term memory** is the most significant property for LSTM.   
The following figure is the illustration of LSTM, from [colah's blog](http://colah.github.io/posts/2015-08-Understanding-LSTMs/):   
![](https://github.com/andrewliao11/homework1/blob/master/lstm.png?raw=true)   
Why it can capture or retain the memory? The point is the **gate** mechanism. From the formula of LSTM, it's clear that it let the LSTM cell learn to retain/forget the memory and the output gate control how much the the memory will contribut the output signal. The following figure is the illustration of LSTM cell:
![](https://github.com/andrewliao11/homework1/blob/master/lstm-formula.png?raw=true)   
The standard LSTM can be also illustrated in another style (in Grid-LSTM paper), and when you stack LSTM cell together, it become stacked LSTM:   
![](https://github.com/andrewliao11/homework1/blob/master/stacked-lstm.png?raw=true)
The stacked lstm is somewhat like the 2d grid lstm in the next part, but note that the stacked lstm is **just** the LSTM stacking together. Each LSTM cell remains the same with **only one memory, one hidden state**.
To go deeper into LSTM cell, let's talk about the multi-dimensional LSTM. The multi-dimensional LSTM is easy to understand: each LSTM cell will get the input and the hidden states from multiple dimension(***N***), and concatenate them together as ***H***. The cell will update the memory cell conditional on multiple memory cells from differnet dimension (***H***) and the formula is look like this:   
![](https://github.com/andrewliao11/homework1/blob/master/multi-dimensional.png?raw=true)   
This has a problem that the input dimension and the memory cell will grow combinatorially as the dimension(***N***) increase.

### Motivation
- LSTM is made to mitigate the gradient vanishing problem and is able to capture the long-term memory well. When dealing with computer vision problem, we might want to build a super deep neural network, while evenetually get a poor performance due to the gradient vanishing problem. So, is it possible for us to use the LSTM to mitigate this problem as well?
- Flexibility: easy to be applied to others task

### Architecture
Unlike the multidimensional case, the block outputs N hidden vectors h1 , ..., hN and N memory vectors m1 , ..., mN that are all distinct. So eventually there will be N transforms LSTM, one for each dimension.

<p align="center"><img src="https://github.com/andrewliao11/homework1/blob/master/arc1.png?raw=true" /></p>

Note that the vector H containing all the input hidden vectors is shared across the transforms, whereas the input memory vectors (m1,...,mN) affect the N-way interaction but are not directly combined (in multi-dimensional case, the differenct memory vectors are directly summed togerther).

<p align="center"><img src="https://github.com/andrewliao11/homework1/blob/master/glstm.png?raw=true" /></p>

Beware that each transform has distinct weight matrices, so the paramaters will increase significantly. So they also proposed weight sharing mechanism, Sharing of weight matrixes can be specified along any dimension in a Grid LSTM and it can be useful to induce invariance in the computation along that dimension. If the weights are shared along all dimensions including the depth, we refer to the model as a **Tied N-LSTM**

### Experiment
To show that this brand-new LSTM cell can be widely used in different task, the author show some experiments on arithmetic calculation, memorization, language modelling, and even mnist digit recognition(2D case).

- Arithmetic calculation
In this part, the arithmetic task is look like the following figure. Input is fed a sequence of digit, and the output is expected to be the sum of them.
<p align="center"><img src="https://github.com/andrewliao11/homework1/blob/master/arith.png?raw=true" /></p>   
It's tested in stacked LSTM, untied grid 2-LSTM, and tied grid 2-LSTM. The tied grid 2-LSTM got the best performance. Probably because the summation is operated repeatly, which is more direct to use the same parameter in every dimension, namely tied grid 2-LSTM.
<p align="center"><img src="https://github.com/andrewliao11/homework1/blob/master/arith-exp.png?raw=true" /></p>
## Discussion

- Stacked LSTM is different from 2d grid LSTM
  - the stacked LSTM units output the same hidden units to the spatial and temporal direction, while the 2d grid LSTM output different hiddene state to these two direction (connect the temporal and spatial information)
![](https://github.com/andrewliao11/homework1/blob/master/compare.png?raw=true)
- Multi-dimension LSTM is different from 3d grid LSTM
  - multi-dimensional LSTM just concantenate the N hidden state and the input as H(the input of the cell), which leads to every large LSTM cell as the dimension increase :point_right: the values will grown combinatorially  
- Grid LSTM
  - Use the LSTM to connect the different dimension (multi-dimension LSTM use linaer function, resulting a scalar values)
  - h1,h2,....hN are distinct, m1, m2, ...mN as well.
  
### Variant grid LSTM
- Tied Grid LSTM
- Use the non-linear function to replace the LSTM connection

## Question?
-  ***the advantage of grid lstm?***    
It's powerful and flexible, various branch of grid lstm can reach the state-of-the-art performance.

-  ***what's the point that make it better in the experiment?***    
One may says that grid lstm simply increase the paramaters, so the performance increases due to larger model. However compare to stack LSTM and multi-dimensional LSTM, grid LSTM reach better performce. One of the possible reason is that grid LSTM enable informations from different dimensioin interact with each other in an efficient way.

-  ***1D grid LSTM -> activation func?***    
For a 1D grid LSTM, it's just a vector in vector out process. We can somehow treat it as a complec activation function. Closely observe the following equations, we can see that **m** is the one who control the flow of a signal, which can be seen as the switch of the activation function   

<p align="center"><img src="https://github.com/andrewliao11/homework1/blob/master/mh.png?raw=true" width="250"> </p>  

-  2d data => apply in image? => HOW? Can go to very deep?
-  multi-dimensional grid lstm => suitable for what?


## Reference
- ***[Grid Long Short-Term Memory](https://arxiv.org/abs/1507.01526)***, ICLR 2016
- ***[GRID-LSTM slide](http://futureai.media.mit.edu/wp-content/uploads/sites/40/2015/09/GRID-LSTM.pptx_.pdf)***
- ***[grid-lstm](https://github.com/coreylynch/grid-lstm)***, source code
