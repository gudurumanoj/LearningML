Structuring the queries with the template with which models are trained is essential to get coherent good outputs because
- models are optimised during ift (instruction fine tuning) with a certain templatic way (to separate instructions of user, system prompt, llm's output etc)
- given two prompts with same question, one with the template with which it is trained and in normal format, the response for the former prompt will be better

Chat interfaces, convert the prompt into the template they've trained with in the background before passing the query to the llm.

When hosting the model locally, make sure to put the prompt into the model's chat template before querying it, else there will be several issues like:
- random gibberish output
- repetition in the output (it tries to fill the entire max tokens specified with the output)

When we apply chat template to our prompt for instruct model and pass it to the llm, it generates an organised output with `[INST]`and `[STOP]`tokens (this happens when there's chat template and doesn't happen when there's no template because these tokens will be present and hence the model will understand to generate a coherent output?). It also adds a token to indicate the start of the llm's response, to make sure it doesn't do something unexpected like completing/extending the user's prompt etc.

These will be part of tokenizer because the special tokens are part of it. Hence, the template is model (and may be tokenizer) dependant.

