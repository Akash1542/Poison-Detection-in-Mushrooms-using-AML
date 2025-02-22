# Refining Data Collection for Mushroom Toxicity

## Team

- Yashaswi Pandey - Point of Contact - [GitHub](https://github.com/YashaswiPandey) - https://github.com/YashaswiPandey
- William Trent Welstead - [GitHub](https://github.com/trent335) - https://github.com/trent335
- Eric Lee - [GitHub](https://github.com/scLee123) - https://github.com/scLee123
- Sanchit Tomar - [GitHub](https://github.com/UnpretentiousGeek) - https://github.com/UnpretentiousGeek
- Sai Akash Addala - [GitHub](https://github.com/Akash1542) - https://github.com/Akash1542

## Introduction

Researchers collect data on various types of wild mushrooms to predict the likelihood of a given mushroom containing harmful toxins based on its physical attributes. This data has traditionally been collected and stored into relational database models such as simple spreadsheets. Nowadays, due to the accessibility of smartphone cameras, this data is also collected and stored simply as pictures of wild mushrooms. Apps such as Mushroom Identify, Mushroom Identification, and Picture Mushroom allow scientists, hikers, explorers, and others to share their images and experience. CNN models are typically used, but these are limited by the necessity to have images. For our Machine Learning project, we are attempting to build a model which can predict whether or not a wild mushroom is poisonous based on attributes such as its height, color, terrain of location, and other physical aspects. Furthermore, our approach will be new because we also will take the additional step of comparing modeling approaches applied to both the relational spreadsheet data versus the abundance of images.

Our unique end goal is to determine whether stakeholders invested in mushroom toxicity predictions should allocate their resources towards improving traditional, spreadsheet mushroom data, or alternatively, capturing more images of wild mushrooms. We believe we will be successful because our methods go beyond toxicity predictions itself, but instead, focus on the usefulness of the specific data used to create such models. The principal stakeholders for this project are the wildlife researchers seeking to improve their methods of data collection for various mushroom-related research. This can assist studies related to biochemistry, nutrition, and psychedelics. Other noteworthy stakeholders include application developers and business owners who want to improve the predictive power of their mushroom-focused applications. Therefore, our success can lead to expedited efforts for both business and research endeavors.

The results showed that identifying poisonous mushrooms from tabular data is more accurate, we also extracted the important features used in deciding whether a mushroom is poisonous or not.

## Literature Review

With the rise of smartphones, everyone has a high quality camera on them, allowing them to click photos at a moments notice. This has allowed researchers to gather large amounts of data to train machines on, before this details of the mushroom were stored as tables and analyzed and trained upon. We aim to find the better way to figure out the dilemma of toxicity of a wild mushroom, or at least help better the way images are collected today to improve predictions.

Nusrat Jahan Pinky, S.M. Mohidul Islam, Rafia Sharmin Alice[^1],classified mushrooms using ensemble methods, bagging, boosting and random forest. Naïve Bayes and dissimilarity measure for bagging, AdaBoost for boosting and decision tree for random forests and found Random Forest to have the highest accuracy and dissimilarity-based bagging but random forest was much faster and claimed that proposed methods are robust than previously used methods like SVM.

H. Yang and Y. He[^2], used artificial neural network(ANN) to analyze visual and near infrared spectral data by principal components analysis for space clustering. The principal components were then used as inputs for a three-layer backpropagation ANN and achieved an extremely accurate model.

Anil, A., Gupta, H., & Arora, M. [^3], studied the "enzymatic browning" caused by the passage of time and attempted to prove the gradual change by classifying mushroom samples based on pattern behavior at different time intervals, support vector machine performed the best the classifying and the model could also predict weather a mushroom is fresh for consumption.

### Stakeholder Needs

- Health and Safety Authorities:
  - Need accurate models to assess toxicity of a mushroom
- Mushroom Foragers:
  - Need to be sure that the mushroom is not deadly, use of models in apps to make foraging safe
- App developers:
  - Need to use the latest and best models when developing apps so they keep users safe
- Food and Agriculture Regulatory Body:
  - Need for model to be up to standards, so it does not cause harm

## Data

[Image Dataset](https://www.kaggle.com/datasets/marcosvolpato/edible-and-poisonous-fungi)

[License](https://opendatacommons.org/licenses/odbl/1-0/)

### Image Dataset Summary

- 3401 Images
- .jpg format
- Resized to 224X224
- Augmentation applied to create 3 versions of each image

[Tabular Dataset](https://archive.ics.uci.edu/dataset/73/mushroom)

[License](https://creativecommons.org/licenses/by/4.0/legalcode)

### Tabular Dataset Summary

- 8124 rows, 22 columns
- Categorical columns
- Data in abbreviations (single letter) mapped to actual data from documentation
- Column indicating missing values

## Methods

### Transformations / Preprocessing

- Image data: Resizing, cropping, flipping, filtering, rotating
- Tabular data: Imputation for missing values, encoding categorical values

#### Exploratory Data Analysis

- Visualized distributions to find patterns
- Analysed outliers and found correlations

#### Modeling

- Tabular data: Various Classifiers and Regressors(Logistic Regression, RandomForest, SVM)
  - Initially we used all the columns which resulted in overfitting, so we used recursive feature elimination<br>
    to remove the weaker features and selected the six most important features.
- Image dataset: Used mobilnet_v2 pre-trained model as the first layer followed by our dense layer for image classification,
  hyperparameter tuning

#### Evaluation

- The models were cross-validated using k-fold-cross-validation using the weighted f1 scoring.

## Results

During the Exploratory Data Analysis, we found that certain characteristics only existed in poisonous mushrooms or were present in higher numbers in the poisonous types, some of these were certain odors, spore-print colors, mushrooms from certain habitats, gill colors.

![](assets/20240507_193604_image.png)

![](assets/20240507_193642_image.png)

![](assets/20240507_194649_image.png)

![](assets/20240507_194556_image.png)

The correlation matrix also confirmed odor, stalk-surface-above-ring, stalk-surface-below-ring, bruises, gill size, spore print color were highly correlated.

After preprocessing the data, the recursive feature elimination gave us 'odor', 'gill-size', 'gill-color', 'ring-type', 'spore-print-color', 'population' as the six features that were most important in the decision making process.

![](assets/20240507_215923_image.png)

The dataset was filtered so just these features remained and was then used in the modelling process. SGDC and SVM classifiers gave us similar F1 scores of around 0.91 while the logistic regression was close behind with an F1 score of 0.89.

![](assets/20240507_215954_image.png)

The images were augmented and three versions were created, data generators were used with batch size of 25, and these were passed through the mobilnet_v2 which is our first layer and then through a few dense layers to train the model, dropout rate of 0.3 was used after every dense layer. The resulting model was compiled with the Adam optimizer and the binary cross entropy for the loss function, this model gave us an accuracy of 0.76

In the following image we see the change in the accuracy of the training and validation data set, it shows a parallel rise for both which tells us that the model can generalize well, the closeness of the training and validation lines suggests that it is a balance fit and is not too biased.

![](assets/20240507_213236_accImage.jpg)

## Discussion

The result of the project shows that tabular data is better at predicting toxicity of mushrooms when compared to images, this could be due to the disorganized nature of the image data that might be causing our neural network to not fit it properly. If the images were subdivided into species before modeling for toxicity, it might help improve the model due to the large number of species of mushrooms present.

The stakeholder needs of the project were satisfied as we concluded that the tabular data collection is better for modelling toxicity as the models perform better on it, we also found the most important features needed for predicting toxicity.

## Limitation

-The training data only had a few species of mushrooms so it might not be able to perform as well on data related to those species not present.
<br>
<br>
-The tabular data is limited to the features present, but there could be additional features that could be good predictors that were not captured.

## Future Work

The future work includes: -

- Creating an application where the user can input images and information that corresponds to the most important features so we
  can predict toxicity of wild mushrooms.
- Adding data related to more species of mushrooms in both the tabular and image data sets.
- Classifying mushroom images by species before classifying for toxicity to see if model performance increases.

## References

[^1]: Nusrat Jahan Pinky, S.M. Mohidul Islam, Rafia Sharmin Alice, "Edibility Detection of Mushroom Using Ensemble Methods", *International Journal of Image, Graphics and Signal Processing (IJIGSP)*, Vol.11, No.4, pp. 55-62, 2019. DOI: https://doi.org/10.5815/ijigsp.2019.04.05
    
[^2]: H. Yang and Y. He, "Nondestructive Variety Discrimination of Fragrant Mushrooms Based on Vis/NIR Spectral Analysis," 2008 Congress on Image and Signal Processing, Sanya, China, 2008, pp. 64-67, doi: 10.1109/CISP.2008.627
    
[^3]: Anil, A., Gupta, H., & Arora, M. (2019). Computer vision based method for identification of freshness in mushrooms. 2019 International Conference on Issues and Challenges in Intelligent Computing Techniques (ICICT), 1, 1-4.
