# Homework1 - Introduce a New NN with Memory

# To-Do
* [+10][Paper Title] Please find a recent paper (2014-2015) which introduced a NN with memory.
* [+50][Paper Introduction] Write a report to briefly introduce the paper;
* [+40][Experiments and Tasks] then, focus on discussing the unique properties of the new NN and where it can be applied to take advantage of the properties.

# Paper Title
One-shot Learning with Memory-Augmented Neural Networks<br>
by Google DeepMind, [To Arxiv File](https://arxiv.org/abs/1605.06065)<br>
Submitted on 19 May 2016

# Outline
- Abstract (Start of peper introduction)
- Related Work
  - Meta-Learning Task
- Memory-Augmented Model
  - Neural Turing Machine
  - Least Recently Used Access
  - MANN(Memory-Augmented Neural Network) Architectures
  - Output ditribution and Learning
    - One-hot label classification
    - String label classification
    - Regression (End of paper introduction)
    
- Experiments and Tasks
  - Omniglot Classification
  - Regression

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
(I originally intended to introduce Neural Turing Machine. However, it has been chosen from other group.) The original Neural Turing Machine architecture contains two basic components: a neural network controller and a memory bank.

The similarity between a NTM controller and a normal neural network is that the controller interacts with the external world via input and output vectors, such as LSTM. The dissimilarity between a NTM controller and a normal neural network is that the controller also interacts with a memory matrix using selective read/write operations.

Memory encoding/retrieval in a Neural Turing Machine external memory module is rapid because the vector representations are placed into or taken out almost every time-step. This makes the NTM a perfect candidate for meta-learning and low-shot prediction task. The NTM is also capable of both long-term storage and short term storage, because of the slow updates of its weights(long-term) and the external memory module(short-term).

Because of these properties, we should be able to use a well-trained NTM, which has a general strategy for the types of representations it should place into memory and how it should use these representations for predictions, to do the one-shot prediction tasks.

### Least Recently Used Access
The original NTM paper, memories were addressed by both content and location. Although the location-based addressing was advantageous for sequnce-based prediction tasks, it is not optimal for tasks that emphasize a conjunctive coding of information independent of sequences. Therefore, this paper proposed a newly designed memory access module, Least Recently Used Access(LRUA).

The LRUA module is a pure content-based memory writer that writes memories to either the least or the most recently used memory location, so as to preserve recent, and hence potentially useful memories(LRU), or to update the memory with newer, possibly more relevant information(MRU). If the memories were written into the previously used slot, the least used memories would simply get erased.

### MANN(Memory-Augmented Neural Network) Architectures
The MANN model proposed in this paper is a variant of a NTM:<br>
![pic here]():<br>
The controllers in the experiments in this paper are feed-forward networks or LSTMs. The best performance comes from a LSTM with 200 hidden units. The controller receives some concatenated input (x_t, y_t-1) and update its state.

### Output distribution and Learning
#### For one-hot label classification
The controller output is first passed through a linear layer with an output size equal to the number of classes to be classified per episode. This linear layer output is then pased as input to the output distribution. The output distribution is a categorical distribution, implemented as a softmax function:<br>
![pic here]()<br>

In the learning phase, the network minimizes the episode loss of the input sequence:<br>
![pic here]()<br>
<i>y_t</i> is the target one-hot label at time <i>t</i>, only one element assumes the value 1.

#### For string label classification
The linear output size is kept at 25. Each splitted parts of size 5 is sent to an independent categorical distribution that computes probabilities across its five inputs.

In the learnig phase, the loss is similar to that of one-hot label classification:<br>
![pic here]()<br>
<i>y_t</i> is the target string label at time <i>t</i>, five elements assumes the value 1(one per five-element 'chunk').

#### For regression
The network's output distribution is a Gaussian, and as such receives two-values from the controller output's linear layer at each time-step. Therefore, in the learning phase, the network minimizes the negative log-probabilities as determined by the Gaussian output distribution given these parameters and the true target <i>y_t</i>.

# Experiment and Tasks

# Other
* Due on Oct. 3rd before class.
