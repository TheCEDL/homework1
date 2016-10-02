# Homework1 - Introduce a New NN with Memory

# Already-Did
* [+10] Please find a recent paper (2014-2015) which introduced a NN with memory.
* [+50] Write a report to briefly introduce the paper;
* [+40] then, focus on discussing the unique properties of the new NN and where it can be applied to take advantage of the properties.

# Paper Title
One-shot Learning with Memory-Augmented Neural Networks<br>
by Google DeepMind, [Arxiv File link](https://arxiv.org/abs/1605.06065)<br>
Submitted on 19 May 2016

# Outline
- Abstract
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
  - Task Setup
  - Omniglot Classification
    - One-hot label classification
    - String label classification
  - Regression
- Conclusion

## Abstract
Traditional gradient-based networks require a lot of data to learn, often through extensive iterative training. The models must inefficiently relearn parameters to adequately incorporate with the new encountered data information.

Neural Turing Machines (networks with memory capacities) offer the ability to quickly encode and retrieve new information so it can potentially obviate the down-sides of conventional models.

This paper introduce the ability of a memory-augmented neural network to rapidly assimilate new data, and make accurate predictions after only a few samples, with additional method for accessing an external memory that focuses on memory content, which is quite different from the previous methods that use memory location-based focusing mechanisms.

## Related Work
### Related one-shot learning methods
I did some survey before reading this paper. For the one-shot learning task, there are mostly tackled with generative model (since prof. Fei-Fei, Li in 2006). NIPS this year (2016) also proposed a paper [Learning feed-forward one-shot learners](https://arxiv.org/abs/1606.05233).

However, these tasks which focus on one-shot learning didn't apply the power of memory networks. In recent months, Google's DeepMind proposed two papers in May (this paper), and in June (combining Metric Learning with Memory Networks to solve the one-shot learning task. [Link is here](https://arxiv.org/abs/1606.04080)). Although the latter one is a generally more powerful one-shot learning model, in homework 1 I decided to focus solely on the memory-augmented neural network discussed in this paper.

### About Meta-Learning
Meta-Learning is a form of "Learning to learn", we choose parameters to reduce the expected learning cost across a distribution of datasets p(D):<br>
![Meta-learning loss](https://github.com/markakisdong/homework1/blob/master/fig/Meta-learning_loss.png)<br>
Meta-learning model that meta-learns would learn to bind data representations to their appropriate labels regardless of the actual content of the data representation or label. These method is heavily used in one-shot learning tasks. However, is not really associated with memory network, so I'm going to stop here.

## Memory-Augmented Model
### Neural Turing Machine
(I originally intended to introduce Neural Turing Machine. However, it has been chosen from other group.) The original Neural Turing Machine architecture contains two basic components: a neural network controller and a memory bank:<br>
![Original NTM](https://github.com/markakisdong/homework1/blob/master/fig/NTM_original.png)<br>

The similarity between a NTM controller and a normal neural network is that the controller interacts with the external world via input and output vectors, such as LSTM. The dissimilarity between a NTM controller and a normal neural network is that the controller also interacts with a memory matrix using selective read/write operations.

Memory encoding/retrieval in a Neural Turing Machine external memory module is rapid because the vector representations are placed into or taken out almost every time-step. This makes the NTM a perfect candidate for meta-learning and low-shot prediction task. The NTM is also capable of both long-term storage and short term storage, because of the slow updates of its weights(long-term) and the external memory module(short-term).

Because of these properties, we should be able to use a well-trained NTM, which has a general strategy for the types of representations it should place into memory and how it should use these representations for predictions, to do the one-shot prediction tasks.

### Least Recently Used Access
The original NTM paper, memories were addressed by both content and location. Although the location-based addressing was advantageous for sequnce-based prediction tasks, it is not optimal for tasks that emphasize a conjunctive coding of information independent of sequences. Therefore, this paper proposed a newly designed memory access module, Least Recently Used Access(LRUA).

The LRUA module is a pure content-based memory writer that writes memories to either the least or the most recently used memory location, so as to preserve recent, and hence potentially useful memories(LRU), or to update the memory with newer, possibly more relevant information(MRU). If the memories were written into the previously used slot, the least used memories would simply get erased.

### MANN(Memory-Augmented Neural Network) Architectures
The MANN model proposed in this paper is a variant of a NTM:<br>
![MANN architecture](https://github.com/markakisdong/homework1/blob/master/fig/MANN_architecture.png):<br>
The controllers in the experiments in this paper are feed-forward networks or LSTMs. The best performance comes from a LSTM with 200 hidden units. Duriung training and testing, the controller receives concatenated inputs (x_t, y_t-1) and update its state.

### Output Distribution and Learning
#### For one-hot label classification
The controller's output is first passed through a linear layer with an output size equal to the number of classes per episode. This linear layer output is then passed as input to the output distribution. The output distribution is a categorical distribution, implemented as a softmax function:<br>
![Output distribution one-hot label classification](https://github.com/markakisdong/homework1/blob/master/fig/Output_distribution_one-hot_label_classification.png)<br>

In the learning phase, the network minimizes the episode loss of the input sequence:<br>
![one-hot label classification loss](https://github.com/markakisdong/homework1/blob/master/fig/One-hot_label_classification_loss.png)<br>
<i>y_t</i> is the target one-hot label at time <i>t</i>, only one element assumes the value 1.

#### For string label classification
The linear output size is kept at 25. Each splitted parts of size 5 is sent to an independent categorical distribution that computes probabilities across its five inputs, thus form the output distribution.

In the learnig phase, the loss is similar to that of one-hot label classification:<br>
![string label classification loss](https://github.com/markakisdong/homework1/blob/master/fig/String_label_classification_loss.png)<br>
<i>y_t</i> is the target string label at time <i>t</i>, 5 elements assumes the value 1 (one per five-element 'chunk').

#### For regression
The network's output distribution is a Gaussian, and as such receives two-values from the controller output's linear layer at each time-step. So, in the learning phase, the network minimizes the negative log-probabilities as determined by the Gaussian output distribution given these parameters and the true target <i>y_t</i>.

# Experiment and Tasks
For classification task, Omniglot dataset is used. For regression task, sampled functions from a Gaussian process with fixed hyperparameters is used.

## Task setup
For classification, <i>y_t</i> is the class label for an image <i>x_t</i>, and for regression, <i>y_t</i>is the value of hidden function for a vector with real-valued element <i>x_t</i>. In this setup, <i>y_t</i> is both a target, and is presented as input along with <i>x_t</i> (as mentioned previously in section MANN architecture, input were concatenated). The more detailed setup is described in the below figure.<br>
![Task setup](https://github.com/markakisdong/homework1/blob/master/fig/Task_setup.png)<br>

## Omniglot classification
### One-hot label classification
MANN was trained using one-hot vector representations as class labels (the figure above). After training, the network was to predict the class labels for never-before-seen classes pulled from a disjoint test set from within Omniglot. One relevant baseline is human performance. In a series of proper setup for human performance testing which I skipped here, the performance of the MANN surpassed that of a human on each instance. Interestingly, the MANN displayed better than random guessing on the first instance within a class:<br>
![Omniglot one-hot accuracy](https://github.com/markakisdong/homework1/blob/master/fig/One-hot_accuracy_table.png)<br>

### String label classification
Since learning the weights of a classifier using large one-hot vectors becomes increasingly difficult with sclae, this paper adopted another approach: constructing new labels consisting of 5 characters (string). Strings were represented as concatenated one-hot vectors, hence length 25 with 5 elements assuming a value 1 (as mentioned previously in Section output distribution). The proposed MANN with LRUA surpassed MANN with standard NTM module. The rest detail setup is skipped:<br>
![String classification accuracy](https://github.com/markakisdong/homework1/blob/master/fig/String_classification_accuracy.png)<br>

The below figure is Omniglot accuracy during increasing training episodes comparing to novice LSTM, (a) and (b) are one-hot labels, (c) and (d) are five-characteres string labels.<br>
![Omniglot classification](https://github.com/markakisdong/homework1/blob/master/fig/Omniglot_classification_all.png)<br>

There are other method to test the network, including persistent memory interference, that is, there are unique classes/labels in each episode. The test is that they performed the classification task without wiping the external memory between episodes. However, this task proved predctably difficult, and the network was less robuts in its ability to achieve accurate classification. They stated that exploring the requirements for robust performance is a topic of future work.

They also use curriculum training on the MANN. The training started with 15 unique classes, and increment one in every 10000 episodes. The network, yet, generally exhibited gradually decaying performance as the number of classes increased towards 100. They stated that assessing the maximum capacity of the network is also a topic of future work.<br>
![Curriculum classification decay](https://github.com/markakisdong/homework1/blob/master/fig/Curriculum_classification_decay.png)<br>

## Regression
To test regression, they generated functions using a GP prior with a fixed set of hyper-parameters. The network was trained using unique functions in each episode. Unlike in the image-classification scenario, this task demands a broader read from memory so the network must learn to interpolate from previously seen points, which most likely involves a strategy to have a more blended read-out from memory. The performance of the network was compared to true GP predictions of samples presented in the same order as was seen by the network.

In both the 2-d and 3-d experiments, the log-likelihood predictions of the MANN tracks appreciably well versus the GP, with predictions becoming more accurate as samples are stored in the memory.<br>
![GP regression](https://github.com/markakisdong/homework1/blob/master/fig/GP_regression.png)

# Conclusion
The central contribution of this paper is to demonstrate the special utility of a particular class of Memory-Augmented Neural Networks for meta-learning. The MANN examined here was found to display performance superior to a LSTM in the two experiments. We can say that MANNs are well-suited to meet the challenge that the new information must be flexibly stored and accessed in the experiments conducted in this paper.

# Contribution of this homework
Mark 董晉東, 100%
# Other
* Due on Oct. 3rd before class.
