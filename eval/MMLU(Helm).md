- Standardised prompts
- Accuracy 
### General issues
- reported MMLU scores using non-standard prompting techniques and using some specialised methods (like CoT, few-shot, self-correct etc). These can lead to improvement in eval results because of the methods and not because of model understanding leading to discrepancy in eval results
- insufficient information about prompting templates and in-context examples used. Even this is not desired because model predictions are sensitive to the prompts used
- using internal private data for eval
- not using same "state" for eval because of which results cannot be directly shared across papers (for ex, if everyone uses llm-eval-harness, then one can just borrow the results from other papers without having to re-run benchmarks across models)
### HELM
- Uses standardised prompts, accuracy metric
- expects the model to output the string \<option> to evaluate based on string match instead of the other widely used "token probability" based metrics because some apis do not reveal token probabilities
- Instruction-following tuning prevented some models from following in-context learning prompts. For instance, with regular in-context learning prompts, Claude 3 would respond with complete sentences rather than with a single letter as desired. To deal with this, we had to add an additional instruction to Claude 3: “Answer with only a single letter.”
