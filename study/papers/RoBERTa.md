Improves [[Bert|Bert]] by modified pre-training approach while architecture remains the same. [Paper link](https://arxiv.org/abs/1907.11692) 
The differences are as follows
- training the model longer, with bigger batches, over more data
	- more epochs with bigger batches and relatively larger learning rate
- removing the next sentence prediction objective
	- even though bert mentions removing nsp prediction loss degrades the performance over various tasks, removing nsp loss along with the nsp token improves performance
- training on longer sequences 
	- unlike bert, where the segments are randomly drawn (p=0.5) with nsp token, here contiguous sentences are drawn from the same documents. sentences at the end of the document are concatenated with sentences from next document
	- an ablation study with just pair of natural sentences used, there is a degradation in performance which can be hypothesized that the model isn't learning with longer contexts from the data.
- dynamically changing the masking pattern applied to the training data
	- instead of masking tokens prior to the training phase, masking is performed before each training batch making the model learn language distribution across all words
	- more pre-training epochs are required for the model to converge

RoBERTa is trained with dynamic masking, FULL-SENTENCES without NSP loss, large mini-batches and and a larger byte-level BPE
