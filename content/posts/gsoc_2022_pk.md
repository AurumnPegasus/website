---
title: "Google Summer of Code: 2022 PK"
date: 2022-11-17T15:08:33+05:30
draft: false
toc: false
tags:
  - DL
  - software
---

This blog is about my contributions to sktime during GSoC '22.

## Introduction

[sktime](https://github.com/sktime/sktime) is a library based on sklearn meant for time-series data and problems. It has easy to use implementations of important statistical and ML-based estimators for the purposes of classification, regression and forecasting. This summer, I joined the sktime time on the project to add DL estimators to sktime.

The main motivation for adding DL models in sktime is the recent surge in using DL networks in time series related computational problems. DL has taken over most computational problems, like NLP and CV, and in the most recent time-series forecasting competition, [M5](https://www.kaggle.com/c/m5-forecasting-accuracy), a lot of top submissions used DL models in there estimators. I wanted to support this upcoming change, and hence I wanted to contribute to sktime on this particular project.

## [#3351: Migration of sktime-dl](https://github.com/sktime/sktime/issues/3351)

[sktime-dl](https://github.com/sktime/sktime-dl) is an extension of sktime specific to DL estimators. Although sktime-dl comes under stkime, since it existed as a seperate module, the user workflow for both were not similar. sktime-dl did not have documentation with the clarity sktime had, and there were design differences which existed which made it not a simple task to just use-and-fit estimators in the workflow as one likes.

Before joining, the migration project was already underway. The first PR towards it was by Tony in [#2447 First transfer of deep learning classifiers and regressors from sktime-dl](https://github.com/sktime/sktime/pull/2447). Within it, `CNNClassifier` was the first DL-Estimator to get migrated, based on which my work was built. My first contribution towards this issue was via the [`CNNRegressor`](https://github.com/sktime/sktime/pull/2902), which was based on the `CNNNetwork` defined. In a sense, this reduced redundancies a lot, since we had to define a single network for both classifiers and regressors, and it made the workflow simpler. My first PR related to migration also required me to create `BaseDeepRegressor`, which is supposed to be similar in design to `BaseDeepClassifer` to ensure simple workflow.

Once the first migration for classifiers and regressors (along with their BaseClass designs), it was simple to migrate other estimators along the same line. Some of the other estimators I have worked on migrating are:

- CNTCClassifier: [#3171](https://github.com/sktime/sktime/pull/3171)
- FCNClassifier: [#3233](https://github.com/sktime/sktime/pull/3233)
- MLPClassifier: [#3232](https://github.com/sktime/sktime/pull/3232)
- LSTMFCNClassifier: [#3292](https://github.com/sktime/sktime/pull/3292)

Finally, to make the process of migration reproducable and easy, I wrote down the steps for a person to migrate any estimator they want (either from `sktime-dl` or one they create on their own) into `sktime`. This is present in the final issue I created related to this [#3351: Migration of sktime-dl](https://github.com/sktime/sktime/issues/3351). The process of migration now is so simple that it has been converted into a `good-firt-issue`, and can be taken up by new contributors.

On top of the migration, I worked on improving the existing implementation of DL Estimators by adding more parameters and functionality, to improve the user experience and customizability.

- Adding more features to DL Classifiers: [#2991](https://github.com/sktime/sktime/pull/2991)
- Adding GPU functionality to DL Estimators: ongoing: [#3072](https://github.com/sktime/sktime/pull/3072)
- Adding extension template for DL Classifers and Networks: ongoing: [#3433](https://github.com/sktime/sktime/pull/3433)
- Adding save/load functionality for DL Estimators: worked with [Franz Kiraly](https://github.com/fkiraly) and [Sagar Mishra](https://github.com/achieveordie):
  - Added save/load functionality for Deep Learning models: [#3128](https://github.com/sktime/sktime/pull/3128)
  - Serialization and deserialization for sktime estimators: [#27](https://github.com/sktime/enhancement-proposals/pull/27)
  - save/load aka serialization/deserialization for estimators: [#3336](https://github.com/sktime/sktime/pull/3336)
  - Save/Load Functionality for both Classical and DL Estimator: [#3425](https://github.com/sktime/sktime/pull/3425)

## [#3755: Creating Base Deep Class](https://github.com/sktime/sktime/pull/3755)

During the migration of estimators from sktime-dl to sktime, the existing class design for DL estimators was observed to be redundant. Hence, I proposed to change the design of the base classes, and introduce a new `BaseDeepEstimator`, a parent class which would be common to all DL estimators.

My initial PR related to it was in the form of a [STEP document](https://github.com/sktime/enhancement-proposals), proposing a design document of the proposed changes. Based on the conversation and ideas there, multiple iterations of changes were made to make the design clearer and better. Finally, we decided to go with the solution of creating a parent `BaseDeepEstimator`.

The proposed design structure would be

```
BaseEstimator -> BaseDeepEstimator -> BaseDeepClassifier -> CNNClassifier
```

- Design for Base Deep Class: [#26](https://github.com/sktime/enhancement-proposals/pull/26)
- Creating Base Deep Class: [#3755](https://github.com/sktime/sktime/pull/3755)

## [#3501: Adding DLForecaster to sktime](https://github.com/sktime/sktime/pull/3501)

Worked on adding DL Forecasters to sktime, with a similar structure as that of DL Classifiers and DL Regressors. In this case, since there is no pre-existing code base for DL forecasters, we will have to create DL Forecasters ourselves, roughly following the structure of other DL Estimators.

The process for creating this was a lot more theoretical than coding based. It required 2 weeks of literature review, where I tried finding papers with an existing implementation of DL Forecasters which could be adapted to sktime's code base, and also spent time just learning about forecasters in general, and what all can they do.

Finally, I started writing the code for it with a lot of help from my mentors, and the current progress is really good! At this stage, the forecaster is ready to be used, and the only thing remaining is the integration of it with the base design of DL Estimators, which requires a design decision.

In general, forecasters take input in _fit as (y, fh=None, X=None), where y is the main data we need to use for predictions, and predict, and X is the exogeneous data which may or may not be provided.

But in DL Estimators, y represents the target, and X represents the input to the model.

There is an inherent difference in what each variable means in these cases. In my current implementation of this, I have tried to adhere to both norms as much as possible (in mlp.py I have taken input as expected of a forecaster, but then converted into a datatype which is more appropriate for a DL model as source and target), but once I try to adhere to the overall structure for BaseDeep classes, it breaks since it is still checking for X and y in the context of DL models.

There is an implementation which directly works for now in the notebook, but for implementation which is consistent with the style, either we need to take a step and change how DL Forecasters work separately (not preferred) or we could create a function which does the conversion from forecasting type data to DL type data, and accept the DL type data as the input.

- Adding DLForecaster to sktime: [#3501](https://github.com/sktime/sktime/pull/3501)

## Some other PRs

Here are a list of some other PRs I have worked on during the duration of my GSoC. They did not really come into any of the 3 buckets above, but they are meaningful contributions nonetheless!

- VECM model as Forecaster: [#2829](https://github.com/sktime/sktime/pull/2829)
- predict_interval for VECM: [#2925](https://github.com/sktime/sktime/pull/2925)
- Adding solution to "no matches found" error: [#2786](https://github.com/sktime/sktime/pull/2786)
- Presented during Dev Days of sktime in Summer '22 and in Fall '22.

## Whats Next?

I plan to continue working with sktime, and work on DL estimators mostly. Main job of work would be to complete the DLForecaster, so atleast one of them is up as soon as possible. I also need to take time out to create example notebooks and documentations on how to use these DL Estimators.

## Acknowledgements

A lot of credit goes to the `sktime` community to help me throughout the summer! It was tough starting out on a huge code base like this, but they helped me out through each step and made it comfortable for me to contribute in the best manner possible. Especially I would like to thank my mentors: Franz Kiraly, Guzal Bulatova and Leonidas Tsaprounis for overlooking my project, and helping me at every step. I would also recommend you to check out projects done by other GSoC contributors in sktime: [Mirae Parker](https://miraep8.github.io/2022-09-14-GSoC-2022-Project-Summary/) and [Katie Buchhorn](https://katiebuc.github.io/gsoc/).
