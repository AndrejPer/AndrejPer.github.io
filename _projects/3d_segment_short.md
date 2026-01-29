---
layout: page
title: 3D Brain Segmentation
description: Subcortical structure segmentation of 3D brain MRIs
img: assets/img/3d_brain.jpg
importance: 1
category: school
related_publications: true
---

This research was done as a part of the machine learning research scientist internship at Owkin. It focuses on probing the limits of the **nnUNet** pipeline in low-data regimes and comparing it to **SynthSeg**, a synthetic-data-based segmentation approach.

# Results

Experiments were performed using **nnUNet** and **SynthSeg**. SynthSeg is treated as an _out-of-the-box_ benchmark requiring no training, while nnUNet serves as a data-driven baseline trained with very limited annotations (5 or 30 image–mask pairs).

Quantitative results are summarized below, followed by representative visualizations. For full experimental details and extended analysis, the reader is referred to the complete report.

---

## Performance comparison

| Model    | OASIS DSC | OASIS HD95 | OASIS NSD | ADNI DSC | ADNI HD95 | ADNI NSD |
| -------- | --------: | ---------: | --------: | -------: | --------: | -------: |
| nnUNet5  |     82.53 |       5.52 |      1.97 |    68.54 |   14.43\* |   7.75\* |
| nnUNet30 |     88.97 |       1.58 |     29.15 |    84.85 |      4.72 |     2.05 |
| SynthSeg |     76.59 |       3.69 |      0.93 |    69.13 |      2.94 |     1.24 |

<div class="caption">
Performance comparison on OASIS and ADNI. * indicates one example with infinite surface distance that was excluded from the mean.
</div>

---

## nnUNet trained on 5 images

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/dice_scores_fold_0_oasis5.png" title="Dice score (nnUNet5, OASIS)" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/hausdorff95_scores_fold_0_oasis5.png" title="HD95 (nnUNet5, OASIS)" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/NSD_scores_fold_0_oasis5.png" title="NSD (nnUNet5, OASIS)" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
nnUNet trained on only 5 OASIS images. Even in the extreme low-data regime, the model captures coarse anatomical structure.
</div>

---

## nnUNet trained on 30 images

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/dice_scores_fold_0_adni30.png" title="Dice score (nnUNet30, ADNI)" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/hausdorff95_scores_fold_0_adni30.png" title="HD95 (nnUNet30, ADNI)" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/NSD_scores_fold_0_adni30.png" title="NSD (nnUNet30, ADNI)" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
nnUNet trained on 30 ADNI images. Dice scores improve substantially, while surface metrics remain less stable.
</div>

---

## SynthSeg (out-of-the-box)

<div class="row">
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/dice_scores_synthseg_adni.png" title="Dice score (SynthSeg, ADNI)" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/hausdorff95_scores_synthseg_adni.png" title="HD95 (SynthSeg, ADNI)" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/NSD_scores_synthseg_adni.png" title="NSD (SynthSeg, ADNI)" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
SynthSeg performance on ADNI. Dice scores are lower than nnUNet30, but surface metrics are more consistent and robust.
</div>

---

## Symmetry effect and qualitative analysis

<div class="row justify-content-sm-center">
  <div class="col-sm-10 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/qualitative_bad_adni.png" title="Qualitative example illustrating the symmetry trap" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
<div class="caption">
Challenging ADNI example illustrating the <em>symmetry trap</em>. While nnUNet predictions have accurate shapes, labels are mixed across hemispheres.
From left to right: ground truth, nnUNet5, nnUNet30, SynthSeg.
</div>

nnUNet cross-dataset inference reveals a strong bias toward symmetric structure placement: shapes are captured correctly, but left/right class assignment fails. Dice scores increase dramatically when labels are merged by structure or foreground, indicating preserved shape understanding but poor semantic consistency across domains.

---

<em>This page presents a curated visual summary. For full metrics, ablations, and discussion, please refer to the complete technical report.</em>
