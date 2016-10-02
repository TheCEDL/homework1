# End-to-End memory network
author: Sainbayar Sukhbaatar, Arthur Szlam, Jason Weston, Rob Fergus   
link: [[paper](http://arxiv.org/abs/1503.08895)] [[code](https://github.com/facebook/MemNN)]

## Abstract
* Introduce a memory network with a recurrent soft attention model over a large external memory.
* It can be trained end-to-end, so it requires less supervision during training. (Only need supervision on the final output.)
* A novel architecture where the recurrence reads from a possibly large external memory multiple times before outputting a symbol. (Performs multiple lookups (hops) on memory.)
* This model can not only be applied on synthetic question answering problems, but also language modeling problems.

## Approach
* Take discrete set {x1,...,xn} that are to be stored in memory as input, a query q, and output an answer a.
* Input xi, query q and answer a contains symbols coming from a dictionary with V words.
* The model writes all x to the memory up to a fixed buffer size, and then finds a continuous representation for the x and q.

## Model
![](https://github.com/gina9726/homework1/images/QA-single%20layer.png)
* Single layer
    * Input memory representation
        - The entire set of {xi} are converted into memory vectors {mi} of dimension d computed by embedding each xi in a continuous space, using embedding matrix A of size V*d. 
        - Query q is also embedded, using matrix B with the same size as matrix A, output a vector u.
        - In embedding space, compute the match between u and each memory mi by taking inner product followed by a softmax, output pi, so the set p={pi} is a probability vector over the inputs.
    * Output memory representation
        - Each xi has a corresponding output vector ci, using embedding matrix C with the same size as A and B.
        - The response vector from the memory o is then a sum over the transformed inputs ci, weighted by the probability vector from the input: o = sum(pi*ci)

![](https://github.com/gina9726/homework1/images/QA-multiple%20layer.png)
* Multiple layers (multiple hops)
    * The model can be extended to handle multiple hop operations. To reduce the number of parameters, they propose two types of weight tying approaches.
        - Adjacent weight tying   
![](https://github.com/gina9726/homework1/images/QA-Adjacent.png)
        - Layer-wise (RNN-like) weight tying
![](https://github.com/gina9726/homework1/images/QA-RNN.png)

## Experiment
* Synthetic Question Answering
    * Dataset: [bAbI QA tasks](https://research.facebook.com/research/babi/)
        - This dataset is proposed by facebook AI Research, containing 20 types of QA tasks.
    * Details:
        - Sentence representation
          - BOW(bag-of-words): lack of the order in the sentence.
          - PE(position encoding): encode the position of words in the sentence.
        - Temporal encoding
          - To encode the order of sentences by special matrices, which are learned by training.
        - Learning time invariance by injecting random noise(RN)
          - Add dummy memories to regularize special matrices.
        - Linear start
          - Training without Softmax layer before the loss is stuck.
    * Results:
        - The error rate of MemN2N is comparable to strongly supervised MemNN, and is much lower than weakly supervised methods.
        - It can be found that multiple hops can lead a better result.

* Language Modeling
    * Dataset: Penn Treebank and Text8
    * Details:
        - The goal in language modeling is to predict the next word in a text sequence given the previous words x.
        - Use multiple layers model with layer-wise (RNN-like) weight sharing.
    * Results:
        - The perplexity of MemN2N is lower than baseline models on the both datasets.
        - Increasing number of hops and memory size can get lower perplexity.
        - (Note: lower perplexity => better model.)

## Discussion
* This paper introduces a recurrent attention model over a large external memory, which is revised from [Memory Network](https://arxiv.org/abs/1410.3916). A memory network is composed of a global storage and a controller. The controller sends an query message to read a memory part at each step, and does some operations. The benefit of memory network over RNN or LSTM is **that each memory block is placed independently in the global external memory. In this way, the input sentences will not be mixed during the propagation.** Therefore, we can pay attention to the supporting sentences more easily.   
* In the previous works of QA tasks, the supporting subset should be strongly or weakly supervised; however, in this work, the **soft attention model can find its way to find the correlative sentences**. Besides, the hard max operations within each layer is replaced with softmax function that lead the easy calculation of backpropagation. Thus, it can be trained end-to-end.
    

   

