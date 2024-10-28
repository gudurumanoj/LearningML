- multi-dimensional Reward Model that can be used as part of a synthetic data generation pipeline to create training data
- Nemotron-4-340B-Base model and a linear layer that converts the final layer representation of the end-of-response token into five scalar values
- Given a conversation with multiple turns between user and assistant, it rates the following attributes (typically between 0 and 4) for every assistant turn
	1. **Helpfulness**: Overall helpfulness of the response to the prompt.
	2. **Correctness**: Inclusion of all pertinent facts without errors.
	3. **Coherence**: Consistency and clarity of expression.
	4. **Complexity**: Intellectual depth required to write response (i.e. whether the response can be written by anyone with basic language competency or requires deep domain expertise).
	5. **Verbosity**: Amount of detail included in the response, relative to what is asked for in the prompt.
- Suitable for **synthetic** english data generation. Requires fine-tuning to adapt it to other languages

Link to the blog [here](https://huggingface.co/nvidia/Nemotron-4-340B-Reward)
