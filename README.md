# X-MARS Reordering of the MARS Dataset for Image to Video Evaluation

This repository provides the X-MARS training and test splits based on the [MARS dataset](http://www.liangzheng.com.cn/Project/project_mars.html). Details on the X-MARS dataset can be found in my [masters thesis](https://arxiv.org/pdf/1803.08709.pdf).

If you use this work in your research, please kindly cite my thesis as

```
@article{DBLP:journals/corr/abs-1803-08709,
  author    = {Andreas Eberle},
  title     = {Pose-Driven Deep Models for Person Re-Identification},
  journal   = {CoRR},
  volume    = {abs/1803.08709},
  year      = {2018},
  url       = {http://arxiv.org/abs/1803.08709},
  archivePrefix = {arXiv},
  eprint    = {1803.08709},
  timestamp = {Wed, 11 Apr 2018 11:12:46 +0200},
  biburl    = {https://dblp.org/rec/bib/journals/corr/abs-1803-08709},
  bibsource = {dblp computer science bibliography, https://dblp.org}
}
``` 

and the work of Zheng et al. which created the MARS dataset as

```
@proceedings{zheng2016mars,
  title={MARS: A Video Benchmark for Large-Scale Person Re-identification},
  author={Zheng, Liang and Bie, Zhi and Sun, Yifan and Wang, Jingdong and Su, Chi and Wang, Shengjin and Tian, Qi},
  booktitle={European Conference on Computer Vision},
  year={2016},
  organization={Springer}
}
```


## The Idea of Image to Video Evaluation

With real-world applications of person re-id becoming more and more suitable, the amount of required data for training a re-id system becomes an important concern since labeling large amounts of images is a time-consuming and hence expensive task.
This holds especially true when working with video datasets to do re-id on tracklets instead of single images.
For example the MARS dataset providing tracklets is about 36 times larger than the [Market-1501](http://www.liangzheng.org/Project/project_reid.html) dataset, while they are both based on the same data source and MARS even contains less IDs than Market-1501.
In contrast, being able to train on a standard single-image person re-id dataset (like Market-1501) and using that network for tracklet detection (as provided by MARS) would drastically reduce the number of images to be labeled.
Therefore, cross evaluating a person detection system trained on the Market-1501 dataset with the MARS dataset could give insights into these aspects.


## The Problem with MARS

Unfortunately, while the MARS and Market-1501 datasets are based on the same data source and labels are assigned consistently, they cannot be used for cross-evaluation since their test and training sets overlap largely.
In fact, 48.3% of the MARS test set IDs are contained in the Market-1501 training set diminishing the significance of a cross evaluation.


## The X-MARS Reordering of MARS

To enable a meaningful cross-evaluation, a reordering of the MARS dataset's test and training splits called X-MARS is proposed.
Since the IDs in Market-1501 and MARS are consistent and the IDs used by MARS are a subset of the IDs used by Market-1501, it is possible to reorder the tracklets of MARS based on the train/test split of Market-1501. 
This is done by assigning all IDs of MARS (i.e. the union of the test and training IDs) which are part of the Market-1501 training set to the X-MARS training set. 
The same procedure is applied for the test set of X-MARS.

In order to ease comparability between X-MARS and MARS and allow reusing the evaluation scripts for MARS with X-MARS, the query/gallery split and the used file format are created in the same way as it was done for MARS and provided in the [info subfolder](https://github.com/andreas-eberle/x-mars/tree/master/info) of this repository.

Furthermore we provide the code for generating the training, query and gallery splits with the [x_mars_creator.py script](https://github.com/andreas-eberle/x-mars/blob/master/x_mars_creator.py). Note that the script generates csv files containing the data. The provided matlab files have been created manually from the generated csv files. Also note that the gallery split is chosen by random in the script. Therefore the gallery split will probably differ from the one provided in the files in the selected tracklets but not in the way they have been chosen.


## Getting Started

Since X-MARS is just a reordering of the MARS dataset's IDs, the same images/tracklets are used. Therefore please refer to the MARS website for the download of the [MARS dataset](http://www.liangzheng.com.cn/Project/project_mars.html) required to use X-MARS. Furthermore, Zheng et al. also provide their [evaluation code for the MARS dataset](https://github.com/liangzheng06/MARS-evaluation).

To use the X-MARS dataset simply follow the same protocol as for the MARS dataset but use the train/test splits provided in the [info subfolder](https://github.com/andreas-eberle/x-mars/tree/master/info) of this repository. For the evaluation simply replace the `info` folder from the MARS evaluation code with our `info` folder and run the normal evaluation scripts.


## State of the Art 

The following table states the current state of the art. If your results are missing, please open an issue so I can add them.

The table gives the evaluation accuracies for the Market-1501 and X-MARS datasets. Market-1501 results are provided because the models have been trained on this dataset and thus it is of interest to see the drop in accuracies when applying this model on the X-MARS dataset.

<table style="width:100%">
  <tr><th>Reference</th><th colspan="5">Market-1501</th><th colspan="5">X-MARS</th></tr>
  <tr><td></td><td>mAP</td><td>R-1</td><td>R-5</td><td>R-10</td><td>R-50</td><td>mAP</td><td>R-1</td><td>R-5</td><td>R-10</td><td>R-50</td></tr>
  <tr><td style="text-align: left;">"Pose-Driven Deep Models for Person Re-Identification", Andreas Eberle, 2018: PSE (Inception-v4)</td><td>64.9</td><td>84.4</td><td>93.1</td><td>95.2</td><td>98.4</td><td>58.7</td><td>75.3</td><td>85.6</td><td>88.6</td><td>93.9</td></tr>
  <tr><td style="text-align: left;">"Pose-Driven Deep Models for Person Re-Identification", Andreas Eberle, 2018: PSE (ResNet-50)</td><td>69.0</td><td>87.7</td><td>94.5</td><td>96.8</td><td>99.0</td><td>59.2</td><td>74.9</td><td>85.2</td><td>88.3</td><td>92.9</td></tr>
 <!-- <tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr> -->
</table> 





