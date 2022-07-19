# Fantastic VLPMs
This repository builds upon our recent [survey paper for **Vision-Language Pretraining Models (VLPMs)**](https://arxiv.org/pdf/2204.07356.pdf) pusblished in IJCAI 2022. It contains a list of papers (codes if available) and pretraining/downstream task datasets for the existing VLPMs, currently with main focus on *Text* and *Image*. We will keep maintaining and adding on new VLPM research and works. 

<h4 align="center">
  <b>Long, S., Cao, F., Han, C., & Yang, H. (2022). <br/><a href="https://arxiv.org/pdf/2204.07356.pdf">Vision-and-Language Pretrained Models: A Survey</a><br/> (IJCAI 2022), pp. XXX</b></span>
</h4>

If you find any error or have any suggestion, please don't hesitate to open an issue and contact us.
If you find this repository helpful for your research or work, please kindly cite our VLPM survey paper pusblished in IJCAI 2022 using the Bibtex provided below:

```
@article{long2022vision,
  title={Vision-and-Language Pretrained Models: A Survey},
  author={Long, Siqu and Cao, Feiqi and Han, Soyeon Caren and Yang, Haiqing},
  journal={arXiv preprint arXiv:2204.07356},
  year={2022}
}
```

# Introduction
So far, pretraining models have produced great success in both Computer Vision and Natural Language Processing. While CV researchers use VGG and ResNet predict the categorical label of a given image, BERT has been used and revolutionised many NLP tasks, such as natural language inference, and reading comprehension. Motivated by this, many cross-modal Vision-Language pretraining models, i.e., VLPMs, have been designed and trained by feeding visual and linguistic contents into the multi-layer transformer.

This repo builds on our survey and systematically organizes the existing VLPMs in terms of related papers (with their codes if available), and the commonly used pretraining and downstream task datasets, aiming to provide a general guideline and reference for future research development in VLPMs. 

# Generalized VLPMs architecture
To present the review in a structured way, we generalized the VLPM into four major components: 
- *V or L Raw Input Data*: defines the representative raw data streams from the single modality contents, being either vision or language.
- *V or L  Representation*: processes the raw data input into the desired format of modality representations.
- *V and L Interaction Model*: enforces the cross-modal modelling between the two modality representations.
- *V and L Representation*: defines the possible cross-modal representations derived from the cross-modal modelling. 

We systematically review the VLPM architectures in terms of those four components, with main focus on multi-modal handling perspective. Based on this, we then summarise the pretraining strategies and downstream evaluation tasks accordingly.


# Structure of this repo
- [Vision-and-Language Pretrained Models: A Survey](#VLSurvey)
  - [Generalized VLPM structure](#VLSStruc)
  - [Input Encoding](#VLSIE)
  - [V-LIM](#VLSVLIM)
  - [Pretraining tasks and dataset](#VLSPTD)
  - [Downstream tasks and dataset](#VLSDTD)
  - [Future Research Direction](#VLSFRD)
- [Comprehensive Overview](#Comprehensive)
  - Paper with code (only listed if available) ordered by year
  - Paper with methodlogies - Table 1 in the journal version
  - Paper with tasks and datasets - Table 2 in the journal version (add downstream tasks?)



<h1 id="VLSurvey">
Vision-and-Language Pretrained Models: A Survey
</h1>

<h2 id="VLSStruc">
Generalized VLPM structure
</h2>

<h2 id="VLSIE">
Input Encoding
</h2>

<h2 id="VLSVLIM">
V-LIM
</h2>

<h2 id="VLSPTD">
Pretraining tasks and dataset
</h2>

<h2 id="VLSDTD">
Downstream tasks and dataset
</h2>

<h2 id="VLSFRD">
Future Research Direction
</h2>

<h1 id="Comprehensive">
Comprehensive Overview
</h1>

<h2 id="CompCode">
Code availability overview
</h2>

<h2 id="CompMethod">
Methodology summary overview
</h2>

<h2 id="CompTaskData">
Tasks and dataset summary overview
</h2>


![image](https://github.com/usydnlp/Fantastic_VLPMs/blob/main/img/vlpm_ontology.png)
