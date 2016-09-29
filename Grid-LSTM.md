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
The standard LSTM can be also illustrated in another style (in Grid-LSTM paper), and when you stack LSTM cell together, it become stacked LSTM:   
![](https://github.com/andrewliao11/homework1/blob/master/stacked-lstm.png?raw=true)
The stacked lstm is somewhat like the 2d grid lstm in the next part, but note that the stacked lstm is **just** the LSTM stacking together. Each LSTM cell remains the same with only one memory, one hidden state.
To go deeper into LSTM cell, let's talk about the multi-dimensional LSTM. The multi-dimensional LSTM is easy to understand: each LSTM cell will get the input and the hidden states from multiple dimension(***N***), and concatenate them together as ***H***. The cell will update the memory cell conditional on multiple memory cells from differnet dimension (***H***) and the formula is look like this:   
![](https://github.com/andrewliao11/homework1/blob/master/multi-dimensional.png?raw=true)   
This has a problem that the input dimension and the memory cell will grow combinatorially as the dimension(***N***) increase.

- Existing LSTM is made to deal with sequence (1D) data, while grid LSTM is developed to cope with multi-dimensional data (eg. image).
- Motivation: LSTM is able to capture longer memory than RNN, which deal with the gradient vanishing problem. Deep network also has the gradient vanishing problem, can generalize LSTM solve it?
- Change the way LSTM communicate with each others
- Can be applied to feed-forward network or recurrent neural network

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
-  the advantage of grid lstm?
-  what's the point that make it better in the experiment?
-  2d data => apply in image? => HOW? Can go to very deep?
-  multi-dimensional grid lstm => suitable for what?
- 1D grid LSTM -> activation func?

## Reference
- ***[Grid Long Short-Term Memory](https://arxiv.org/abs/1507.01526)***, ICLR 2016
- ***[GRID-LSTM slide](http://futureai.media.mit.edu/wp-content/uploads/sites/40/2015/09/GRID-LSTM.pptx_.pdf)***
- ***[grid-lstm](https://github.com/coreylynch/grid-lstm)***, source code
