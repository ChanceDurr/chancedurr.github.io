---
layout: post
title: Scalar Coupling Constant
subtitle: Predicting the Scalar Coupling Constant
gh-repo: ChanceDurr/chancedurr.github.io
gh-badge: [star, fork, follow]
tags: [machinelearning, chemistry]
comments: true
---

## What is a Scalar Coupling Constant?

NMR (Nuclear Magnetic Resonance) spectroscopy is one of the most popular tools to elucidate chemical structures of an unknown molecule in solution. In practical application, NMR can also be used to validate new structures because it provides information about scalar coupling, which is an indirect interaction of the nuclei of atoms in a magnetic field.

These scalar coupling, or J coupling constants obtained from an NMR spectrum contain information about relative bond distances and angles in a molecule. This is useful for determining the connectivity of atoms in a molecule.

For the seasoned chemist, confirming a basic chemical structure from the peaks on an NMR spectra alone is definitely possible, such as the 1H NMR spectrum for 1,1,2-trichloroethane below. The peaks below correspond to the hydrogens in the molecule and can easily be assigned.

![nmr](/img/nmr.png)

But what happens when you are dealing with more complex molecules or are working to synthesize new ones where you can’t easily determine what splitting patterns mean? How can you validate that what you’ve isolated is what you think it is?

This is where J coupling constants come in.

If you know what J coupling constant you’re looking for prior to getting the NMR spectrum, you can use that number as a confirmation that you’ve isolated your target molecule, or at the very least, confirm that you’re on the right track. Incorrect structures can be reported if there is no way to validate the coupling constants with the results found.

Therefore, software tools that can predict these constants accurately will be useful in validation of structures in practice.

In collaboration with [Elizabeth Ter Sahakyan](https://medium.com/@liztersahakyan]), this project aims to predict these scalar coupling constants using machine learning models given known properties of molecules so that they can be used in the application of research in chemistry.

<iframe src="https://giphy.com/embed/T2lUjGdArRxQs" width="500" height="280" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/breaking-bad-walter-white-bryan-cranston-T2lUjGdArRxQs">via GIPHY</a></p>

## The Dataset
We used this dataset that is part of the CHAMPS (Chemistry and Mathematics in Phase Space) Kaggle competition. The train dataset contained 4,658,147 scalar coupling observations of 85,003 unique molecules, and the test dataset contained 2,505,542 scalar coupling observations of 45,772 unique molecules. These molecules contained only the atoms: carbon (C), hydrogen (H), nitrogen (N), fluorine (F), and oxygen (O). There were 8 different types of scalar coupling: 1JHC, 1JHN, 2JHH, 2JHC, 2JHN, 3JHH, 3JHC, 3JHH. Fluorine coupling was not represented in this dataset.

Looking at the data, we can see that the train and test sets had relatively even distributions of scalar coupling type, and of the number of atoms present in each dataset. This tells us that the train data is a good enough representation of the test data in order to create a model that predicts the scalar coupling constants.

![data](/img/train_test.png)

## What are the different types of scalar coupling constants?
J coupling is an indirect interaction between the nuclear spins of 2 atoms. The number that comes before the J in the J coupling types (1J, 2J, 3J) denotes the number of bonds between the atoms that are coupling. So 1J, 2J, 3J coupling will have 1, 2, and 3 bonds between the atoms, respectively.

If we look at the distribution of the distance between atoms in the different types of coupling below, we can see that 1J has the lowest distance between atoms, and 3J has the highest distance between atoms, with 2J somewhere in the middle. The increase in bonds between atoms is observed as an increase in the distance between them and contains information that could help a model predict the J coupling constant more accurately.

The distribution of the scalar coupling constant values isolated by type also reveals that there are clear differences in the ranges that these values appear in. This gives us the insight that different molecular properties affect each type of J coupling differently and unique models should be used for all 8 coupling types found in the dataset.

![j_coup_type](/img/j_coup_type.png)

## What factors affect the scalar coupling constant?
Understanding what properties of molecules affect the scalar coupling constant is the key to training a model that can accurately predict these values for future experiments.

Some properties that affect the scalar coupling constant are:
- dihedral angles — the angle between two intersecting planes
- substituent electronegativities — the tendency of an atom to attract a shared pair of electrons)
- hybridization of atoms — contains information about the number of atoms bonded and the coordination of the molecule
- ring strain — instability in a molecule due to abnormal bonding angles found in a ring

Since our dataset doesn’t explicitly include this information, we need to engineer features that will bring information about the factors impacting coupling values into the data set.

## Feature Engineering
Our dataset was extremely limited in features, so our model was going to rely heavily on engineering a lot of new features. The important consideration here was understanding what could potentially be useful and working from principles. For instance, knowing that hybridization had an impact on the J coupling constant, we understood that the number of bonds on each atom would be an important feature to engineer for our model.

We engineered ~60 features — some based off of basic math and some with pretty dense calculations. Some of these features included:
- `distance` — the distance between the given cartesian points of each atom
- `n_bonds` — the number of bonds on a specific atom
- `mu` — the square root of the sum of the squared Cartesian values
- `delta_en` — the difference between the electronegativites in a bond

