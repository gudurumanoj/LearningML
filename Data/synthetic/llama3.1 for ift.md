[Medium Arcticle](https://towardsdatascience.com/create-a-synthetic-dataset-using-llama-3-1-405b-for-instruction-fine-tuning-9afc22fb6eef) 

## TL;DR
- Llama3.1 405B to create a dataset of Git commands
- Nvidia’s Nemotron 4 as a reward model to filter out any bad prompt/response pairs
![[Pasted image 20240812144909.png]]

## Intro

Since llama3.1 405B has good knowledge and generation capabilites, it can be used for various tasks like RAG, SFT and synthetic data generation

For specific tasks, to be cost effective, finetuning small language models is the alternative. For task specific data (because we need it for finetuning), if data isn't available, then our only alternative is to use synthetically generated data

## Method

- Topic curation
	- Prompted llama3.1 to generate topics as well (n=5)
	- Specific instructions on formatting of the given topics 
- Prompt generation
	- Different prompts for instruction and responses
	- Specific instructions on formatting the prompts
		- For instruction generation, "Write some of these instructions as if given by someone with limited knowledge of Git terminologies and knowledge, like a beginner programmer. Your response should be in a list format. The list must be without numbers. The questions/instructions should be separated by a newline character. There must be no other text than the list."
		- For response generation, "Given an question/instruction related to Git, generate a response that could be given. Keep the response on-topic, informative, concise."
- Response Filtering using [[Nemotron Reward Model]]
	- instruction/response pairs to Nemotron 4, and receive five scores ranging from 0 to 4. These five scores are _helpfulness, correctness, coherence, complexity, and verbosity_
	- filtering based on thresholds of scores (3 and 2.5 for helpfulness and verbosity)

[[Pipeline Summary]]
