#Neural Turing Machine
##Introduction and Overall Architecture
<p>
<a href="https://arxiv.org/abs/1410.5401">Neural Turing Machines (NTM)</a> can be basically viewed as a machine with simple regular computer architecture, which consists of a controller, memory, and read/write heads, shown as below. The controller, which is made out of neural network (LSTM and feedforward network), makes decisions, takes actions and outputs depending on external input and information read from memory. Meanwhile, parameters of read/write heads and contents stored in memory are updated according to controller decisions.
</p>
<p align="center">
  <img src="img/neuralturing.png">
</p>
<p>
Ideally, NTM can accomplish whatever tasks a general computer is capable of. The main difference between NTM and computers is mechanism set up for controller decisions and actions. In computer, controller (CPU) is designed manually by engineers. Instruction set and a variety of computing method (e.g. add, floating point) are embedded into controller to cope with commands from users. On the other hand, design in NTM controller only associates with architecture of neural network used inside. Strategies while confronting problems are not defined explicitly by engineers but can be learned through feeding input output pairs. In other word, ultimately, NTM can learn algorithms which may or may not be designed and developed by human.<br>
Furthermore, NTM is a neural network with memory. NTM has a explicitly-defined memory space, in which we can seperate “thinking” from “memory”, which is much similar to human brain. Also, apart from LSTM RNN, where memory is located implicitly and distributedly in the hidden state of each LSTM cell, centralized management over memory of NTM, in some cases, enjoy much faster training process. In certain intuition, imagine that we want to know the name of the host in a party. We can either try hard to learn from the hustle or simply jusk ask a person. 
</p>
##Read/Write Operation
###Read
<p>With normalized weighting vector Wt, we will get</p>
<p align="center">
<img src="img/normalized_weight_vector.png">
</p>
<p>If we have a row vector Mt in memory, then we define the read vector by multipliation with weighting vector and memory vector:
</p>
<p align="center"><img src="img/read_vector.png"></p>
<p>then rt is data we get.</p>

###Write
<p>
When we want to write data into memory, we split writing into two parts: erase and add operation. First, we will erase data in memory with an erase vector et:
</p>
<p align="center"><img src="img/erase_vector.png"></p>
<p>
Mt bar is the erased memory based value and we will add it by an add vector:
</p>
<p align="center"><img src="img/add_vector.png"></p>
<p>
The Mt will be the final value stored into memory.
</p>
##Addressing
<p>
In the previous part, we talked about the process of reading and writing of the memory. In this part, we will discuss about 
how the weight is produced to address the memory. There are two types of addressing: 
</p>
###Content based
<p>
Content based addressing relies on similarity between the output of controller and the content in the memory. As a result, it is relatively simple. 
</p>
###Location based
<p>
Location based is used in arithmatic operation. Since the content in such variable is arbitrary, the variable name is the one 
matters. Thus, similarity is of no use here.<br><br>
</p>
<p>
In Neural Turing Machine, it combined two types of addressing machanism to produce the weighting vector. The system flow diagram:
</p>
<p align="center"><img src="img/Flow_diagram.png"></p>
###Content Addressing
<p>In content addressing, we compare the output vector of controller with all the vectors stored in the memory.</p>
<p align="center"><img src="img/similarity_comparison.png"></p>
<p>kt is the output vector, ßt is a scalar deciding the precision of the focus, and K is the function producing cosine
similarity(like vector inner product):
</p>
<p align="center"><img src="img/cosine.png"></p>
<p>
This part can be viewed as producing a probabilty distribution of whether kt would be in certain location.  The effect
of scalar ßt is illustrated in the picture below:
</p>
<p align="center"><img src="img/beta_t.jpeg"></p>
###Interpolation
<p>
Take the weight of current time step(produced by content addressing part) and the weight of previous time step as inpu
t, we perform interpolation based on the scalar gt(interpolation gate):
</p>
<p align="center"><img src="img/gate_interpolation.png"></p>
<p align="center"><img src="img/interpo.png"></p>
###Convolutional Shift
<p>
In previous part, we focused more on content based addressing. If gt  is relatively samll, the effect of weight produc
ed in current time step has less influence. Thus we can view this part as performing location based addressing.
<br>Take st vector produced by controller as input, it performs circular convolution on the weight sent from 
interpolation part:
</p>
<p align="center"><img src="img/convolutional_shift.png"></p>
<p align="center"><img src="img/shift.png"></p>
###Sharpening
<p>
After convolutional shift part, the probability distribution is changed again. As a result, in the last part, we 
should adjust the the final result by sharpening
</p>
<p align="center"><img src="img/sharpening.png"></p>
<p align="center"><img src="img/gama.png"></p>

##Controller
<p>
Controller is a neural network which will generate the representation that is used by read and write heads. Controller can be either a feed-foward network or a RNN.
</p>
###Type1, Feed-Forward
<p align="center"><img src="img/flow1.png"></p>
###Type2, RNN(LSTM)
<p align="center"><img src="img/flow2.png"></p>

##Property and Application
###Property
<ul>
<li><strong>Flexibility</strong></li>
<p>
Since neural turing machine can learns general concepts and algorithms, we can feed input output pair associated with a variety of tasks as long as the controller architecture contain sufficient computational capability over certain algorithm or concepts.
</p>
<li><strong>External Memory</strong></li>
<p>
Unlike LSTM, whose memory is distributedly stored in each LSTM cell, neural turing machine seperates memory from controller and thus centralizes management over memory. This design benifits longer term memory and faster training.
</p>
<li><strong>Addressing Mechanism</strong></li>
<p>
Addressing paramemters are hand-crafted parameters, and one can design his own parameters to reach the requirment.
</p>
</ul>
###Usage
<ol>
<li>
In the concept of turing machine, it can perform various tasks and it is the prototype of the computers. Computers need to be programed so that they can achieve the desire missions. Instead of being programed, neural turing machine can learn these tasks like the way our brains do. As a result, with enough capability, neural turing machine can become the neural net version computer whose architecture of controllers may have more potential to simulate human brains rather than manully-designed computational units.
</li>
<li>
GGG
</li>
</ol>





