## Intro
- Synthetically generated dataset
- Used for **pre-training** (there's already extensive work happening in synthetically generating sft datasets)
- Most time spent on meticulous **prompt engineering**. Different audience and generation styles are used
- Topics are curated form various sources and from web
- $\phi$ 1.5 report that they have selected 20k topics

## Topic Curation
- Curated from various sources and from web
- Various sources
	- Topics are directly picked from Stanford courses, Khan Academy, OpenStax, and WikiHow etc
	- Since the topics and excerpts from these topics will be limited, **different audience styles** are used
- Web data
	- Contributes over 80% of the prompts from the dataset
	- Clustering of web samples into 145 clusters and asking an llm (mixtral here, 'coz its the best open source during generation of cosmopedia v1?) to annotate by finding a common topic among a small sample (n=10) from the cluster
	- Finally 112 after manual inspecting of clusters and removing them

## Prompt Engineering
- Few templates for each source were selected and used
- HuggingChat for the initial iterations on the prompts
- Generated a few hundred samples for each prompt using  [llm-swarm](https://github.com/huggingface/llm-swarm) to spot unusual patterns
	- For instance, the model used very similar introductory phrases for textbooks and frequently began stories with the same phrases, like "Once upon a time" and "The sun hung low in the sky". Explicitly asking the model to avoid these introductory statements and to be creative fixed the issue; 
	- They were still used but less frequently.
## Tech Stack
Full code [here](https://github.com/huggingface/cosmopedia)
### Topic Curation
- [ Text Clustering](https://github.com/huggingface/text-clustering/) repo to perform clustering
- Annotation using mixtral along with an educational score for the cluster
- Roughly 100k samples are used for to perform clustering yielding 145 clusters
- Then we assigned **15 million samples** to these clusters using the inference mode of [ Text Clustering](https://github.com/huggingface/text-clustering/) 
- Half of them did not fit into any cluster and were excluded from prompt creation
### Generation at scale
- [llm-swarm](https://github.com/huggingface/llm-swarm) 
	- scalable synthetic data generation tool using local LLMs or inference endpoints on the Hugging Face Hub
	- supports various inference tools like TGI, vLLM etc
### Training
- [datatrove](https://github.com/huggingface/datatrove) library for data deduplication and tokenization
	- library to process, filter and deduplicate text data at a very large scale
- [nanotron](https://github.com/huggingface/nanotron/tree/main) for model training
- [lighteval](https://github.com/huggingface/lighteval-harness) for evaluation.

## Miscellaneous
- Benchmark decontamination before benchmarking the model
	- identify potentially contaminated samples using a 10-gram overlap. After retrieving the candidates, we employ [`difflib.SequenceMatcher`](https://docs.python.org/3/library/difflib.html) to compare the dataset sample against the benchmark sample. If the ratio of `len(matched_substrings)` to `len(benchmark_sample)` exceeds 0.5, we discard the sample
	- Samples are discarded from the data which is used to benchmark the trained model
