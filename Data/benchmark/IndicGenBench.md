## TL;DR
Supports tasks like cross-lingual summarization, machine translation, and crosslingual question answering. Diverse set of 29 indic languages covering 13 scripts and 4 language families.
Top performing models: Largest PaLM-2 models (which are proprietary) performs the best on most tasks. However, there is a significant gap between the corresponding scores on English benchmark.

**Why an evaluation benchmark?**
- It is crucial to evaluate their capabilities across a variety of languages to uncover performance gaps and guide future research
- Various conclusions can be inferred from the scores as to what works and what doesn't
- Can understand the reasons behind the performance robustly (task specific expertise v language understanding etc)
## Data Procurement 
Manually translated existing benchmarks to data instead of adding or increasing existing benchmarks data by collecting new data 
**Why manual translation?**
- Translation-based extension of existing benchmark results in multi-way parallel data, allowing researchers to attribute performance due to task knowledge vs. language understanding, and measure cross-lingual generalization
- clean text knowledge corpus (e.g., Wikipedia) is not available making it difficult to acquire source data for annotation for many low resource languages
- By focusing only on translation quality in the target Indic languages, able to leverage the quality control that went into designing the source benchmarks 
- 28 Indic languages resulting in 32k examples
## Task descriptions
- Cross-Lingual Summarization: CROSSSUM-IN
	- CrossSum contains multi-way parallel data in 45 languages where *BBC news articles as source* in a language are paired with corresponding summaries in other languages
	- 20.3k examples across 29 Indic languages
- Machine Translation: FLORES-IN
	- human-annotated multi-way parallel machine translation (MT)
	- 58.2k examples across 29 Indic languages
- Multilingual Question-Answering: XQUAD-IN
	- multilingual reading comprehension dataset XQuAD (XQuAD is in turn derived from the SQuAD, which is English Wikipedia passage is paired with multi- ple question-answer (QA) pairs where the answers are short spans for the given passage. The authors of XQuAD collected human translations)
	- 3.3k passages and 16.6k QA pairs in 12 Indic languages
- Cross-Lingual Question-Answering: XORQA-IN
	- questions in non-English languages paired with English evidence passages and short span answers from those passages (similar to SQuAD)
	- created with the idea of developing NLP systems that can answer questions in users’ native language by refering to sources in a high-resource language, such as English, which was more likely to contain the answer due to the information scarcity of low- resources languages on the web
	- XORQA-IN-EN
		- Xx- question, En-passage, En-answer
	- XORQA-IN-XX
		- Xx- question, En-passage, Xx-answer

![[English.png]]
![[Language 1.png]]
##  Studies
- model performance on one-shot prompting
- also measure performance across language categories based on resourcedness
- evaluate the effect of number of in-context examples shown to the model as supervised data
- the effect of prompting in a higher-resource language such an English or Hindi
- also made ablation with study in few shot prompting
- performance of in-context examples v fine-tuning
## Reportings
- larger models from the same LLM family perform better
- performance for translating English to the target language (enxx) drops signif- icantly from higher to lower resourced languages whereas the performance in the xxen direction does not fall this drastically
	- This highlights that current LLMs are better at understanding than gen- eration in these lower-resourced languages
- smaller performance deltas between medium and lower resourced languages compared to higher and medium categories
	- can be attributed to many languages in the lower category being similar to Hindi and written in the same Devanagari script
- increasing the amount of supervision in terms of the in-context examples improves performance
- For languages with no supervised data, one option to improve performance is utilizing existing supervised data another language as in-context exemplars
	- Hindi in-context exemplars are much more useful for all models as compared to their English counterparts. For smaller models, performance with Hindi exemplars comes extremely close to prompting in the test language, even better sometimes
- larger model sizes, it might be better to learn in-context rather than fine-tuning the model
- For languages where text is broken into more number of tokens, fewer in-context examples can be input to the LLM during inference. This can negatively impact performance
	- **Token fertility**: average number of sub-words that a word is broken down into by the tokenizer
- model generates mixed-language output with words mixed from Hindi and also outputs incorrectly inflected forms of the main verbs in the output for languages which are written in the same script as hindi (which is devanagiri)
- in **cross-lingual summarization** model often outputs extra information that is not present in the source article
- in **translation**, information from the source sentence is missing from the generated output
- model fails to understand polysemous English words and generates translation for the incorrect sense
	- Polysemous: having multiple interpretations or meanings
## Metrics
- cross-lingual summarization and translation tasks, CROSSSUM-IN and FLORES-IN, we report **Character-F1 (ChrF)** metric since token-level metrics like **ROUGE** and **BLEU** are not reliable for low-resource languages
- to be consistent with existing literature on QA tasks, we report SQuAD-style **Token-F1** on XQUAD-IN and XORQA-IN QA tasks
## Limitations
- it misses some India-specific entities and linguistic nuances because it extends the existing benchmarks
- it might carry the bias present in the existing benchmarks

![[Dataset.png]]
