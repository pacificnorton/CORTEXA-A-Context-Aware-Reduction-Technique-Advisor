# CORTEXA-A-Context-Aware-Reduction-Technique-Advisor
CORTEXA (COntext-aware Reduction TEchnique Advisor) is a knowledge-based decision-support system for data-reduction-method selection.

The system assists practitioners in identifying reduction techniques that are compatible with dataset characteristics, analytical-task requirements, and user-defined optimization priorities. CORTEXA supports feature-based, instance-based, and hybrid reduction strategies and provides transparent explanations of the trade-offs associated with alternative choices.

## Motivation

Selecting an appropriate data reduction technique is a challenging decision problem.

Previous studies demonstrated that reduction-method behaviour depends on both the analytical context and the optimization objectives considered. Different reduction techniques exhibit distinct trade-offs between predictive-performance preservation, reduction effectiveness, interpretability, and computational efficiency.

CORTEXA addresses this challenge by providing a decision-support framework capable of recommending reduction methods according to dataset characteristics, analytical-task requirements, and user-defined priorities.

## System Architecture

![CORTEXA Architecture](Architecture/Figures/CORTEXA-Architecture.png)

CORTEXA consists of three main components:

- Knowledge Base
- Decision Engine
- User Interaction

## Repository Structure

- Methodology - Contains the conceptual foundations of CORTEXA.
- Architecture - Contains detailed descriptions of:
  - Knowledge Base
  - Decision Engine
  - User Interaction
- Experiments - Contains the validation studies performed on:
  - Classification tasks
  - Regression tasks
- Demonstration scenarios
- Publications - Contains the papers associated with the project.

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
