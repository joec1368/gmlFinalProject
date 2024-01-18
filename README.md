# Graph Machine Leanring Final Project
## Background
In most graph problems today, only one problem can be handled at a time, and if tasks need to be performed on different graphs, fine-tuning is required. Is there a chance to handle different graphs and tasks without fine-tuning?

## Motivations
After reading PRODIGY[[1]](#1),the main achievement of PRODIGY is that this model can accept other graphs as input without fine-tuning, and still achieve high performance. I am wondering if it's possible to modify it to not only handle multiple graphs, but also process multiple different tasks simultaneously.

## Technical Challenges
Because the graph requires different processing for different tasks, and each structure of the graph itself is different, it is difficult to have a unified model that can handle different tasks and different structures simultaneously. So If this way works, we can save a lot of time and money to do multiple graph with a variety of tasks.

## The Proposed Method
Using Prog[[2]](#2) meta learning as prompt way to transform the node, edge problmes to graph level problems. Then use this prompt graph as new input graph.
![image](https://hackmd.io/_uploads/ByVhf_IY6.png)
* In the Prog[[2]](#2) paper, the results of my research indicate that his primary approach involves utilizing a pre-trained graph label prediction model. Subsequently, under the condition of maintaining the overall framework unchanged, problems at the edge and node levels are elevated to the level of graph labels using prompts. This allows the model to meet the requirements of the original pre-training and enables it to accomplish diverse tasks. Therefore, in my approach, after extracting the graph, prompts can be added to elevate it to the level of graph label problems.
* Then, using the framework from the PRODIGY[[1]](#1) paper, in the data graph section, we employ GNN to learn graph embeddings and utilize the task graph to determine the corresponding label.
* Therefore, we need to:
    - Utilize meta-learning to learn suitable prompts.
    - Train a model capable of extracting graph embeddings from a graph based on query and prompt.
## Training Procedure
* Put prompt graph method (Prog[[2]](#2) ) into PRODIGY[[1]](#1) framework
* Ajust meta learning
* Train new GNN for meta learning
* Train new meta learning
* Put new meta learning in PRODIGY[[1]](#1) framework
* Adjust PRODIGY[[1]](#1) model
    * Original ( choose target node)
    * graph embedding + pooling
* Train PRODIGY[[1]](#1)
## Evaluation Results
![image](https://hackmd.io/_uploads/rJNFVuLKa.png)
![image](https://hackmd.io/_uploads/Sy2YE_IY6.png)
## Discussion
* Meta Learning:
    * The dimension is too large, failing to achieve the best results
        * The amount of data is too small to fully learn the knowledge.
        * Too many epochs lead to overfitting.
        * Adjust the training objectives and strategies.
    * Use a different GNN
        * Adjust the GNN
        * Directly train using the model we will ultimately use.
    * The graph is too small
        * The graph could be replaced with a larger one.
* Prodigy plus’ discussion : 
    * At the very beginning, it can be seen that having a prompt indeed helps to significantly improve scores, proving that prompts are indeed useful.
    * It is observed during training that under the conditions of 'original + prompt' and 'ux + prompt', the 'ux +prompt' performs better. This indicates that a new embedding method is indeed necessary.
    * The performance without training the prompt is better than with training the prompt. The inferred reason is similar to what was discussed earlier: our current prompt, after being trained, contains more information that is unsuitable for our model, leading to poorer results.
    * However, from the later part, we can see that 'ux + train prompt' still has a very good effect. It has the highest score among the changes made to the model and the prompt, which indicates that altering both simultaneously indeed has a synergistic effect, even though it still falls short of the original in some aspects.
    * The performance in linking is not very good overall. I speculate the reason is that our prompt is not very effective, failing to directly transform the task into a graph generation task. Therefore, a better prompt graph is needed.
* How to improve rodigy plus’ accuracy:
    * Prompt graph location
        * Adjust prompt graph location
        * Adjust meta learning token number
    * We can find a more suitable way to transform data-graph into graph embeddings.
## Conclusion
I believe my idea is feasible. Although there hasn't been significant progress in the experimental data, I am confident that if we implement the improvements mentioned in the previous pages, we should be able to achieve our goal


## References
<a id="1">[1]</a> 
Huang, Q., Ren, H., Chen, P., Kržmanc, G., Zeng, D., Liang, P., & Leskovec, J. (2023). PRODIGY: Enabling In-context Learning Over Graphs. ArXiv, abs/2305.12600.

<a id="2">[2]</a> 
Sun, X., Zhang, J., Wu, X., Cheng, H., Xiong, Y., & Li, J. (2023). Graph Prompt Learning: A Comprehensive Survey and Beyond. arXiv:2311. 16534. Retrieved from http://arxiv.org/abs/2311.16534
