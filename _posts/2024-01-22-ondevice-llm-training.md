---
title: 'Literature Notes: On-Device LLM Training'
date: 2024-01-22
permalink: /posts/2024/01/ondevice-llm-training/
tags:
  - LLM
  - on device training
  - model compression
---

I'm collecting notes from papers on the topic of training LLMs (and where possible, ML models in general) on edge devices. Edge devices are generally used for inference: most software libraries and literature aimed at on-device operation are aimed at efficient inference. Training on device is a less-developed area. Training on device is useful in applications where a model must be fine tuned for individual use cases. 

#### [Efficient Fine-Tuning of BERT Models on the Edge (2022)](https://arxiv.org/abs/2205.01541) 

##### Abstract
- Proposes Freeze and Reconfigure (FAR) approach. Increases training efficiency by reducing memory accesses in training of BERT-like models. FAR reduces training time on DistilBERT model and CoLA dataset by 30%. Reductions in metric performance on GLUE and SQuAD datasets are around 1% on average.

##### Introduction
- Training requires 3X more memory operations than inference. Especially bad since memory accesses to main memory are two orders of magnitude more energy intensive than computation. Model compression does not bridge the gap: a compressed model with 1/3 the size of the original is hard to achieve.
- During inference, only parameters and data are retrieved from memory (2 operations). During training, parameters must be retrieved at least twice and stored once, activations are stored and retrieved, data is retrieved (6 operations).   
- Two techniques to achieve on-device learning are (a) using a compressed model to reduce baseline memory requirements and (b) training in a resource-efficient manner. FAR is intended to accomplish the latter.
- FAR has two elements: (a) a novel learning metric that tracks the size of weight updates over contiguous groups of parameters in order to indicate which groups should be frozen and (b) a novel parameter freezing scheme that regroups frozen and non-frozen parameters to reduce training time.
- Claimed results: if 60% of model parameters are frozen, fine-tuning time is reduced by 30%. Metric performance is maintained.

##### Background and Related Work
- Basic building block of BERT-like models is Attention and Feed Forward Network (FFN). FAR reconfigures these blocks. This paper's proposed algorithm is intended to be used with compressed models like DistilBERT. 
- Other approaches to efficient learning train only the best-learning weights while leaving others at their initialization values. This unstructured selection of weights leads to inefficient memory accesses. 

##### Freeze and Reconfigure (FAR)
- In FAR, each FFN layer is considered a single group of parameters. These nodes are classified as learner and non-learner depending on how a node performs during fine tuning. The non-learner nodes are frozen. Reconfiguration of the FFN layers is completed during fine tuning by separating learner and non-learner nodes into distinct, newly created FFN sublayers. 
- The set of learner nodes is decided upon after an initial set of fine-tuning iterations called priming. During priming, a copy of the pre-trained weights is made and some small % of the fine tuning iterations are performed. The initial FFN weight vectors are subtracted from the fine-tuned FFN weight vectors. We compute the L1 norm to compute the total amount learned by each node in the FFN. Usage of the L1 norm allows for weights of the FFN node to be grouped so that architectural modifications can be made later for greater memory savings.
- To compute the set of learner nodes, a retention percentage is defined as the ratio of learner nodes to the total number of nodes. A more compressed model may require higher retention to avoid substantial losses in metric performance.
- FFNs in each transformer block are reconfigured to separate learner nodes from non-learner nodes. Such separation ensures that the memory of learner nodes is grouped together for optimal memory access patterns. To restore the output order of FFNs, a permutation is applied to output vectors. The combination of reconfiguration and permutation trades memory accesses for additional time spent on computation and data movement which are far less costly.
- Reconfiguration is also beneficial for practical implementation since in PyTorch, automatic gradient computation can not be disabled for single parameters but instead only whole layers or sublayers. No special hardware or libraries are required to implement FAR.

##### Experiments
- Comments: the authors compare metric performance of FAR versus random selection in Table 3. In that table, it is clear that there is not a large metric performance improvement between FAR and random selection. However, the authors do not compare fine tuning time improvement of FAR vs random selection. They claim that the memory access pattern of random selection would make it much slower (which is a believable claim) but no data is provided to substantiate this.
![fig1](/content/efficient_fine_tuning_of_bert_on_edge.png)  

#### More papers coming soon...