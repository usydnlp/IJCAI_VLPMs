# VLPMs Survey (IJCAI 2022)
This repository builds upon our recent [survey paper for **Vision-Language Pretraining Models (VLPMs)**](https://www.ijcai.org/proceedings/2022/0773.pdf) pusblished in <b>IJCAI 2022</b>. At the current initial stage, it mainly presents a structured summary of the survy and lists up the related VLPM papers with code availability, aiming to provide a general guideline and reference for future research development in VLPMs. As our future work, we will keep maintaining and adding on new VLPM related research and works. 




<h4 align="center">
  <b>Long, S., Cao, F., Han, C., & Yang, H. (2022). <br/><a href="https://www.ijcai.org/proceedings/2022/0773.pdf">Vision-and-Language Pretrained Models: A Survey</a><br/> (IJCAI 2022)</b></span>
</h4>

![image](https://github.com/usydnlp/Fantastic_VLPMs/blob/main/img/vlpm_ontology.png)

If you find any error or have any suggestion, please don't hesitate to open an issue and contact us.
If you find this repository helpful for your research or work, please kindly cite our VLPM survey paper pusblished in IJCAI 2022 using the Bibtex provided below:



```
@inproceedings{ijcai2022-773,
  title     = {Vision-and-Language Pretrained Models: A Survey},
  author    = {Long, Siqu and Cao, Feiqi and Han, Soyeon Caren and Yang, Haiqin},
  booktitle = {Proceedings of the Thirty-First International Joint Conference on
               Artificial Intelligence, {IJCAI-22}},
  publisher = {International Joint Conferences on Artificial Intelligence Organization},
  editor    = {Lud De Raedt},
  pages     = {5530--5537},
  year      = {2022},
  month     = {7},
  note      = {Survey Track}
  doi       = {10.24963/ijcai.2022/773},
  url       = {https://www.ijcai.org/proceedings/2022/0773.pdf},
}
```


# Structure of this repo
- [Vision-and-Language Pretrained Models: A Survey](#VLSurvey)
  - [Introduction](#intro)
  - [Generalized VLPM architecture](#VLSStruc)
  - [Input Encoding - Raw V/L Input and V/L Representation](#VLSIE)
  - [V and L Interaction Model (V-LIM)](#VLSVLIM)
  - [Pretraining tasks and dataset](#VLSPTD)
  - [Downstream tasks and dataset](#VLSDTD)
  - [Future Research Direction](#VLSFRD)
- [Code Availability](#Comprehensive)





<h1 id="VLSurvey">
Vision-and-Language Pretrained Models: A Survey
</h1>

<h2 id="intro">
Introduction
</h2>
So far, pretraining models have produced great success in both Computer Vision and Natural Language Processing. While CV researchers use VGG and ResNet to predict the categorical label of a given image, BERT has been used and revolutionised many NLP tasks, such as natural language inference, and reading comprehension. Motivated by this, many cross-modal Vision-Language pretraining models, i.e., VLPMs, have been designed and trained by feeding visual and linguistic contents into the multi-layer transformer.


<h2 id="VLSStruc">
Generalized VLPM architecture
</h2>

To present the review in a structured way, we generalized the VLPM into four major components, as is illustrated in the figure below (Figure 1 in our survey paper): 

<p align="center"><img src="https://github.com/usydnlp/Fantastic_VLPMs/blob/main/img/general_arc.png" alt="Generalized Architecture" width="600"/></p>

 __*1) V or L Raw Input Data*__: defines the representative raw data streams from the single modality contents, being either vision or language.
 
 __*2) V or L  Representation*__: processes the raw data input into the desired format of modality representations.
 
 __*3) V and L Interaction Model*__: enforces the cross-modal modelling between the two modality representations.
 
 __*4) V and L Representation*__: defines the possible cross-modal representations derived from the cross-modal modelling. 
 

We systematically review the VLPM architectures in terms of those four components, with main focus on multi-modal handling perspective. Based on this, we then summarise the pretraining strategies and downstream evaluation tasks accordingly.

<h2 id="VLSIE">
Input Encoding - Raw V/L Input 
</h2>

The pretraining process and downstream tasks normally reply on pairwise image-text corpus. Most VLPMs adopt the *single sentence (as **raw input for language**)* that aligns with the corresponding *single image (as **raw input for vision**)*, such as in the VisualBert (Li et al., 2019). Some specialized VLPMs apply *multiple sentences* that are semantically related to the corresponding image, e.g., the VLPM for Visual Dialogue Task (Wang et al., 2020) and the VLPM under multi-lingual settings (Fei et al., 2021), or apply *multiple images*, such as for the Vision Language Navigation (VLN) task (Majumdar et al., 202). Provided below is the examples for the V/L raw input in the existing VLPMs. 

<p align="center"><img src="https://github.com/usydnlp/Fantastic_VLPMs/blob/main/img/rawinput.png" alt="V/L Raw Input" width="900"/></p>



<h2> Input Encoding - V/L Representation </h2>

In the following table, we summarize the granularities of **V representation (visual representation)** applied by existing VLPMs, i.e. how image pixels are split into groups as visual tokens. It is an important decision of design since it decides the alignment level of cross-modal modeling in the image content, i.e. the source of cross-modal interaction. 




<table>
    <thead>
        <tr>
            <th style="width: 10%">Granularity</th>
            <th style="width: 10%">Definition</th>
            <th style="width: 25%">Intution</th>
            <th style="width: 20%">Visual Embedding</th>
            <th style="width: 20%">Spatial Position Embedding</th>
            <th>Example VLPMs</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ROI-based</td>
            <td>Object regions as visual tokens</td>
            <td>Assume most image-text pairwise data is supposed to have its text describe the salient object regions in the corresponding image </td>
            <td>Visual feature extracted by pre-trained Faster R-CNN object detector</td>
            <td>Coordinates of object regions, e.g. 5d vector (normalized ROI bbox and the fraction of image area)
</td>
            <td>Vilbert (Lu et al., 2019) </td>
        </tr>
        <tr>
            <td>Patch-based</td>
            <td>Grid-patches as visual tokens</td>
            <td rowspan=2><li>Seek for more fine-grained cross-modal alignment</li><li>No dependency on object detector (as in ROI-based VLPMs)</li><li>Efficient for end-to-end visual representation learning</li></td>
            <td rowspan=2><li>Mostly, visual feature extracted by CNN-based extractor</li><li>More recently, shallow and convolution-free embedding that utilizes simple linear projection for encoding</li></td>
            <td rowspan=2>Pixel location, e.g. 2d-aware vector for row/column number</td>
            <td>Fashionbert (Gao et al., 2020)</td>
        </tr>
        <tr>
            <td>Pixel-based</td>
            <td>Rows of pixels as visual tokens</td>
            <td>Pixel-BERT (Huang et al., 2020) </td>
        </tr>
        <tr>
            <td>Image-based</td>
            <td>Complete images as visual tokens</td>
            <td><li>Driven by multi-image based tasks such as Vision-Language Navigation (VLN)</li><li>Focus more on inter-image correlation, angle change in the images of the same scene</li></td>
            <td>Pooled visual feature from CNN model, e.g. EfficientNet</td>
            <td>Relative position of images, i.e. the elevation and heading angle relative to the agent for representing the positional relations among panorama images in VLN task
</td>
            <td>PREVALENT (Hao et al., 2020) </td>
        </tr>
    </tbody>
</table>


<h3>Intra-modality Processing</h3>

Before feeding the Bert-formatted language/visual representation into the cross-modal interaction model, some VLPMs further conduct additional **intra-modality processing** to the language or visual representation (or both). Most VLPMs adopt additional self-attention based transformer blocks as intra-modality processing. Several VLPMs also apply non-transformer processing such as using AoANet and the Visual Dictionary-based mapping. Transformer blocks provides flexibility of selecting a different number of blocks for the two modalities. In practice, the language modality normally applies more blocks than the vision modality for a better balance. 

<p align="center"><img src="https://github.com/usydnlp/Fantastic_VLPMs/blob/main/img/intra-modealprocessing.png" alt="Intra-modal Porcessing" width="900"/></p>



<h2 id="VLSVLIM">
V and L Interaction Model (V-LIM)
</h2>

So far, there are two types of mainstream VLPM model structures in the existing literature:

<p align="center"><img src="https://github.com/usydnlp/Fantastic_VLPMs/blob/main/img/ExistingVLPMmainstreams.png" alt="Intra-modal Porcessing" width="900"/></p>

- *Single-stream (1-stream)*: directly fuse the Bert-formatted language/visual representation by using the joint cross-modal encoder at the initial stage
- *Double-stream (2-stream)*: separately apply the intra-modality processing to two modalities along with a shared cross-modal encoder

However, this way of classification is mainly based on the perspective of data stream and intra-modality data handling before the cross-modal modelling. 

**In this survey, we propose three main categories with focus on the Vision-Language Interaction Modelling (V-LIM), hoping to enlighten a new perspective on cross-modal alignment learning strategy.**

<p align="center"><img src="https://github.com/usydnlp/Fantastic_VLPMs/blob/main/img/VLIMs.png" alt="3 types of V-LIMs" width="900"/></p>

**1) Self-attention-based V-LIM**: directly apply single-stream self-attention module to the modality representations for cross-modal modeling, e.g. most single-stream VLPMs and some of the double-stream VLPMs 

**2) Co-attention-based V-LIM**: decouple the intra- and cross- modal modeling processes, which keep two separate streams of transformer blocks for each modality that are entangled only at the specific cross-attention sub-layer, e.g. double-stream VLPMs

**3) VSE-based V-LIM**: simply utilize the dual-encoder structured model with Visual-Semantic-Embedding(VSE)-based cross-modal contrastive learning. It is mostly applied for retrieval task-focused VLPMs

We categorize those encoder-decoder VLPMs as either 1) or 2) or combination of both depending on the type of attention mechanism is applied between the two modalities.

The table below provides the brief summarization of the three types of V-LIMs.

<table>
    <thead>
        <tr>
            <th style="width: 10%">V-LIM</th>
            <th style="width: 30%">Cross-modal Modeling Strategy</th>
            <th style="width: 25%">Example VLPMs</th>     
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Self-attention-based</td>
            <td>Enforce simultaneous intra- and cross-modal modeling via self-attention</td>
            <td>Visualbert (Li et al., 2019) [single-stream] <br> Pixel-bert (Huang et al., 2021) [double-stream]</td>
        </tr>
        <tr>
            <td>Co-attention-based</td>
            <td><li>Decouple intra- and cross-modal interaction while having focus on both</li><li>Allow flexible design of modeling depth on separate intra- and cross-modal modeling</li></td>
            <td>Ernie-vil (Yu et al., 2021) [double-stream] <br> Lxmert (Tan and Bansal, 2019) [double-stream]</td>
        </tr>
        <tr>
            <td>VSE-based</td>
            <td><li>Eradicate the fusion-style attention-based V-LIM, which provides efficiency</li><li>Enable independent V/L representation encoding of both modalities, making pre-computation possible for more efficient retrieval</li></td>
            <td>ALIGN (Jia et al., 2021) <br> LightningDOT (Sun et al., 2021)</td>
        </tr>
    </tbody> 
</table>


<h2 id="VLSVLIM">
V and L Representation
</h2>

The design of the Vision or Language and Vision-language representation depend on the model structure whereas their usage depends on the goal of pretraining and downstream tasks.

<table>
    <thead>
        <tr>
            <th style="width: 10%">V-LIM</th>
            <th style="width: 30%">V/L Representation</th>
            <th style="width: 25%">V-L Representation</th>   
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Self-attention-based</td>
            <td>Output representation of the language or vision sequence from the joint cross-modal encoder</td>
            <td>Output representation of the global-level special token, such as [CLS]</td>
        </tr>
        <tr>
            <td>Co-attention-based</td>
            <td>Output representation of the two separate modality streams</td>
            <td>Output representation of the global-level special token of either modality stream or their aggregation</td>
        </tr>
        <tr>
            <td>VSE-based</td>
            <td>Independent V/L representation from the separate language and visual encoder</td>
            <td>Fused representation of V/L representation</td>
        </tr>
    </tbody>
</table>

<h2 id="VLSPTD">
Pretraining datasets and tasks 
</h2>

<h3>Pretraining datasets </h3>
There are several categories of pretraining dataset(s) used by the existing VLPMs.

**1) Large-scale webly collected dataset(s)**, featured with enormous size and diversity
- Conceptual Captions (CC, roughly 3M) and SBU Captions (SBU, around 1M) are the most commonly used Webly collected datasets for VLPM pretraining
- Some simple Dual-encoder collect even larger scale corpus from the web, such as WIT (400M) and ALIGN (1.8B)   
- It is found that a larger sized corpus leads to better performance in downstream transfer tasks

**2) Out-of-domain and In-domain combination**
- Commonly used combinations: CC/SBU as out-of-domain + MSCOCO (COCO)/Visual Genome (VG) as in-domain, since most downstream tasks are built on COCO/VG
- The in-domain dataset can also be task-specific datasets that specifically come from the commonly evaluated downstream tasks
- Inclusion of in-domain dataset for pretraining leads to better domain adaptation


