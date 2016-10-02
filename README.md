# Homework1 - Introduce a New NN with Memory 
* Team Member : 邱志堯 , 劉兆寧, 曾守曜 ,簡光宏
* Paper title : Semantic Object Parsing with Graph LSTM (ECCV2016)
* Paper link : https://arxiv.org/pdf/1603.07063v1.pdf

# Briefly intro of paper
**The topic in this paper is semantic object parsing which aims to segment an object within an image into multiple parts** with more fine-grained semantics. Many high level computer vision applications can benefit from a powerful semantic object parser, including action recognition, clothes recognition and retrieval and human behavior analysis.<br>


![alt text](https://github.com/Tommy-Liu/homework1/blob/master/fig1.PNG)


In this paper, **they introduce a novel Graph LSTM model to do a more precise segmentation by considering global structure context.** Graph LSTM extends the traditional LSTMs from sequential and multi-dimensional data to general graph-structured data. **The graph topology constructed from the graph-structured data can effectively exploit the global context.**<br>

# Briefly intro of Graph LSTM
![alt text](https://github.com/Tommy-Liu/homework1/blob/master/fig2.PNG)
## Adaptive graph topology
The adaptive graph topology is constructed based on the superpixel nodes and their spatial connections. Each node can be influenced by various numbers of neighboring nodes. Hence, Graph LSTM can effectively propagate information from one adaptive starting superpixel node to all superpixel nodes along the adaptive graph topology for each image.

## A scheme to select the starting node of topology
**Proposed confidence-driven scheme selects the starting node of graph topology and sequentially updates all nodes, which facilitates the flexible inference while preserving the visual characteristics of each image.**


Previous LSTMs often starts at pre-defined locations and then proceed toward other locations following a fixed updating route for different images. In contrast, new scheme starts from a proper superpixel node and updating the nodes following a certain content-adaptive path which leads to a more flexible and reliable inference for global context modelling. 
## Learn the forget gates with respect to different neighboring nodes
In previous LSTMs, each node receives influence equally from all of its neighboring nodes by sharing the forget gates , which is not always true in visual applications. Iin Graph LSTM, they learn the forget gates with respect to different neighboring nodes in order to model various neighbor connections. This scheme is beneficial with Graph LSTM, in which the connections between nodes convey more semantically meaningful interactions than the ones in the pixels/patches based LSTMs with a fixed topology.
## How Graph LSTM be appended into NN
![alt text](https://github.com/Tommy-Liu/homework1/blob/master/fig3.PNG)
The Graph LSTM layers built on a superpixel map are appended on the convolutional layers in a FCN to enhance visual features with global structure context. The convolutional features pass through 1 × 1 convolutional filters to generate the initial confidence maps for all labels. The node updating sequence for the subsequent Graph LSTM layers is determined by the confidence-drive scheme based on the initial confidence maps, and then the Graph LSTM layers can sequentially update the hidden states of all superpixel nodes. The residual connections are also incorporated between the Graph LSTM layers to increase convergence speed and propagate signals more directly through the network.
# The structure of Graph LSTM
## Confience-driven Scheme
Given the constructed undirected graph G, they tried several schemes to update all nodes in a graph in the experiments, including the BFS,DFS and Confidence-Driven Search (CDS). They find that the CDS achieves better performance.


Given the top convolutional feature maps, the 1 × 1 convolutional filters can be used to generate the initial confidence maps with regard to each semantic label. Then the confidence of each superpixel for each label is computed by averaging the confidences of its contained pixels, and the label with highest confidence could be assigned to the superpixel.


Among all the foreground superpixels (i.e., assigned to any semantic part label), the node updating sequence can be determined by ranking all the superpixel nodes according to the confidences of their assigned labels. The ones with higher confidences will be updated first. If two nodes have the same confidence score, the spatially left one will be first updated.


**The CDS scheme can provide a  more reliable updating sequence for better semantic reasoning, since the earlier nodes in the updated sequence belong to  ｓｏsome important semantic parts with higher confidence and their visual features may be more reliable for message passing.**
## Averaged Hidden States for Neighboring Nodes
In adaptive updating scheme, when they want to update a specific node,some of its neighboring nodes have already been updated while others may have not. Thus, they use a visit flag qj to indicate whether the graph node vj has been updated, where qj is set as 1 if updated. They use the updated hidden states hj,t+1 for the visited nodes and the previous states hj,t for the unvisited nodes, and then compute the hidden states h i,t of node vi by  averaging the hidden states of neighboring nodes.


![alt text](https://github.com/Tommy-Liu/homework1/blob/master/equ1.PNG)


where the 1(·) is an indicator function and |Ng(i)| denote the number of neighboring graph nodes.


## Adaptive Forget Gates
Graph LSTM specifies different forget gates for different neighboring nodes. It results in the different influences of neighboring nodes on the updated memory states mi,t+1 and hidden states hi,t+1. The memory states of each neighboring node are also utilized to update the memory states mi,t+1 of the current node. The shared weight metrics U fn for all nodes are learned to guarantee the spatial transformation invariance and enable the learning with various neighbors. The Graph LSTM thus incorporates these adaptive forget gates to cover diverse visual patterns.
## Graph LSTM Unit
The Graph LSTM consists of five gates: 
the input gate g u, the forget gate g f, the adaptive forget gate g¯ f, the memory gate g c and the output gate g o. The Wu, Wf, Wc, Wo are the recurrent gate weight matrices specified for input features while Uu, Uf, Uc, Uo are those for hidden states of each node. Uun, Ufn, Ucn, Uon are the weight parameters specified for states of neighboring nodes. The hidden and memory states by the Graph LSTM can be updated as follows:


![alt text](https://github.com/Tommy-Liu/homework1/blob/master/equ2.PNG)


Here δ is the logistic sigmoid function, and ⊙ indicates a point-wise product. The memory states mi,t+1 of the node vi are updated by combining the memory states of visited nodes and those of unvisited nodes by using the adaptive forget gates.

# Experiments
## Datasets
1. PASCAL-Person-Part dataset
2. Horse-Cow parsing dataset
3. ATR dataset and Fasionista dataset
## Data augmentation
For dataset 1 & 2, First cropping out the object using a random generated bounding box, and then perform additional cropping, filpping, color intensity scaling and rotation, as in [12]. For dataset 3, do horizontal flipping, as in [36].
## Evaluation metric
For dataset 1 & 2, standard IOU and pixel-wise accuracy. For dataset 3, method proposed in [4][22][36], including accuracy, average precision, average recall and average F-1 score.
