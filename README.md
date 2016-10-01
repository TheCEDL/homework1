# Homework1 - Introduce a New NN with Memory

# To-Do
* [+10][Paper Title] Please find a recent paper (2014-2015) which introduced a NN with memory.
* [+50][Paper Introduction] Write a report to briefly introduce the paper;
* [+40][Experiments and Tasks] then, focus on discussing the unique properties of the new NN and where it can be applied to take advantage of the properties.

# Paper Title
One-shot Learning with Memory-Augmented Neural Networks<br>
by Google DeepMind, [To Arxiv File](https://arxiv.org/abs/1605.06065)<br>
Submitted on 19 May 2016

# Paper Introduction
- Abstract
- Related Work
  - Meta-Learning Task
- Memory-Augmented Model
  - Neural Turing Machine
  - Least Recently Used Access
  - MANN(Memory-Augmented Neural Network) Architectures

## Abstract
Traditional gradient-based networks require a lot of data to learn, often through extensive iterative training. The models must inefficiently relearn parameters to adequately incorporate with the new encountered data information.

Neural Turing Machines(memory capacities) offer the ability to quickly encode and retrieve new information so it can potentially obviate the down-sides of conventional models.

This paper introduce the ability of a memory-augmented neural network to rapidly assimilate new data, and make accurate predictions after only a few samples, with additional method for accessing an external memory that focuses on memory content, which is quite different from the previous methods that use memory location-based focusing mechanisms.

## Related Work
### Related one-shot learning methods
I did some survey before reading this paper. For the one-shot learning task, there are mostly tackled with generative model (since prof. Fei-Fei, Li in 2006). NIPS this year (2016) also proposed a paper [Learning feed-forward one-shot learners](https://arxiv.org/abs/1606.05233).

However, these tasks which focus on one-shot learning didn't apply the power of memory networks. In recent months, Google's DeepMind proposed two papers in May (this paper), and in June (combining Metric Learning with Memory Networks to solve the one-shot learning task. [Link is here](https://arxiv.org/abs/1606.04080)). Although the latter one is a generally more powerful one-shot learning model, in homework 1 I decided to focus solely on the memory-augmented neural network in this paper.

### About Meta-Learning
Meta-Learning is like "Learning to learn", we choose parameters to reduce the expected learning cost across a distribution of datasets p(D):<br>
![pic here]()<br>
Meta-learning model that meta-learns would learn to bind data representations to their appropriate labels regardless of the actual content of the data representation or label. These method is heavily used in one-shot learning tasks. However, is not really associated with memory network, so I'm going to stop here.

## Memory-Augmented Model
### Neural Turing Machine
### Least Recently Used Access
### MANN(Memory-Augmented Neural Network) Architectures

## 
# Experiment and Tasks

# Other
* Due on Oct. 3rd before class.
