Deep Bidirectional Transformer for Language Understanding ([BERT](https://arxiv.org/abs/1810.04805))
It relies on learning language distribution using a language corpus and **fine tuning** with an additional task specific output layer (the entire model is finetuned, albeit for very small number of epochs)
More pre-training steps might be required for the model to converge

### Introduction
Language pre-training objectives can be broadly classified into two types.
- Feature based approaches
	- language model is pretrained, whose outputs alongside some hidden layers' activations are used as inputs for the fine-grained task
	 - requires task-specific architectures but has an advantage of representing the fine-tuned task better
- Fine-tuning based approaches
	- language model is pretrained which is later finetuned for fine-grained tasks
	- has an advantage of minimal additional features required for various tasks
	- sensitive to how good was the pre-training phase

![[Pasted image 20240816165116.png]]

Lot of these language models employ uni-directional training i.e, training either left to right or right to left (x to y =  y|x, termed as predicting y given only context x). There are few models which do both and use a concatenated feature vector of both contexts as the representation of language. 
**Pre-training methods** are generally **unlabelled**. Model tries to learn the language representation

### Bert approach
Uses bi-directional training: Model is allowed to predict using both the left and right context. Makes sense intuitively because words have backward dependencies as well which cannot be captured using a unidirectional modelling.

Standard bi-directional conditioning cannot be directly employed as each word can *indirectly see itself*. To alleviate it, bert employs **MLM** (masked language modelling) approach. To make model understand inter-sentence relations, they also employ **NSP** (next sentence prediction) task.

### Architecture \& Training Parameters' details
- Same as the transformer architecture from [attention is all you need](https://arxiv.org/pdf/1706.03762)
- Omitting few specific details of number of attention heads, number of transformer blocks and hyperparameters of training procedure
- Pre-training
	- batch size of 256 sequences (256 sequences * 512 tokens = 128,000 tokens/batch) for 1,000,000 steps, which is approximately 40epochs over the 3.3 billion word corpus
	- Adam with learning rate of 1e-4, β1 = 0.9, β2 = 0.999, L2 weight decay of 0.01, learning rate warmup over the first 10,000 steps, and linear decay of the learning rate
	- gelu activation layer
- Fine-tuning
	- exhaustive search over (16,32) & {5,3,2}e-05 & (2,3,4)
	- small datasets are sensitive to hyperparameters
- Loss: combination of MLM loss and NSP loss
### Input Modelling
- Segment: Each segment comprises of multiple sentences.
- Modelled as a stream of tokens with three special tokens `[CLS],[SEP]`and `[MASK]`
	- `[CLS]`: Present at the start of every stream of tokens, whose representation is used for classification tasks
	- `[SEP]`: Used as an identifier token to separate the two segments. Also present at the end of each segment
- Input token: Word token + position embedding + segment embedding
	- segment embedding is mainly useful for nsp task, where `A`and `B`is used to represent two segments
![[Pasted image 20240816161021.png]]
- Sampled such that the combined length is less than 512 tokens
### Pre-training
#### MLM
Masked language modelling, as the name suggests, is an unsupervised pre-training approach where x% (=15 here) are masked with a special token `[MASK]`which the model predicts. 
Since `[MASK]`doesn't appear during fine-tuning phase, masking process of the x% of tokens is modified as follows
- 80% of time `[MASK]`
- 10% of time - random token
- 10% of time - left as it is (this is to include a bias towards the right token)
The advantage of this procedure is that the Transformer encoder does not know which words it will be asked to predict or which have been re-placed by random words, so it is forced to keep a distributional contextual representation of every input token
Loss: soft-max cross-entropy with the pair  representation of the `[MASK]`token and the original token is used over the vocabulary
![[Pasted image 20240816163551.png]]
#### NSP
Next Sentence Prediction, is incorporated to teach the model relation between sentences
- 50% of the time two segments are chosen from the same document and are labelled with `IsNext`
- Remaining times they are randomly sampled and labelled with `NotNext`
Loss: bce with the pair  representation of the `[CLS]`token and two classes
![[Pasted image 20240816163501.png]]
### Fine-tuning
Task specific input-output pairs are constructed, alongside an additional output layer whenever necessary. All the parameters are finetuned.

At the input level, sentence A and sentence B from pre-training are analogous to
- sentence pairs in paraphrasing
- hypothesis-premise pairs in entailment
- question-passage pairs in question answering
- a degenerate text-∅ pair in text classification or sequence tagging. 

At the output level,
- the token representations are fed into an output layer for token level tasks, such as sequence tagging or question answering
- the `[CLS]` representation is fed into an output layer for classification, such as entailment or sentiment analysis

Loss computation is task specific based on final representations of tokens

### Miscellaneous 
- Large models can also be finetuned for tasks with smaller datasets, given they have good pre-trained knowledge
- Feature-based approach by extracting the activations from one or more layers without fine-tuning any parameters of BERT can also be employed in addition to fine-tuning the entire bert model
- Fine-tuning is typically very fast, so it is reasonable to simply run an exhaustive search over the parameters and choose the model that performs best on test set
