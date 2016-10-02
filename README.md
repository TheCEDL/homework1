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

