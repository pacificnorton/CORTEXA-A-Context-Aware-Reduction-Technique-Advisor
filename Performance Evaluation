## Evaluation Metrics Definition

Predictive performance is assessed by training Logistic Regression, Decision Tree, and XGBoost classifiers on both the original and reduced datasets using stratified 5-fold cross-validation. The five required metrics (Accuracy, Precision, Recall, F1-score, ROC-AUC) are averaged and converted into an accept/reject decision using the following rule-based scoring scheme:

- **Metric-level scoring.**  
  For each of the five metrics:
  - Add 2 points if $m \ge 0.80$  
    *(*high-quality performance*).
  - Add 1 point if $0.60 \le m < 0.80$  
    *(*acceptable performance*).

- **Precision–Recall balance.**  
  If $|\mathrm{Precision} - \mathrm{Recall}| < 0.10$, add 1 point  
  *(*ensures that the reduced data do not create strong class-balance distortions*).

- **Overfitting penalty.**  
  If all five metrics exceed $0.96$, subtract $2.5$ points  
  *(*very high scores commonly indicate overfitting*). 

- **Final decision.**  
  The model is accepted if the total score is at least $3$, and rejected otherwise  
  *(*minimal level ensuring that multiple aspects of performance remain preserved*). 

Among the three classifiers, only the best-performing one contributes to the final decision.
