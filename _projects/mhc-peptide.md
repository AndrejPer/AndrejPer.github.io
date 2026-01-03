---
layout: page
title: Protein binding prediction
description: Implementation of Capsule Networks for predicting MHC-peptide binding
img: assets/img/train_burst.png
importance: 1
category: fun
related_publications: true
---

### Problem description

This project presents the implementation of Capsule Networks ([CapsNet](https://arxiv.org/pdf/1710.09829)) for predicting MHC-peptide binding hit. The study covers dataset analysis, methodological explanation, experimental setup, and result evaluation. The advantages of CapsNet are explored through several experiments. This study is the implementation of [CapsNet-MHS](https://www.nature.com/articles/s42003-023-04867-2) paper.

CapsNet has been proposed as an alternative to traditional Convolutional Neural Networks (CNNs) to better capture spatial hierarchies. This project aims to implement CapsNet for MHC-peptide binding prediction, evaluating its performance against conventional deep learning models.

### Dataset Analysis

The dataset utilized is the <em>NetMHC</em> dataset. It comprises over 3.6 million peptide-allele pairs labeled for MHC class I binding. The distribution of alleles is heavily imbalanced.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/allele_train_distr.png" title="Allele frequency distribution in the training set" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/train_burst.png" title="Hierarchical distribution of alleles" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: Allele frequency distribution in the training set. Right: Hierarchical distribution of alleles.
</div>

#### Challenges

- Extreme class imbalance (94.6% non-binding, 5.4% binding)
- Imbalanced allele frequency and presence of unseen alleles in test set
- Need for robust embeddings (e.g., using BLOSUM)

### Network Architecture

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/architecture.png" title="Network architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Network architecture from reference {% cite kalemati2023capsnet %}.
</div>

### How did I do it

Capsule Networks represent local features using vectors instead of scalars. Each vector's norm indicates probability, while the direction captures spatial relationships. Inputs $u_i$ are transformed via matrices $W_{ij}$, producing $$\hat{u}_{j \mid i} = W_{ij} u_i$$. Outputs are computed as:

$$ s_j = \sum_i c_{ij} \hat{u}_{j|i} $$

and the final capsule output is:

$$ v_j = \frac{||s_j||^2}{1 + ||s_j||^2} \cdot \frac{s_j}{||s_j||} $$

#### Implementation Details

- Sigmoid output for binary classification
- Binary Cross Entropy (BCE) loss used
- Alleles represented using amino acid sequences
- Input features encoded using [BLOSUM](https://en.wikipedia.org/wiki/BLOSUM) matrices

#### Training

The model was trained using the Adam optimizer with a learning rate of $10^{-6}$. Although the learning rate might appear small, the batch size used is rather big. The training setup included:

- BLOSUM45, 62, and 80 embeddings compared
- Trained for 10 epochs initially, then retrained on the whole training set with BLOSUM62 for 30 epochs
- Used weighted BCE to compensate for the imbalance
- Used <code>vast.ai</code> for training due to computational cost
- Batch size: 512 (train), 1024 (validation)
- Evaluating with AUC PR, since properly classifying the positive class is more important than being able to distinguish between the classes, which is modeled by AUC ROC

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/loss_area_29.png" title="Training loss for BLOSUM62" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Training and validation loss, AUC Precision-Recall, and Matthew's correlation coefficient curves for BLOSUM62 embedding during training.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/pr_curve_29.png" title="PR curve for BLOSUM62 after 30 epochs" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Final precision-recall (PR) curve for the model with BLOSUM62 embedding after 30 epochs.
</div>

### Results

| BLOSUM Matrix | AUC-ROC |
| ------------- | ------- |
| BLOSUM45      | 0.8407  |
| BLOSUM62      | 0.8375  |
| BLOSUM80      | 0.8407  |

_Table 1:_ AUC-ROC values on validation set

| Metric   | Value |
| -------- | ----- |
| AUC PR   | 0.370 |
| MCC      | 0.333 |
| Accuracy | 0.798 |
| F1-Score | 0.306 |

_Table 2:_ Test set performance metrics for the final model trained for 30 epochs.

| Metric   | Value |
| -------- | ----- |
| AUC PR   | 0.374 |
| MCC      | 0.339 |
| Accuracy | 0.805 |
| F1-Score | 0.314 |

_Table 3:_ Validation set performance metrics for the final model trained for 30 epochs.

#### Discussion

CapsNet-MHC showed strong results on imbalanced data. An alternative NLP-based approach using fastText-like embeddings was tested but was computationally prohibitive due to massive training pairs.

### Conclusion

Capsule Networks effectively capture peptide-allele interactions. While promising, further tuning and training on all folds is necessary to realize full potential. Future efforts should focus on hyperparameter tuning, reconsidering the attention approach and deeper analysis of capsule outputs for the purpose of explainability.
