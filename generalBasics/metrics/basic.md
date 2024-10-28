### Basic Def
- **Precision**: Measures how precise/accurate your model is. It is the ratio between the correctly identified positives (true positives) and all identified positives. The precision metric reveals how many of the predicted entities are correctly labeled.
    
    `Precision = #True_Positive / (#True_Positive + #False_Positive)`
    
- **Recall**: Measures the model's ability to predict actual positive classes. It is the ratio between the predicted true positives and what was actually tagged. The recall metric reveals how many of the predicted entities are correct.
    
    `Recall = #True_Positive / (#True_Positive + #False_Negatives)`
    
- **F1 score**: The F1 score is a function of Precision and Recall. It's needed when you seek a balance between Precision and Recall.
    
    `F1 Score = 2 * Precision * Recall / (Precision + Recall)`
**Note**: Precision, recall and F1 score are calculated for each entity separately (_entity-level_ evaluation) and for the model collectively (_model-level_ evaluation)