**3) Domain/task-specific dataset only**, with solo focus on specialized tasks/domains
- Some VLPMs focus on Visual Dialogue (VD) pretrain on only the VD dataset due to the reduced suitability to the above text-image datasets regarding domain nature or data format

**4) Additional single-modality dataset** - joint pretraining with single-modal tasks on auxiliary single-modality dataset, driven by single-modality downstream goal
- Single-modality dataset, i.e. non-paired collection of image or text are used for reinforcing single-modal representations

<h3>Pretraining tasks </h3>

The table below provides the brief view of the main categories of V-L pretraining tasks applied in existing VLPMs. The detailed summarization of those pretraining tasks and also some other specific downstream-driven tasks can be found in the survey paper.

<table>
    <thead>
        <tr>
            <th style="width: 20%">Category</th>
            <th style="width: 30%">Representative Task</th>
            <th style="width: 25%">Example VLPMs</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=3>Most commonly used cross-modal pretraining tasks</td>
            <td>Cross-modal Masked Language Modeling (CMLM)</td>
            <td>Vl-bert (Su et al., 2019) <br> Pixel-bert (Huang et al., 2020)</td>            
        </tr>
        <tr>            
            <td>Cross-modal Masked Region Modeling (CMRM), with 3 objective variants</td>
            <td>Uniter (Chen et al., 2020) <br> Lxmert (Tan and Bansal,2019)</td>
        </tr>
        <tr>
            <td>Cross-modal Alignment</td>
            <td>Lightningdot (Sun et al., 2021) <br> Cookie (Wen et al., 2021)</td>
        </tr>
        <tr>
            <td>Generation goal-oriented pretraining tasks</td>
            <td>Seq2seq CMLM</td>
            <td>Unimo (Li et al., 2021) <br> Vd-bert (Wang et al., 2020)</td>
        </tr>
    </tbody>
