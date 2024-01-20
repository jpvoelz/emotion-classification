# Multilabel Emotion Classification
Multi-label emotion classification using logistic regression, RNNs, and BERT

In this project completed for an Advanced NLP class as part of the Barcelona School of Economics master in Data Science Methodology, we test three different approaches for multi-label emotion classification using the GoEmotions dataset: a One-vs-Rest logistic regression classifier, an RNN classifier, and a classifier based on BERT. With the BERT model, we successfully replicate the SoA result of the original Demszky et al. (2020) paper.

The GoEmotions dataset is a hand-annotated dataset of over 58,000 Reddit comments. The comments have an average of 13 words each, with a maximum of 33 words. Each comment is assigned 28 binary labels that indicate the presence of any of 27 emotion categories or neutrality. The emotion labels capture a large number of positive, negative, and ambiguous emotion categories, from admiration to disappointment, confusion, and desire. 

<p align="center">
<img width="400" alt="amusement" src="https://github.com/jpvoelz/emotion-classification/assets/101129349/c5a763ff-55b0-496f-b42e-297c488d436d">
  <img width="400" alt="amusement" src="https://github.com/jpvoelz/emotion-classification/assets/101129349/90784f42-5ceb-4c04-bddc-00318aa353e8">
</p>

The classification task is thus a multi-label classification problem, where each data point can have more than one label, and these labels are potentially correlated with one another. We implement our three classifiers and evaluate them based on precision, recall, and F1-score for each emotion label and as a macro-average.

The baseline classifier performs well, although suffers in precision due to its tendency to predict emotions that are not recorded in the ground truth labels. The RNN model has a higher precision but lower recall than the baseline classifier.  The BERT model successfully replicates the SoA BERT results of the Demszky paper, and even exceeds it in F1-score, though this could be due to the train-test-validation split.

<p align="center">
<img width="587" alt="eval_metrics" src="https://github.com/jpvoelz/emotion-classification/assets/101129349/869c7329-1377-4100-889c-b30b4d3ad376">
</p>

In terms of error, all three models perform much better predicting some emotions versus others. For example, all models had poor performance with emotions like grief and realization, but performed well with gratitude and love. This is probably due not only to the smaller number of samples associated with certain emotions, but the fact that they have fewer characteristic tokens associated with them (for example, “thank you” is characteristic of gratitude). As suggested in Demszky et al, these emotions may be more verbally implicit and require context for interpretation.

The models have some differences in how they classify and commit errors. The baseline classifier is imprecise because it overpredicts emotions that are not present in ground truth labels. The RNN classifier, on the other hand, has lower recall and fails to predict true labels. The BERT model also overpredicts emotions that are not present, but less so than baseline and thus has better metrics overall.

The main potential source of bias in the data comes from the fact that the comments are sourced from Reddit, which skews heavily young and male. The comments are all in English, which necessarily limits the scope of the classifiers. We perform a small test for gender bias in the classifiers, but do not find convincing evidence of bias. If anything, the classifiers suffer from a lack of precision and recall that makes bias still a secondary issue at this stage.

<p align="center">
<img width="587" src="https://github.com/jpvoelz/emotion-classification/assets/101129349/6d3737a1-f866-4de7-b885-cc7b0547c5c3">
</p>

The emotion detection capability of these classifiers is necessarily limited by the fact that the comments in the GoEmotions dataset are very short and lack context. The dataset suffers from a large class imbalance, since 33% of all comments are neutral. However, during testing we found that downsampling neutral comments did not lead to higher performance. In addition, we noticed that some of the comments regardless of labeling seemed ambiguous to us – humans - and thus the subjectivity of the annotations could be a limiting factor in the performance of the classifiers.

We can further try to strengthen model performance by upsampling and increasing exposure to underrepresented emotions. One possible way would be to generate more comments using ChatGPT or another language model, but this could induce further bias in the data. Since we have noticed the dataset contains comments that are difficult to interpret or annotated un-intuitively, we fear the generation of new data would not improve performance in classification of this particular dataset.
