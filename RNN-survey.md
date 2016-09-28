# Grid Long Short-Term Memory
Nal Kalchbrenner & Ivo Danihelka & Alex Graves, from Google DeepMind

## Overview
- Introduction
  - Motivation
  - Architecture
  - Experiment
- Discussion

## Introduction
- Existing LSTM is made to deal with sequence (1D) data, while grid LSTM is developed to cope with multi-dimensional data (eg. image).
- Motivation: LSTM is able to capture longer memory than RNN, which deal with the gradient vanishing problem. Deep network also has the gradient vanishing problem, can generalize LSTM solve it?

## Discussion
- Stacked LSTM is different from 2d grid LSTM
- Multi-dimension LSTM is different from 3d grid LSTM
  - multi-dimensional LSTM just concantenate the N hidden state and the input as H(the input of the cell), which leads to avery large LSTM cell as the dimension increase :point_right: the values will grown combinatorially  


## Reference
- ***[Grid Long Short-Term Memory](https://arxiv.org/abs/1507.01526)***, ICLR 2016
- ***[GRID-LSTM](http://futureai.media.mit.edu/wp-content/uploads/sites/40/2015/09/GRID-LSTM.pptx_.pdf)***
- ***[grid-lstm](https://github.com/coreylynch/grid-lstm)***, source code
