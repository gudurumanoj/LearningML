## Prompts
- recategorised the categories (of web samples) into more suitable template (textbook/wikihow/blogpost)
- original clusters were fit into the clusters originally obtained
- filtered some clusters based on educational value and manual inspection of medium educational score valued clusters
- whole ds with the excerpts are mapped with a prompt generating func `buildprompt` with `num_proc`for parallelization. `buildprompt`builds prompts according to generation style
- `insert_extract`and `insert_context`randomly 50% of the time is used
- cluster -> build a ds with excerpts and topic class -> map with the prompt builder
- code not present for v2
## Generation
- they did some manual cleanup of commonly occuring patterns `boilerplate_cleanup.py`
- generates data using the prompts dataset built from prompts earlier on llm-slurm and pushes the data to huggingface
- utilizes huggingface modules
## fulltext_search
- indexes the cc docs on llm-slurm (a python script to do it) `index_docs.{slurm,py}`
- uses `manticore`database - it is an easy to use fast search database
- fetches topics from the 34k collected (using seed 5k and expanding it to 34k using mixtral), and runs queries over it after replacing some 
	- uses two kinds of queries (in the described order untill n_pages are hit)
		- boosted: higher importance for each token from the search query
		- match: normal search
- `hit_ids` are used to dedup while combining (and to store as well?)
## classification
- fineweb-edu classifier training code
- even this is done on slurm
## deduplication
- using minhash from [datatrove](https://github.com/huggingface/datatrove) to dedup
- `deduplicate_dataset.py`: dedup happens stage-wise
## decontamination
- to decontaminate cosmopedia dataset from the benchm