Advanced features were created with the help of a few helpful Kaggle kernels that you can view [here](https://www.kaggle.com/adrianoavelar/bond-calculaltion-lb-0-82) and [here](https://www.kaggle.com/seriousran/just-speed-up-calculate-distance-from-benchmark) and [here](https://www.kaggle.com/borisdee/predicting-mulliken-charges-with-acsf-descriptors).

## J type subsetting + Train/Validation splitting
In order to build 8 models, first we created subsets of the data containing each J type: 1JHC, 1JHN, 2JHH, 2JHC, 2JHN, 3JHH, 3JHC, 3JHH.

Then, we further split each subset into a train/val set. Since our training dataset was so big (4M+ observations), we opted for a 75/25 train/val split instead of doing a cross validation to save time in the initial phases of building a model, even though a cross validation would likely produce better results (Note: This is an ongoing project and cross validation will be used to validate the model when it is closer to final).

The training data includes more than 1 observation under each molecule_name. Because of this nature, we had to be considerate not to leak data from train molecules into the validation set. We did a `train_test_split()` on the `molecule_name` instead, and created train/val subsets with the data from each J type — now 16 subsets total!

## Modeling
Our hypothesis was that the coupling constant from each J type would be impacted differently by each feature, so we created 8 different models to get the most accurate predictions possible.

We first started with a simple Linear Regression for our baseline, but needed something with decision trees for better accuracy. We also confirmed that using 8 models was significantly better than 1 by running a test model with all of the J types in the data for comparison.

Ultimately, we used a LightGBM Regressor model for all 8 J types. We then tuned the hyperparamers, by running the LGBM Regressor with `early_stopping_rounds` and `RandomizedSearchCV` to give us the best score we could get.
Below you can find the summary of the modeling, which includes each model broken down by its respective validation score and permutation importances:

<iframe width="600" height="450" src="https://datastudio.google.com/embed/reporting/1G0evilM5HIXr6OqDmpWB9C5U7Yb8SXe9/page/kfXw" frameborder="0" style="border:0" allowfullscreen></iframe>

<iframe width="600" height="450" src="https://datastudio.google.com/embed/reporting/1KtoAwhnZ-6Ughws7DIHZAxXBRgdHY33F/page/kfXw" frameborder="0" style="border:0" allowfullscreen></iframe>

## Validation + Score
Validation error was calculated based on the log of the mean absolute error (MAE). The function below takes all models into account and returns the average score:

We also calculated separate validation scores for each model to better understand how each J type model scored. If one model was scored significantly lower than the other, this would give us the insight to potentially explore other modeling options for that coupling type. To calculate the validation score for separate J types, we used:
```python
np.log((val_pred — y_val).abs().mean())
```
For this metric, the MAE for any group has a floor of 1e-9, which means the best possible score is -20.732. The best publicly available score is currently -3.069.

## Results + Discussion
Using data that included known molecular properties of molecules, we created machine learning models to predict the scalar coupling constant of a pair of atoms. We used 8 different LightGBM Regressor models to predict the target scalar coupling constant for each type of coupling: 1JHC, 1JHN, 2JHH, 2JHC, 2JHN, 3JHH, 3JHC, 3JHH, resulting in a final score of -0.64.

We engineered ~60 features to help train our models accurately and improve the validation scores. With so many features, some were very helpful for our models, some not so much, and for the ones that did not add anything at all to our score, we discarded them. We also found that over cluttering the data with bunch of features could also hurt the score, so we removed a few of those as well.
The validation scores for each J coupling type were:
- 1JHC : 0.258
- 1JHN : -0.388
- 2JHH : -1.179
- 2JHC : -0.352
- 2JHN : -0.973
- 3JHH : -0.991
- 3JHC : -0.259
- 3JHN : -1.242

The model predicting the 3JHN coupling constants performed the best with a validation score of -1.242, and the model predicting the 1JHC coupling constants performed the worst with a validation score of 0.25.

The significant differences in validation scores between models is due to features that are unnaccounted for in our dataset that likely have a large effect on that specific type of coupling. To improve those scores, some features we can try adding are ones that take dipole interactions, magnetic shielding, and potential energy into account.

All of the models had different feature importances depending on the type of coupling. Some of the most common features with high importance for each model were the distance, mulliken_charges, and both of the bond_lengths (x and y).

Below are two shapely plots showing two different predictions. The one on top is a prediction for molecule dsgdb9nsd_133884 with 1JHC, the worst model, and the one on the bottom is for molecule dsgdb9nsd_105770 with 3JHN, the best model we have. With the 1JHC model, we predicted 90.14, when the actual value was 99.69, resulting in an error of 9.55. With the 3JHN model, we were way closer to the actual value of 0.06, with a prediction of 0.77, meaning the model was only off by 0.71.

![pred_shap](img/pred_shap.png)

## Data + Code
The data for this project was provided by CHAMPS (Chemistry and Mathematics in Phase Space) for a Kaggle competition. All code was written in python and visualizions were created using Matplotlib, ASE, Eli5, and Seaborn libraries. You find the notebooks to engineer features, create models, and visualizations in this github repo.

Since this project is part of an ongoing Kaggle competition we will continue to work toward improving the models until the competition deadline of August 28, 2019. Our team is currently in the top 50% on the public leaderboard.

## Sources
[Kaggle Competition](https://www.kaggle.com/c/champs-scalar-coupling/data?source=post_page---------------------------)

[Literature](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4218771/?source=post_page---------------------------)

[UCI Chem](https://www.chem.uci.edu/~jsnowick/groupweb/Maestro/coupling_constants.pdf)

[University of Wisconsin Chem](https://www.chem.wisc.edu/areas/reich/nmr/Notes-05-HMR-v26-part2.pdf)

[J-Coupling](https://en.wikipedia.org/wiki/J-coupling?source=post_page---------------------------)
