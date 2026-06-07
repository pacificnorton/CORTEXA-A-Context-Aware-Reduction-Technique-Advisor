# CORTEXA-A-Context-Aware-Reduction-Technique-Advisor
CORTEXA (COntext-aware Reduction TEchnique Advisor) is a knowledge-based decision-support system for data-reduction-method selection.

The system assists practitioners in identifying reduction techniques that are compatible with dataset characteristics, analytical-task requirements, and user-defined optimization priorities. CORTEXA supports feature-based, instance-based, and hybrid reduction strategies and provides transparent explanations of the trade-offs associated with alternative choices.

## Motivation

Selecting an appropriate data reduction technique is a challenging decision problem.

Previous studies demonstrated that reduction-method behaviour depends on both the analytical context and the optimization objectives considered. Different reduction techniques exhibit distinct trade-offs between predictive-performance preservation, reduction effectiveness, interpretability, and computational efficiency.

CORTEXA addresses this challenge by providing a decision-support framework capable of recommending reduction methods according to dataset characteristics, analytical-task requirements, and user-defined priorities.

## System Architecture

![CORTEXA Architecture](images/Architecture.png)

CORTEXA consists of three main components:

- Knowledge Base
- Decision Engine
- User Interaction

## Repository Structure

- [Problem Formalization](1.Problem%20Formalization.md) - Contains the conceptual foundations of CORTEXA.
- [Illustrative Example](2.Illustrative%20Example.md)
- [Architecture](3.Architecture/3.1.Architecture.md) - Contains detailed descriptions of:
  - [Knowledge Base](3.Architecture/3.2.Knowledge%20Base/3.2.1.KB%20README.md)
    - [Source Knowledge](3.Architecture/3.2.Knowledge%20Base/3.2.2.Source%20Knowledge/) (Definition of the [Dataset Profiles](3.Architecture/3.2.Knowledge%20Base/3.2.2.Source%20Knowledge/KB%20SK%20Dataset%20Profiles.md) and the [Reduction Methods](3.Architecture/3.2.Knowledge%20Base/3.2.2.Source%20Knowledge/KB%20SK%20Reduction%20Methods.md) considered in this work)
    - [Artifact Knowledge](3.Architecture/3.2.Knowledge%20Base/3.2.3.Artifact%20Knowledge/) ([Synthetic Datasets Generation](3.Architecture/3.2.Knowledge%20Base/3.2.3.Artifact%20Knowledge/AK%20Synthetic%20Datasets.md), [Performance vector computation](3.Architecture/3.2.Knowledge%20Base/3.2.3.Artifact%20Knowledge/AK%20Performance%20Vector.md), [Behavioural Patterns Analysis](3.Architecture/3.2.Knowledge%20Base/3.2.3.Artifact%20Knowledge/AK%20Behavioural%20Patterns.md))
  - [Decision Engine](3.Architecture/3.3.Decision%20Engine/3.3.1.DE%20README.md)
    - [Lexicographic Selection](3.Architecture/3.3.Decision%20Engine/3.3.2.DE%20Lexicographic%20Selection.md)
    - [Explicability Module](3.Architecture/3.3.Decision%20Engine/3.3.3.DE%20Explicability%20Module.md)
    - [Resource Efficiency Indicators](3.Architecture/3.3.Decision%20Engine/3.3.4.DE%20Resource%20Efficiency.md)
- [Evaluation](4.Evaluation/) - Contains the validation studies performed on:
  - [Classification](4.Evaluation/4.1.Classification.md)
  - [Regression](4.Evaluation/4.2.Regression.md)

## Main Contributions

- Formalization of reduction-method selection as a context-aware multi-objective decision problem.
- Behavioural characterization of feature-based, instance-based, and hybrid reduction methods.
- Knowledge-based decision-support framework for reduction-method selection.
- Preference-aware recommendation through lexicographic decision analysis.
- Explainable recommendations supported by behavioural trade-off profiles.

## Publications

## Demonstration

Video:
https://vimeo.com/1144995861
