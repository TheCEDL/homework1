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

**The CDS scheme can provide a  more reliable updating sequence for better semantic reasoning, since the earlier nodes in the updated sequence belong to some important semantic parts with higher confidence and their visual features may be more reliable for message passing.**

## Averaged Hidden States for Neighboring Nodes
In adaptive updating scheme, when they want to update a specific node,some of its neighboring nodes have already been updated while others may have not. Thus, they use a visit flag q<sub>j</sub> to indicate whether the graph node v<sub>j</sub> has been updated, where q<sub>j</sub> is set as 1 if updated. They use the updated hidden states h<sub>j,t+1</sub> for the visited nodes and the previous states h<sub>j,t</sub> for the unvisited nodes, and then compute the hidden states h<sub>i,t</sub> of node v<sub>i</sub> by  averaging the hidden states of neighboring nodes.


![alt text](https://github.com/Tommy-Liu/homework1/blob/master/equ1.PNG)


where the 1(·) is an indicator function and |Ng(i)| denote the number of neighboring graph nodes.


## Adaptive Forget Gates
Graph LSTM specifies different forget gates for different neighboring nodes. It results in the different influences of neighboring nodes on the updated memory states m<sub>i,t+1</sub> and hidden states h<sub>i,t+1</sub>. The memory states of each neighboring node are also utilized to update the memory states m<sub>i,t+1</sub> of the current node. The shared weight metrics U<sup>fn</sup> for all nodes are learned to guarantee the spatial transformation invariance and enable the learning with various neighbors. The Graph LSTM thus incorporates these adaptive forget gates to cover diverse visual patterns.

## Graph LSTM Unit
The Graph LSTM consists of five gates: 
the input gate g<sup>u</sup>, the forget gate g<sup>f</sup>, the adaptive forget gate g<sup>¯f</sup>, the memory gate g c and the output gate g o. The W<sup>u</sup>, W<sup>f</sup>, W<sup>c</sup>, W<sup>o</sup> are the recurrent gate weight matrices specified for input features while U<sup>u</sup>, U<sup>f</sup>, U<sup>c</sup>, U<sup>o</sup> are those for hidden states of each node. U<sup>un</sup>, U<sup>fn</sup>, U<sup>cn</sup>, U<sup>on</sup> are the weight parameters specified for states of neighboring nodes. The hidden and memory states by the Graph LSTM can be updated as follows:

![alt text](https://github.com/Tommy-Liu/homework1/blob/master/equ2.PNG)


Here δ is the logistic sigmoid function, and ⊙ indicates a point-wise product. The memory states m<sub>i,t+1</sub> of the node v<sub>i</sub> are updated by combining the memory states of visited nodes and those of unvisited nodes by using the adaptive forget gates.

# Experiments
## Datasets

1. PASCAL-Person-Part dataset
2. Horse-Cow parsing dataset
3. ATR dataset and Fasionista dataset

## Data augmentation
For dataset 1 & 2, First cropping out the object using a random generated bounding box, and then perform additional cropping, filpping, color intensity scaling and rotation, as in [12].  
For dataset 3, do horizontal flipping, as in [36].  

## Evaluation metric
For dataset 1 & 2, standard IOU and pixel-wise accuracy.  
For dataset 3, method proposed in [4][22][36], including accuracy, average precision, average recall and average F-1 score.

![alt text](https://github.com/Tommy-Liu/homework1/blob/master/F1_score.png)

## Network architecture
For dataset 1 & 2, model based on DeepLab-CRF-LargeFOV.  
For dataset 3, model based on Co-CNN.

## Weight initialization 
For Graph LSTM layers, uniform distribution between -0.1 and 0.1.  
For all convolutional layers, Gaussian distribution with σ = 0.001

## Other training settings

1. Stochastic gradient descent with batch size of 2. 
2. Momentum of 0.9 
3. Weight decay of 0.0005.

## Training Steps
Input images are fixed as 321 x 321 for dataset 1,2, and 150 x 100 for dataset 3.  
Use SLIC to generate avg.1000 superpixels per image.  

Two steps:

1. Pretrain the 1x1 convolutional layer for generate initial confidence maps.Confidence map is used to decide starting node and updating sequence.
2. Finetune the whole model based on the pretrained model in step 1. For each step, learning rate is initialized to 0.001 for newly added layers and 0.0001 for pretrained layers. Finetuning on DeepLAB-CRF-LargeFOV for 60 epochs takes 1 day.Training Co-CNN from scatch takes 4-5 days. All the training processes are done on the framework Caffe.

# Results


![alt text](https://github.com/Tommy-Liu/homework1/blob/master/table1.PNG)


Graph LSTM outperforms other baselines in average IOU. In particularly, it provides **better prediction on easily confusion parts such as upper-arms and lower-arms**. Comparing to HAZN[8], Graph LSTM has 4.95% and 6.67% higher score for lower-arms and upper legs, respectively. **This shows the effectiveness of eploiting global context to boost local predictions**.


![alt text](https://github.com/Tommy-Liu/homework1/blob/master/table2.PNG)


**Graph LSTM gives a huge boost in average IOU**. On horse class, it achieves 2.59% better performance than the state-of-the-art. Also on cow class, it achieves 7.26% better than LG-LSTM[17] and 3.11% better than HAZN[8].

**Graph LSTM (more)** is trained on extra 10,000 images from [36]
Liang, X., Xu, C., Shen, X., Yang, J., Liu, S., Tang, J., Lin, L., Yan, S.: Human parsing with contextualized convolutional neural network. In: ICCV. (2015) 


![alt text](https://github.com/Tommy-Liu/homework1/blob/master/table3.PNG)
![alt text](https://github.com/Tommy-Liu/homework1/blob/master/table4.PNG)


On ATR dataset, Graph LSTM can significantly outperform these baselines in terms of average F-1 score. With additional 10,000 training images, it’s F-1 score can be further improved to 88.20%, which is much better than CRFasRNN (more). This verifies that **Graph LSTM  is superior to the pair-wise CRF in capturing global context**.


![alt text](https://github.com/Tommy-Liu/homework1/blob/master/table5.PNG)


Train on ATR dataset and then test on the 229 images of the Fashionista dataset. Graph LSTM still substantially outperform other baselines.


![alt text](https://github.com/Tommy-Liu/homework1/blob/master/table6.PNG)


Table 6 and Table 7 compare different LSTM structures and node updatng schemes. The result shows that graph-structured LSTM and confidence-driven updating scheme have the best performance in almost every semantic parts.

# Discussions
## Graph LSTM vs locally fixed factorized LSTM  [Table 6.]
Graph LSTM has adaptive graph topologies for each image, each node representing a LSTM unit. The network is more flexible and benefits from the rich local context. Each node in Graph LSTM can has different number of neighbors, better describing the sematic structure of the image and reduce many redundant nodes comparing fixed one. I blieve that these charateristics not only make gradient propagations more effieciently but also lead to better convergence speed and convergence results.
## Graph LSTM vs superpixel smoothing  [Table6.]
Graph LSTM’s performance gain is not just from using SLIC for more accurate boundary information. Comparing to Diagonal BiLSTM and LG_LSTM plused post superpixel smoothing, Graph LSTM still bring more performance gain from its adaptive LSTM graph structure.
## Node updating scheme  [Table 7. and Table 8.]
There are two ways to select starting node for BFS and DFS methods: 1) always starts from the spatially left most node 2) starts from the node with maximal confidence on all foreground labels. Confidence updating scheme can achieve slightly better performace than other alternatives. This possibly infers that features from superpixel nodes with higher forground confidences may contain more accurate semantic informations and thus lead to more reliable global reasoning.
Instead of using all foreground labels, Table 8 test the performance of using initial confidence map of single label. In average, only slight preformance difference there. Interestingly, however, using “head” or “torso” label leads to the best two performance 61.03% and 61.45%, over all single foreground label results. It is possible that  head/torso are the major/central part of human body topology and thus lead to better discrimination of different body parts. Nevetheless, its difficult to determine the best semantic labels for each task.


![alt text](https://github.com/Tommy-Liu/homework1/blob/master/table7.PNG)


## Adaptive forget gates [Table 9.]
In the locally fixed factorized LSTMs, the **same forget states** are learned to exploit the influences of neighboring pixels on the updated states of each pixel. Whereas in Graph LSTM, **adaptive forget gates** are adopted to **treat different neighbors differently**. “Identical forget gate” shows the results of learning identical forget gates for all neighbors and simultaneously ignoring the memory states of neighboring nodes:


![alt text](https://github.com/Tommy-Liu/homework1/blob/master/equ3.PNG)


From Table 9., it can be observed that adaptive forget gates show better performance. Because diverse semantic correlations in local contenxt can be considered during the node updating.

## Superpixel number
The superpixels can group pixels based on spatial and appearance similarity, reducing the number of nodes and keep semantic consistency. However, superpixels may also introduce quantization erros. As from the experiment, there are slight improvements when using over 1,000 superpixels. 


![alt text](https://github.com/Tommy-Liu/homework1/blob/master/fig4.PNG)


## Residual connections
Residual connections can help training very deep convolutional networks. In Graph LSTM, residual connections can boost the performance from 59.12% to 60.16%.

## Advantages of Graph LSTM and further applications
In traditional LSTM, a lot of computation with the fixed topology is redundant and inefficient as it has to consider all the pixels, even for the ones in a simple plain region.Graph LSTM  propagate information from one adaptive starting superpixel node to all superpixel nodes along the adaptive graph topology for each image.**It can effectively reduce redundant computational costs** while better preserving object part boundaries to **facilitate global reasoning over the whole image**.

**The confidence-driven update scheme can provide a more reliable updating sequence for better semantic reasoning**, since the earlier nodes in the updated sequence have stronger semantic evidence (e.g., belonging to any important semantic parts with higher confidence) and their visual features may be more reliable for message passing.

In contrast to global context, the use of **adaptive forget gate** gives Graph LSTM the ability to **learn different local context relationship between neighboring nodes**.

With all the characteristics mentioned aboved, Graph LSTM provides a reliable structure to combine global and local context reasoning. This idea may be used to further improve scene understaning and image captioning, because both the global and local information need to be considered to generate good results. Also, human action detection or joint labeling may also benefit from the image global context.

## Visual Results Comparison


![alt text](https://github.com/Tommy-Liu/homework1/blob/master/fig5.PNG)
![alt text](https://github.com/Tommy-Liu/homework1/blob/master/fig6.PNG)


# Work Assignment

* Briefly intro of paper: 邱志堯、曾守曜、簡光宏
* The structure of Graph LSTM: 邱志堯、劉兆寧
* Experiments: 曾守曜、劉兆寧
* Discussion: 曾守曜、邱志堯、簡光宏
* Publish report on github : 簡光宏、劉兆寧、曾守曜

# Reference

[idx]: the reference index in the orignal paper

1. [12] Wang, P., Shen, X., Lin, Z., Cohen, S., Price, B., Yuille, A.: Joint object and part segmentation using deep learned potentials. In: ICCV. (2015)  
2. [36] Liang, X., Xu, C., Shen, X., Yang, J., Liu, S., Tang, J., Lin, L., Yan, S.: Human parsing with contextualized convolutional neural network. In: ICCV. (2015)  
3. [4] Yamaguchi, K., Kiapour, M., Berg, T.: Paper doll parsing: Retrieving similar styles to parse clothing items. In: ICCV. (2013)
4. [22] Liang, X., Liu, S., Shen, X., Yang, J., Liu, L., Dong, J., Lin, L., Yan, S.: Deep human parsing with active template regression. TPAMI (2015)  

