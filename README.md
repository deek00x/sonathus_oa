# Final Model Results on Test Set

Confusion Matrix:

[[14760   865] TP FP

 [   11   364]] FN TN

 Cost of False Predictions = $14,150


# Summary 

After doing some EDA, these are the key insights
- Class imbalance (59000 -ve and 1000+ve)
- NAs present in dataset
- 170 attributes / predictors 
  
This meant that I would have to
- Use stratify option when splitting data to ensure class distribution
- Fills NAs with 0, decided after doing some EDA
- Scale data
- Dimension reduction for predictors

I scale the data using `StandardScaler()` and applied `PCA` from `Scikit` learn. I choose `N_COMPONENTS=30` initially, and looked at decided to narrow it down to the first 20 components due to the explained variance. I decided to try classical machine learning methods for this and compared `RandomForestClassifier, LogisticRegression, SVC`. `LogisticRegression` performed the best. 

I decided to split the train data into train-val, and use the test data provided only for the final evaluation to prevent leakage. 

### Model Evaluation
Noticing that FN cost 50x more than FP, I decided to prioritise increasing Recall as much as possible, thus reducing the number of FNs. After training the LogisticRegression model, I used `predict_proba` to get the probabilities. I iterate from 0 to 1 for the threshold numbers, keeping track of the costs after deriving the confusion metrics and picking the threshold (0.222) with the lowest cost. 

> I noticed that I performed this evaluation wrongly on the results after the test set, and should have done it on the training set instead. However, I ran out of time and power to fix that. There was a tornado alert in Pittsburgh and we are out of power :(

In the end, the model predicted 11 False Negatives and 865 False Positives, for a total cost of $14,150. The metrics are below. While the precision score suffered, I believe the trade off is worth it for the high Recall score due to the nature of the problem.

# Metrics at Threshold = 0.2222:
Accuracy:  0.9453

Recall:    0.9707

Precision: 0.2962

F1 Score:  0.4539

ROC AUC:   0.9904


# Future Improvements

Some future improvements I will want to do given enough time are
- Explore more feature engineering and do more EDA. Having annoymized data names makes it hard to create new features that may hold more meaning and be better predictors
- Perform grid / randomized search on various models. I tried performing RandomizedSearchCV but it was prohibitively long on my laptop. 
- Find the best threshold on the training set instead of the test set (this was a mistsake on my part I had no time to fix)
- Try a neural network model



Thank you for the opportunity.