</table>


<h2 id="VLSDTD">
Downstream tasks and dataset
</h2>

We also provide the summary of the eight commonly used Vision-language understanding tasks and two generation tasks as downstream goals. An overview of the VLPMs that work on the Vision-Language understanding is listed in the table below (Table 1 in the survey paper), which can serve as a quick access entry to the VLPM with the specific targeted goal. 

<p align="center"><img src="https://github.com/usydnlp/Fantastic_VLPMs/blob/main/img/downstreamtable.png" alt="Downstream understanding tasks" width="900"/></p>


<h2 id="VLSFRD">
Future Research Direction
</h2>

In order to advance this field, we propose several promising future directions for the VLPM:

- **VL Interaction Modeling**: Investigating how to explicitly align the embedding features between image and text so that it can learn fine-grained representations

- **VLPM Pretraining Strategy**: Exploring how the VL multi-tasking can be applied for VLPM pretraining that can generate the best transfer performance on the specific targeted domain, or even more generalizable transfer performance across different domains.

- **Training Evaluation**: The trained VLPMs is only evaluated during the downstream tasks so far. It worth to explore some metrics during the training precedure. 



<h1 id="Comprehensive">
Code availability
</h1>

<table class="tg" style="undefined;table-layout: fixed; width: 1043px">
<colgroup>
<col style="width: 33px">
<col style="width: 100px">
<col style="width: 42px">
<col style="width: 120px">
<col style="width: 646px">
<col style="width: 102px">
</colgroup>
<thead>
  <tr>
    <th class="tg-bobw"><span style="font-weight:bold">No.</span></th>
    <th class="tg-bobw"><span style="font-weight:bold">VLPM</span></th>
    <th class="tg-bobw"><span style="font-weight:bold">Year</span></th>
    <th class="tg-bobw"><span style="font-weight:bold">Pub.</span></th>
    <th class="tg-bobw" colspan="2"><span style="font-weight:bold">Code</span></th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-2b7s">1</td>
    <td class="tg-7h26"><a href="https://arxiv.org/pdf/1908.03557.pdf">VisualBERT</a></td>
    <td class="tg-2b7s">2019</td>
    <td class="tg-7zrl">arXiv</td>
    <td class="tg-7h26"><a href="https://github.com/uclanlp/visualbert">https://github.com/uclanlp/visualbert</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">2</td>
    <td class="tg-7h26"><a href="https://aclanthology.org/D19-1219.pdf">B2T2</a></td>
    <td class="tg-2b7s">2019</td>
    <td class="tg-7zrl">EMNLP</td>
    <td class="tg-7h26"><a href="https://github.com/google-research/language/tree/master/language/question_answering/b2t2">https://github.com/google-research/language/tree/master/language/question_answering/b2t2</a><span style="font-style:normal">2</span></td>
    <td class="tg-7zrl">TensorFlow</td>
  </tr>
  <tr>
    <td class="tg-2b7s">3</td>
    <td class="tg-7h26"><a href="https://aclanthology.org/D19-1514.pdf">LXMERT</a></td>
    <td class="tg-2b7s">2019</td>
    <td class="tg-7zrl">EMNLP</td>
    <td class="tg-7h26"><a href="https://github.com/airsplay/lxmert">https://github.com/airsplay/lxmert</a><span style="font-style:normal"></span></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">4</td>
    <td class="tg-7h26"><a href="https://arxiv.org/pdf/1908.02265.pdf">ViLBERT</a></td>
    <td class="tg-2b7s">2019</td>
    <td class="tg-7zrl">NeurlIPS</td>
    <td class="tg-7h26"><a href="https://github.com/jiasenlu/vilbert_beta">https://github.com/jiasenlu/vilbert_beta</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">5</td>
    <td class="tg-7h26"><a href="https://ojs.aaai.org/index.php/AAAI/article/download/7005/6859">VLP</a></td>
    <td class="tg-2b7s">2020</td>
    <td class="tg-7zrl">AAAI</td>
    <td class="tg-7h26"><a href="https://github.com/LuoweiZhou/VLP">https://github.com/LuoweiZhou/VLP</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">6</td>
    <td class="tg-7h26"><a href="https://arxiv.org/pdf/1908.08530.pdf">VL-BERT</a></td>
    <td class="tg-2b7s">2020</td>
    <td class="tg-7zrl">ICLR</td>
    <td class="tg-7h26"><a href="https://github.com/jackroos/VL-BERT">https://github.com/jackroos/VL-BERT</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">7</td>
    <td class="tg-7h26"><a href="https://arxiv.org/pdf/2004.13278.pdf">VD-BERT</a></td>
    <td class="tg-2b7s">2020</td>
    <td class="tg-7zrl">EMNLP</td>
    <td class="tg-7h26"><a href="https://github.com/salesforce/VD-BERT">https://github.com/salesforce/VD-BERT</a><span style="font-style:normal"></span></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">8</td>
    <td class="tg-7h26"><a href="https://arxiv.org/pdf/2004.14973.pdf">VLN-BERT</a></td>
    <td class="tg-2b7s">2020</td>
    <td class="tg-7zrl">ECCV</td>
    <td class="tg-7h26"><a href="https://github.com/arjunmajum/vln-bert">https://github.com/arjunmajum/vln-bert</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">9</td>
    <td class="tg-7h26"><a href="https://arxiv.org/pdf/2003.13198.pdf">InterBERT</a></td>
    <td class="tg-2b7s">2020</td>
    <td class="tg-7zrl">arXiv</td>
    <td class="tg-7h26"><a href="https://github.com/black4321/InterBERT">https://github.com/black4321/InterBERT</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">10</td>
    <td class="tg-7h26"><a href="https://arxiv.org/pdf/2006.06195.pdf?amp=1">VILLA</a></td>
    <td class="tg-2b7s">2020</td>
    <td class="tg-7zrl">NeurlIPS</td>
    <td class="tg-7h26"><a href="https://github.com/zhegan27/VILLA">https://github.com/zhegan27/VILLA</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">11</td>
    <td class="tg-7h26"><a href="https://openaccess.thecvf.com/content_CVPR_2020/papers/Zhu_ActBERT_Learning_Global-Local_Video-Text_Representations_CVPR_2020_paper.pdf">ActBERT</a></td>
    <td class="tg-2b7s">2020</td>
    <td class="tg-7zrl">CVPR</td>
    <td class="tg-7h26"><a href="https://github.com/PaddlePaddle/PaddleVideo/blob/develop/docs/en/model_zoo/multimodal/actbert.md">https://github.com/PaddlePaddle/PaddleVideo/blob/develop/docs/en/model_zoo/multimodal/actbert.md</a></td>
    <td class="tg-7zrl">Paddle Paddle</td>
  </tr>
  <tr>
    <td class="tg-2b7s">12</td>
    <td class="tg-7h26"><a href="https://openaccess.thecvf.com/content_CVPR_2020/papers/Hao_Towards_Learning_a_Generic_Agent_for_Vision-and-Language_Navigation_via_Pre-Training_CVPR_2020_paper.pdf">PREVALENT</a></td>
    <td class="tg-2b7s">2020</td>
    <td class="tg-7zrl">CVPR</td>
    <td class="tg-7h26"><a href="https://github.com/weituo12321/PREVALENT">https://github.com/weituo12321/PREVALENT</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">13</td>
    <td class="tg-7h26"><a href="https://openaccess.thecvf.com/content_CVPR_2020/papers/Lu_12-in-1_Multi-Task_Vision_and_Language_Representation_Learning_CVPR_2020_paper.pdf">12-IN-1</a></td>
    <td class="tg-2b7s">2020</td>
    <td class="tg-7zrl">CVPR</td>
    <td class="tg-7h26"><a href="https://github.com/facebookresearch/vilbert-multi-task">https://github.com/facebookresearch/vilbert-multi-task</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">14</td>
    <td class="tg-7h26"><a href="https://dl.acm.org/doi/pdf/10.1145/3397271.3401430">FashionBERT</a></td>
    <td class="tg-2b7s">2020</td>
    <td class="tg-7zrl">SIGIR</td>
    <td class="tg-7h26"><a href="https://github.com/alibaba/EasyTransfer/tree/master/scripts/fashion_bert">https://github.com/alibaba/EasyTransfer/tree/master/scripts/fashion_bert</a></td>
    <td class="tg-7zrl">Tensorflow</td>
  </tr>
  <tr>
    <td class="tg-2b7s">15</td>
    <td class="tg-7h26"><a href="https://arxiv.org/pdf/1909.11740.pdf">UNTER</a></td>
    <td class="tg-2b7s">2020</td>
    <td class="tg-7zrl">ECCV</td>
    <td class="tg-7h26"><a href="https://github.com/ChenRocks/UNITER">https://github.com/ChenRocks/UNITER</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">16</td>
    <td class="tg-7h26"><a href="https://arxiv.org/pdf/1912.02379.pdf">VisDial-BERT</a></td>
    <td class="tg-2b7s">2020</td>
    <td class="tg-7zrl">ECCV</td>
    <td class="tg-7h26"><a href="https://github.com/vmurahari3/visdial-bert">https://github.com/vmurahari3/visdial-bert</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">17</td>
    <td class="tg-7h26"><a href="https://arxiv.org/pdf/2004.06165.pdf">OSCAR</a></td>
    <td class="tg-2b7s">2020</td>
    <td class="tg-7zrl">ECCV</td>
    <td class="tg-7h26"><a href="https://github.com/microsoft/Oscar">https://github.com/microsoft/Oscar</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">18</td>
    <td class="tg-7h26"><a href="https://www.aaai.org/AAAI21Papers/AAAI-6208.YuFei.pdf">ERNIE-ViL</a></td>
    <td class="tg-2b7s">2020</td>
    <td class="tg-7zrl">AAAI</td>
    <td class="tg-7h26"><a href="https://github.com/PaddlePaddle/ERNIE/tree/repro/ernie-vil">https://github.com/PaddlePaddle/ERNIE/tree/repro/ernie-vil</a></td>
    <td class="tg-7zrl">Paddle Paddle</td>
  </tr>
  <tr>
    <td class="tg-2b7s">19</td>
    <td class="tg-7h26"><a href="https://arxiv.org/pdf/2009.04965.pdf">RVL-BERT</a></td>
    <td class="tg-2b7s">2020</td>
    <td class="tg-7zrl">IEEE ACCESS</td>
    <td class="tg-7h26"><a href="https://github.com/coldmanck/RVL-BERT">https://github.com/coldmanck/RVL-BERT</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">20</td>
    <td class="tg-7h26"><a href="https://openaccess.thecvf.com/content/CVPR2021/papers/Yu_Transitional_Adaptation_of_Pretrained_Models_for_Visual_Storytelling_CVPR_2021_paper.pdf">TAPM</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">CVPR</td>
    <td class="tg-7h26"><a href="https://github.com/JiwanChung/tapm">https://github.com/JiwanChung/tapm</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">21</td>
    <td class="tg-7h26"><a href="https://arxiv.org/abs/2101.11562">TDEN</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">AAAI</td>
    <td class="tg-7h26"><a href="https://github.com/YehLi/TDEN">https://github.com/YehLi/TDEN</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">22</td>
    <td class="tg-7h26"><a href="https://openaccess.thecvf.com/content/CVPR2021/papers/Zhang_VinVL_Revisiting_Visual_Representations_in_Vision-Language_Models_CVPR_2021_paper.pdf">VinVL</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">CVPR</td>
    <td class="tg-7h26"><a href="https://github.com/pzzhang/VinVL">https://github.com/pzzhang/VinVL</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">23</td>
    <td class="tg-7h26"><a href="https://openaccess.thecvf.com/content/CVPR2021/papers/Zhou_UC2_Universal_Cross-Lingual_Cross-Modal_Vision-and-Language_Pre-Training_CVPR_2021_paper.pdf">UC2</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">CVPR</td>
    <td class="tg-7h26"><a href="https://github.com/zmykevin/UC2">https://github.com/zmykevin/UC2</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">24</td>
    <td class="tg-7h26"><a href="https://openaccess.thecvf.com/content/CVPR2021/papers/Yang_TAP_Text-Aware_Pre-Training_for_Text-VQA_and_Text-Caption_CVPR_2021_paper.pdf">TAP</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">CVPR</td>
    <td class="tg-7h26"><a href="https://github.com/microsoft/TAP">https://github.com/microsoft/TAP</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">25</td>
    <td class="tg-7h26"><a href="https://openaccess.thecvf.com/content/CVPR2021/papers/Ni_M3P_Learning_Universal_Representations_via_Multitask_Multilingual_Multimodal_Pre-Training_CVPR_2021_paper.pdf">M3P</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">CVPR</td>
    <td class="tg-7h26"><a href="https://github.com/microsoft/M3P">https://github.com/microsoft/M3P</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">26</td>
    <td class="tg-7h26"><a href="https://aclanthology.org/2021.naacl-main.77.pdf">LightningDOT</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">ACL</td>
    <td class="tg-7h26"><a href="https://github.com/intersun/LightningDOT">https://github.com/intersun/LightningDOT</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">27</td>
    <td class="tg-7h26"><a href="https://openaccess.thecvf.com/content/CVPR2021/papers/Yang_Causal_Attention_for_Vision-Language_Tasks_CVPR_2021_paper.pdf">CATT</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">CVPR</td>
    <td class="tg-7h26"><a href="https://github.com/yangxuntu/lxmertcatt">https://github.com/yangxuntu/lxmertcatt</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">28</td>
    <td class="tg-7h26"><a href="https://openaccess.thecvf.com/content/CVPR2021/papers/Huang_Seeing_Out_of_the_Box_End-to-End_Pre-Training_for_Vision-Language_Representation_CVPR_2021_paper.pdf">SOHO</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">CVPR</td>
    <td class="tg-7h26"><a href="https://github.com/researchmm/soho">https://github.com/researchmm/soho</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">29</td>
    <td class="tg-7h26"><a href="https://openaccess.thecvf.com/content/CVPR2021/papers/Hong_VLN_BERT_A_Recurrent_Vision-and-Language_BERT_for_Navigation_CVPR_2021_paper.pdf">VLN-BERT</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">CVPR</td>
    <td class="tg-7h26"><a href="https://github.com/YicongHong/Recurrent-VLN-BERT">https://github.com/YicongHong/Recurrent-VLN-BERT</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">30</td>
    <td class="tg-7h26"><a href="https://openaccess.thecvf.com/content/CVPR2021/papers/Zhuge_Kaleido-BERT_Vision-Language_Pre-Training_on_Fashion_Domain_CVPR_2021_paper.pdf">Kaleido-BERT</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">CVPR</td>
    <td class="tg-7h26"><a href="https://github.com/mczhuge/Kaleido-BERT">https://github.com/mczhuge/Kaleido-BERT</a></td>
    <td class="tg-7zrl">TensorFlow</td>
  </tr>
  <tr>
    <td class="tg-2b7s">31</td>
    <td class="tg-zb5k"><a href="https://arxiv.org/pdf/2012.15409.pdf">Unimo</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">arXiv</td>
    <td class="tg-7h26"><a href="https://github.com/PaddlePaddle/Research/tree/master/NLP/UNIMO">https://github.com/PaddlePaddle/Research/tree/master/NLP/UNIMO</a></td>
    <td class="tg-7zrl">Paddle Paddle</td>
  </tr>
  <tr>
    <td class="tg-2b7s">32</td>
    <td class="tg-zb5k"><a href="https://arxiv.org/pdf/2103.06561.pdf">WenLan</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">arXiv</td>
    <td class="tg-7h26"><a href="https://github.com/BAAI-WuDao/BriVl">https://github.com/BAAI-WuDao/BriVl</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">33</td>
    <td class="tg-zb5k"><a href="https://arxiv.org/pdf/2108.11119.pdf">UPOC2</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">ACM International Conference on Multimedia</td>
    <td class="tg-7h26"><a href="https://github.com/syuqings/fashion-mmt">https://github.com/syuqings/fashion-mmt</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">34</td>
    <td class="tg-7h26"><a href="https://arxiv.org/pdf/2108.09105.pdf">Airbert</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">ICCV</td>
    <td class="tg-7h26"><a href="https://github.com/airbert-vln/airbert">https://github.com/airbert-vln/airbert</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
  <tr>
    <td class="tg-2b7s">35</td>
    <td class="tg-zb5k"><a href="https://arxiv.org/pdf/2102.03334v2.pdf">ViLT</a></td>
    <td class="tg-2b7s">2021</td>
    <td class="tg-7zrl">ICML</td>
    <td class="tg-7h26"><a href="https://github.com/dandelin/vilt">https://github.com/dandelin/vilt</a></td>
    <td class="tg-7zrl">PyTorch</td>
  </tr>
</tbody>
</table>



