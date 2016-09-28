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
- Existing LSTM is made to deal with sequence (1D) data, while grid LSTM is developed to cope with multi-dimensional data (eg. image).
- Motivation: LSTM is able to capture longer memory than RNN, which deal with the gradient vanishing problem. Deep network also has the gradient vanishing problem, can generalize LSTM solve it?

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
-  2d data => apply in image?
-  multi-dimensional grid lstm => suitable for what?

## Reference
- ***[Grid Long Short-Term Memory](https://arxiv.org/abs/1507.01526)***, ICLR 2016
- ***[GRID-LSTM](http://futureai.media.mit.edu/wp-content/uploads/sites/40/2015/09/GRID-LSTM.pptx_.pdf)***
- ***[grid-lstm](https://github.com/coreylynch/grid-lstm)***, source code
