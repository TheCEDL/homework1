#Neural Turing Machine
##Introduction and Overall Architecture
<p>
Neural Turing Machines (NTM) can be basically viewed as a machine with simple regular computer architecture, which consists of a controller, memory, and read/write heads, shown as below. The controller, which is made out of neural network (LSTM and feedforward network), makes decisions, takes actions and outputs depending on external input and information read from memory. Meanwhile, parameters of read/write heads and contents stored in memory are updated according to controller decisions.
</p>
<p align="center">
  <img src="img/neuralturing.png"></img>
</p>
<p>
Ideally, NTM can accomplish whatever tasks a general computer is capable of. The main difference between NTM and computers is mechanism set up for controller decisions and actions. In computer, controller (CPU) is designed manually by engineers. Instruction set and a variety of computing method (e.g. add, floating point) are embedded into controller to cope with commands from users. On the other hand, design in NTM controller only associates with architecture of neural network used inside. Strategies while confronting problems are not defined explicitly by engineers but can be learned through feeding input output pairs. In other word, ultimately, NTM can learn algorithms which may or may not be designed and developed by human.<br>
Furthermore, NTM is a neural network with memory. NTM has a explicitly-defined memory space, in which we can seperate “thinking” from “memory”, which is much similar to human brain. Also, apart from LSTM RNN, where memory is located implicitly and distributedly in the hidden state of each LSTM cell, centralized management over memory of NTM, in some cases, enjoy much faster training process. In certain intuition, imagine that we want to know the name of the host in a party. We can either try hard to learn from the hustle or simply jusk ask a person. 
</p>
##Read/Write Operation
###Read
<p>With normalized weighting vector Wt, we will get</p>
<p center="align">
<img src="img/normalized_weight_vector.png"></img>
</p>

###Write
