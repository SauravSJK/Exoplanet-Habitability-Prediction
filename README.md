# Exoplanet-Habitability-Prediction
Uses the NASA Exoplanet Archive and PHL data to predict whether an exoplanet is habitable

## Introduction
This project used data available in the NASA exoplanet and planetary systems to predict whether an exoplanet is habitable. This data is raw so we have used data-cleaning techniques and approximations to get it into a usable form. We have initially used planet masses, radius, stellar flux, surface temperature, orbital period, and the distances of each planet from its star to check whether it is in the Goldilocks zone. These parameters helped in determining whether the planet lies inside the Goldilocks zone or not. But we have also collected other available data to check whether those parameters could improve the accuracy of the model. Using different models on our cleaned data, we have performed a comparative study for performance and accuracy and have presented our conclusions in the form of a graph.

The main processes that we are covering as part of this project are as follows:
1. Data cleaning and transformation
2. Analysis and data visualization
3. Applying Principal Component Analysis (PCA) for feature reduction
4. Modeling different classification algorithms
5. Result comparison and reporting

## Feature Selection
Our dataset contains features that are missing 40% of its values. Missing values can be handled by replacing them with the mean value of the column data. However, in the detection of exoplanets, this method doesn’t give great results for Mass and Radius, which are two main funda- mental properties. Since only for a relatively small fraction of exoplanets are they both available, we are computing the missing mass and radius value by using the following formulas of the Mass – Radius relation of [Chen & Kipping (2017)](https://exoplanetarchive.ipac.caltech.edu/docs/pscp_calc.html). After cleaning and preprocessing our dataset, we have 4970 data points. 57 are the observations of habitable planets, and 4913 belong to non-habitable planets.

Preliminary analysis of the data:
- The number of stars is only related to the number of planets and with the rest of the features, it has a negative correlation (and vice versa for the number of planets).
- The number of moons is an independent feature in our data set as it has no relation to any of the other features.
- The orbital period has a strong correlation with stellar effective temperature.
- Planet mass is showing a strong correlation with luminosity and temperature. This means the greater the mass, the hotter and brighter the exoplanet.
- Planets having large radii have high luminosity.

PCA Loading Vector analysis of the data:
- Number of Stars and Radius are highly correlated to each other.
- Luminosity and Surface Gravity are inversely correlated with each other.

After looking at our PCA results, we are going forward with the following features: Number of Planets, Orbital Period, Planet Radius, Planet Mass, Stellar Effective Temperature, and Stellar Surface Gravity.

## Model Building
For the classification task, we decided to go ahead with four classifiers:
1. K Nearest Neighbour(KNN)
2. Logistic Regression
3. Multi-Layer Perceptron Classifier (MLP)
4. Support Vector Machines (SVM)

Since our data is highly imbalanced (there are very few potentially habitable planets) we have used sampling techniques to resolve the class imbalance. In sampling, either data from the class which has larger values is removed - under-sampling or additional samples are added to the class with smaller values - oversampling. We have used different sampling techniques in our project to achieve the best results. The sampling techniques used are:
1. SMOTE
2. ADASYN
3. SMOTE- Tomek
4. ClusterCentroids

The data sets used include:
1. All the original features
2. The features we selected
3. Features in transformed space - Ur including all PCs
4. Features in transformed space - Ur including top 6 PCs
5. Features in transformed space - U including all PCs
6. Features in transformed space - U including top 6 PCs

Because we have used multiple classifiers with multiple sampling techniques and different data sets we have 96 rows of results with F1 score, precision, recall, AUC, and confusion matrices, all of which are attached along with our code and report in an excel file. But the most important metric is recall value here as we want to correctly get all potentially habitable exoplanets even if at the cost of false positives.

## Conclusion
Comparing the datasets, models, and samplers, we can see that the ’U - All PCs’, ’PHL - All Features’, and ’Ur - All PCs’ datasets provide the most information, the KNN model consistently get high F1 and recall scores, and the ADASYN, SMOTE, and SMOTETomek resampling methods generate the best distribution for the models. In contrast, the Logistic Regression model performs the worst with 80% recall compared to all datasets and samplers providing good recall scores throughout. The best model in our test set turns out to be the KNN model trained on ADASYN sampled U - All PCs dataset with 0.98 F1-score, 96% precision, 100% recall, and 0.98 AUC. The model also trains very quickly as it only takes 0.28 seconds for the entire run. This is the model that we propose as the solution to predicting the habitability of an exoplanet given its features in the U matrix format.
