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

![gif](http://giphygifs.s3.amazonaws.com/media/T2lUjGdArRxQs/giphy.gif)

## The Dataset
We used this dataset that is part of the CHAMPS (Chemistry and Mathematics in Phase Space) Kaggle competition. The train dataset contained 4,658,147 scalar coupling observations of 85,003 unique molecules, and the test dataset contained 2,505,542 scalar coupling observations of 45,772 unique molecules. These molecules contained only the atoms: carbon (C), hydrogen (H), nitrogen (N), fluorine (F), and oxygen (O). There were 8 different types of scalar coupling: 1JHC, 1JHN, 2JHH, 2JHC, 2JHN, 3JHH, 3JHC, 3JHH. Fluorine coupling was not represented in this dataset.
Looking at the data, we can see that the train and test sets had relatively even distributions of scalar coupling type, and of the number of atoms present in each dataset. This tells us that the train data is a good enough representation of the test data in order to create a model that predicts the scalar coupling constants.

## What are the different types of scalar coupling constants?
J coupling is an indirect interaction between the nuclear spins of 2 atoms. The number that comes before the J in the J coupling types (1J, 2J, 3J) denotes the number of bonds between the atoms that are coupling. So 1J, 2J, 3J coupling will have 1, 2, and 3 bonds between the atoms, respectively.
If we look at the distribution of the distance between atoms in the different types of coupling below, we can see that 1J has the lowest distance between atoms, and 3J has the highest distance between atoms, with 2J somewhere in the middle. The increase in bonds between atoms is observed as an increase in the distance between them and contains information that could help a model predict the J coupling constant more accurately.
The distribution of the scalar coupling constant values isolated by type also reveals that there are clear differences in the ranges that these values appear in. This gives us the insight that different molecular properties affect each type of J coupling differently and unique models should be used for all 8 coupling types found in the dataset.

![data](/img/1*321TqE0ajCpFRDfSloNKzg.png)
