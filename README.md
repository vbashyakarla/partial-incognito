# partial-incognito
A partial implementation of LeFevre et al.'s "efficient full-domain K-anonymity" algorithm (dubbed Incognito).

## Motivation
This implementation was written for a study on the privacy (identity and membership disclosure) risks of 2021 Texas State abortion data. 

Given the privacy risks at hand, can we generalize the published data to maintain the utility of the data while preserving privacy? 

[Incognito](https://dl.acm.org/doi/10.1145/1066157.1066164) offers an efficient means of implementing $k$-anonymization via full domain generalization. 

## Data

The data being generalized here originally come from the Texas Department of Health and Human Services. The Department's 2021 Induced Terminations of Pregnancy (ITOP) [data](https://www.hhs.texas.gov/about/records-statistics/data-statistics/itop-statistics) contains two datasets of interest:

* 2021 Induced Terminations of Pregnancy by Age and County of Residence
* 2021 Induced Terminations of Pregnancy by Race/Ethnicity and County of Residence

## Incognito Background

We begin with a generalization hierarchy of the domain by which the domain is generalized. See Le Fevre et al.'s [paper](https://dl.acm.org/doi/10.1145/1066157.1066164) for details.

From the generalization hierarchy, a series of lattices representing different domain generalizations are constructed. These lattices are organized logically; the domain is increasingly generalized when traversing the lattice from bottom to top. 

Incognito leverages the generalization and (anti-)subset properites. 

* Generalization Property: Let T be a table and let P and Q be generalization nodes on the lattice such that $D_P < _D D_Q$ (i.e., Q is a descendant of P). In other words, Q is 'more generalized' than P is. Then, if T satisfies $k$-anonymity with respect to P, then T satisfies $k$-anonymity with respect to Q as well.

Pratically speaking, this means that if a node on the lattice is found to satisify the privacy criterion (e.g., 5-Anonymity), all of its descendants do so as well. Therefore, these descendant nodes need not be separately checked.

* Subset Property: Let T be a table and let Q be a set of attributes in T. Let P be a subset of Q.
If T satisfies $k$-anonymity with respect to Q, then T satisfies $k$-anonymity with respect to P.
* Anti-Subset Property: Let T be a table and let P be a set of attributes in T. Let Q be a superset of P.
If T doesn’t satisfy $k$-anonymity with respect to P, then T doesn’t satisfy $k$-anonymity with respect to Q.

Practically speaking, the (anti-)subset property allows us to navigate and prune nodes in the lattice more efficiently. For example, if a node fails to satisfy the desired privacy criterion, then a node evaluated subsequently in the algorithm that is a superset of the attributes of the original node will fail to satisfy the privacy criterion as well. 

## Partial Incognito Implementation
A Partial Incognito Implementation Guide in `R` has been prepared.

## Questions

Questions can be directed to Varoon (vkb [at] berkeley.edu)

## References

Professor Daniel Aranki Privacy Engineering Lecture Notes ("Anonymization Algorithms I")