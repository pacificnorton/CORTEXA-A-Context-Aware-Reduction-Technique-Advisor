# Recommending-Context-Aware-Data-Reductions

This repository presents the technical architecture and experimentatal results of the Recommending Contex-Aware Data Reductions.
The objective is to demonstrate the design rationale, architectural components, and empirical performance of the proposed system.

---

## Project Overview

Recommending Context-Aware Data Reductions adresses the challenge of selecting the most suitable reduction technique for a specific technical context, analytical task and user priorities in terms of efficiency objectives. It combines a structures knowledge base that acts as a rule-based filtering, a clustering model to identify the reduction techniques sharing similar profiles in terms of technical performance, a Pareto function to discern the best trade-offs among objectives, and a lexicographic ranking scheme to prioritize methods according to user-defined preferences. Experimental evaluations on real-world datasets demonstrate that our approach reliably recommends the most appropriate techniques for the technical framework specified by the user, including the usage purpose, and their priorities in terms of efficiency objectives.

Key Contributions :

---

## Technical Architecture 

The solution is composed of the following primary components:

1. **User Input
   - Handles the collection of user input
   - Two types od user input :
     ° Techincal - for the properties of the dataset he wants to reduce and the analytical task as data usage objective
     ° Priority - for the ranking of the efficiency objectives desired
     
2. **Data Layer
   - Descriptive meta-data relative to data reduction techniques
   - Analytical insights derived from the scientific litterature
   - Empirical performance metrics obtained through systematic iexperimentation on synthetic datasets
     
3. **Core Processing Engine
   - Clustering for identifying the groups of reduction techniques evaluations that share similar performance profiles for a given dataset profile, defined by the technical input of the user
   - Identifying Pareto-Optimal solutions to cover the trade-offs among efficiency objectives
   - Lexicographic Selection for ranking

> A detailed schematic of the architecture is provided in the '/architecture/' folder.

---

## Experimental Setup and Results

Experiments were conducted to evaluate the system's performance on the following task:

- [Task 1: ensure exhaustive coverage of the system's defined technical scenarios]
- [Task 2: demonstrate the cross-domain generalizability of our approach through its application to heterogeneous datasets]
- [Task 3: empirically assess the pertinence of the generated reduction suggestions, both in terms of alignment with technical compatibility and preservation of task-specific performance]

### Datasets

- [Heart Attack Prediction](https://www.kaggle.com/datasets/juledz/heart-attack-prediction)
- [Hotel Booking](https://www.kaggle.com/datasets/jessemostipak/hotel-booking-demand)
- [Housing](https://www.kaggle.com/datasets/camnugent/california-housing-prices)
- [Company Bankruptcy Prediction](https://www.kaggle.com/datasets/fedesoriano/company-bankruptcy-prediction)
- [Mushrooms Classification](https://www.kaggle.com/datasets/uciml/mushroom-classification)
- [Boston House Pricing](https://www.kaggle.com/datasets/fedesoriano/the-boston-houseprice-data)
- [Students Performance](https://www.kaggle.com/datasets/spscientist/students-performance-in-exams)

### Evaluation Metrics 

Performance based scoring function
  ° input: Accuracy, Precision, Recall, F1-score, and ROC AUC
  ° output: General score to asses whether a dataset remains suitable for classification

Full details can be found in the 'EvaluationMetrics.md' file.

### Summary of Results

Full experimental details can be found in the 'FinalResults.md' file.

---

# Repository Structure
