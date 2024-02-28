---
title: "Classical Music Classifier Project"
date: 2019-06-28
draft: false
tags: ["music", "CS", "AI"]
---

## Introduction

[This project](https://github.com/lucaspauker/music-matcher) done for my CS221 class aims to classify classical music by musical era (Baroque, Classical, Romantic, Modern) with composers as a proxy.

Using audio processing techniques, such as Short-time Fourier Transform, we extracted features such as the spectrogram and chromagram of the audio data from two datasets, `Free Music Archive` and `MAESTRO`.

We used two ensemble classifiers, AdaBoost and Random Forest, and found that although Adaboost performed marginally better than Random Forest, the latter made more generalizable predictions. Both models achieve an accuracy rate of 60% on the test data, which is significantly better than the baseline prediction of 45%. Our project reveals the complexity of the era classification task, and we expect more complex models trained on a larger data set to achieve higher success.

## Datasets
Our model was trained, developed, and tested on a combination of the following two datasets:

1. FMA (Free Music Archive): a large-scale dataset of audio files with rich metadata, containing over 100,000 tracks across 161 genres (Defferrard et al). From this dataset, we found 441 tracks that were categorized as "classical" and had a valid composer.
2. MAESTRO (MIDI and Audio Edited for Synchronous TRacks and Organization): a dataset with over 200 hours of labeled piano performances from 10 years of the International Piano-e-Competition, a piano competition that uses digital piano and records the music. This dataset contained 1282 songs that we could use in our model, making it the largest source of data for our project. Furthermore, we used the suggested 80-10-10 split, which eliminates overlap of pieces between training and test set.

In total, our dataset consisted of 1723 songs, and we sampled the first 30 seconds of each song for feature extraction because practically a classifier should be able to make predictions with a limited exposure to the data. The songs were overwhelmingly from the Romantic period, with 789 Romantic songs in our dataset. In addition, we had 541 songs from the Baroque era, 280 from the Classical period, and 113 from the Modern era. The imbalance in the dataset may pose an over-fitting problem.

## Feature Extraction
We used an audio processing library for Python called `librosa` to extract the following features using Short-time Fourier Transform from each audio file: chromagram, spectral centroid, spectral rolloff, root mean square value (RMS), and tonal centroid of a track.

- The *chromagram* is a distribution of the pitches in a track.
- The *spectral centroid* is a measure of the weighted mean of the frequencies over time in a sample of each data point.
- The *spectral rolloff* is the frequency below which 85% of the spectral energy lies. We expected this feature to be a strong factor because the most distinguishing factor across era is the distribution of spectral energy.
- The *root mean square* feature takes the root mean square value of the spectrogram, the distribution of frequency over time.
- Lastly, the *tonal centroid* meaures harmonic properties of the pitches. We also expected to see high weight on this feature because another major distinction between the music from the earlier eras (Baroque and Classical) and the later eras, (Romantic and Modern), are the extent or lack of harmony. For each of the five features, we normalized and computed the mean and standard deviation.

## Algorithms
For this project, we used two main algorithms: Adaboost and Random Forest. Both algorithms have pros and cons, which is discussed below.

**Adaboost**
AdaBoost is a boosting algorithm that iterates on the weights with multiple decision stumps to boost performance of these weak classifiers. The weights are first initialized as 1/n, with n being the size of the training set. These decision stumps are “weak” because the value of each weight is a threshold for each feature, dividing the dataset into four subsets based on the threshold value. In each iteration, the weight is updated according to the error measured by an exponential loss function.

We chose Adaboost as our preliminary model because features we are using are simple and straightforward relative to the complex nature of the audio data. In order to reconcile the simplicity of the features with the complexity of task, we chose ensemble models that use weak classifiers to model a complex system.

**Random Forest**
Random Forest is an ensemble model that combines multiple simple decisions trees for added complexity. One key strength of Random Forest is the robustness against possible overfitting: each decision tree is instantiated randomly, it is more likely to capture the variation in the data.

Another difference between Random Forest and Adaboost is that while the number of estimator parameter specifies the number of iteration in Adaboost, the number of estimator is a parameter for the number of decision trees to be constructed in a Random Forest classifier model. Instead of iterating each weight n times, Random Forest takes an average over n decision tree at the end.

##Comparison of Models
Both models were trained with the number of estimator parameter = 50, and performed moderately well with an accuracy rate of ~0.60, which is 0.15 over the threshold value (0.45) and is comparable to the current level of success with genre classification. Nevertheless, an improvement in the performance is desired 60% is barely over a majority value. Interestingly, the accuracy of models have consistently decreased from our preliminary results as we increased our data by one order of magnitude (100 -> +1700 data points). This suggests the failure of simple models in processing larger variations in a bigger data set and the necessity of more complex techniques, such as recurrent neural networks. Furthermore, while we expected Random Forest to perform better because it is more robust against over-fitting, Adaboost performed marginally better than Random Forest in both training and test set.

An examination of the weight assignment yields more insights into the performance of the two models. We observe that the weight assignment for the Adaboost is more uniform across the ten features compared to Random Forest. In fact, the weights for the Adaboost are all around 0.1 for every feature except the mean of the spectral centroid. This is consistent with our understanding of the algorithms because the Adaboost initializes the weight for each feature with 1/n = 1/10 = 0.1 and then updates the weights after each iteration. With our low number of iterations (n=50), it is understandable that the feature weights do not deviate strongly from the initial values. However, we surprisingly found that the Adaboost performs worse with a larger estimator, which suggests the inherent limitation in the algorithm to process complex data. Since the Random Forest merges observations of randomly initialized decision trees, it is more precise than the Adaboost in that it weighs stronger features more heavily.

Two notable features for the Random Forest model are the standard deviation of the spectral rolloff (=0.35, over 1/3) and the standard deviation of tonal centroid (=.21). We've seen the importance of the former feature consistently in our previous experiments. For each audio segment, (i.e. for each audio frame), spectral rolloff is the frequency at which 85% of the energy lies below the frequency, so the standard deviation of this feature reflects how the energy of a piece changes over time. Intuitively, this comes close to human "feeling" of a piece, which makes it a good feature. To illustrate, we expect the spectral energy to be more concentrated in the midrange of piano for a Baroque piece, in contrast with a modern piece, whose spectral energy is more likely to be widely dispersed throughout the possible frequency range. Similarly, the standard deviation of tonal centroid measures how harmonious the chords are in a piece, which is understandably a distinguishing factor for pieces across different eras.

## Error Analysis
First, we can see that the Adaboost classifier never predicted Classical or Modern on the test dataset, which reveals the limitation of the model based on weak classifiers. This model failed to account for the minority data of the pieces from the Classical and Modern eras and over-fitted to the more abundant data.

In contrast, the Random Forest model seems provides a more nuanced classification, and we expect that this model would outperform the Adaboost with a larger scale data. We suspect the lower performance on our test set of the Random Forest to the relatively small size of our data.

Since a near majority of the data was from the Romantic era, both models performed most successfully in predicting Romantic pieces. We expect to see a notable increase in the performance, especially in the Baroque, Classical, and Baroque data with more song data and more complex models.
