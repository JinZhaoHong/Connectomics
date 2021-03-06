# Abstract 
This project is Zhaohong Jin's research project at [Harvard's Visual Computing Group](https://vcg.seas.harvard.edu/) Lab supervised by [Professor Hanspeter Pfister](https://vcg.seas.harvard.edu/people) and [Donglai Wei, PhD](https://donglaiw.github.io/). It is primarily concerned with studying how to re-construct 3D shaped neurons from Electron Microscope(EM) or Computed Tomography(CT) scanned images of brain tissues. Traditionally, this is done through semantic analysis where deep learning approaches(such as 3D U-Net) predicts which class each pixel of the image belongs to. EM generated images have better qualities at the cost of time and computation power (EM only looks at a small region of the brain). CT based approaches are more scalable at the cost of low image quality. In this project, we are exploring a way to re-construct 3D shapd neurons from low quality CT images by combining semantic segmentation with a global prior on the neuron shape to fix prediction errors. This means large 3D re-construction at scale is possible.

# Background and Problem Statement
Currently the most popular way of doing volumetric segmentation on Biomedical images is through variants of [3D U-Net](https://arxiv.org/abs/1606.06650). Taking a cubic volume of the brain which has shape (x, y, z), 3D U-Net looks at each 2-D slices with sparsely annotated labels during training, and it is able to generalize to unlabelled part. The state-of-the-art 3D U-Net is developed by [Lee et.al](https://arxiv.org/abs/1706.00120), which achieves superhuman accuracy on the SNEMI3D Connectomics Challenge (we call it Superhuman U-Net). Leveraging this model, we will apply it to segment the cubic volume into blood vessels and cells (including the cell body and dendrite). The volume we get is from CT images. This means the image is not high quality, and Superhuman U-Net will make some wrong predictions which cause our reconstruction to go wrong. The solution we propose is to construct a global prior of cells such that we can combine the segmentation and the global prior to correct the reconstructed models. For the global prior, the initial solution is to predict a set of anchor points and build a spline representation of the cell body and dendrite (this goal might be updated in the future upon further study). 

# Algorithm / Model Introduction
The following picture is from [Lee's](https://arxiv.org/abs/1706.00120) paper which describes the structure of Superhuman U-Net. Refer to his paper for a detailed walk through. The novel parts are the "training data augmentation based on simulation of known types of image defects such as misalignments, missing sections, and out-of-focus dections" and the "train the convolutional net to predict affinities of voxel pairs that are relatively distant from each other, in addition to affinities of nearest neighbor voxels."

<p float="left">
  <img src="./img/superhuman_unet.png"/>
</p>

The goal of our project is to find cells and blood vessels. Therefore, instead of output affinities, we output channels where each channel represents the probabilities of a particular class(cells, blood vessels, and background). The probabilities are not normalized to 1 across channels in our current implementation. So the final layer is a sigmoid layer instead of a softmax layer. Then, we run thresholding on each channel to identify the segmentations. Finally, we are able to reconstruct rough neuron models as input to further processes such as comparing with global prior.

# Experiments 

## Dyer et.al Dataset

We test the accuracy of Superhuman U-Net on the work of [Dyer et.al](http://www.eneuro.org/content/eneuro/4/5/ENEURO.0195-17.2017.full.pdf). In this paper, they produced three datasets using synchrotron X-ray microtomography (uCT): V1 (195, 195, 65 um) and V2 (130, 130, 65 um) and test volume V3 (130, 130, 130 um). For V1 and V2, the labels are densely labelled with blood vessels and cell bodies. V3 is sparsely labelled. As a first step, we preprocess the label into three classes: blood vessels, cell bodies, and background. 

We run our Superhuman U-Net on this volume and our output has three channels(background, blood vessel, and cell bodies). To be comparable with the results in the paper, we only look at the channel for cell bodies. We then threshold the probabilities in the channel to determine which pixel belongs cell body. To evaluate the result, we use the precision and recall. First, threshold is treated as a hyperparameter to balance the precision and recall. Dyer's paper uses f1 and f2 score, defined as 

<p float="left">
  <img src="./img/fscore.png" width="200" />
</p>

where beta takes value of 1 or 2. For pixel level segmentation, f2 score produces more accurate result as claimed in the paper. Therefore, we also reproduce the f2 score. To trace the predicted cell body, we use a simply connected component algorithm to find all connected points and then compute a centroid. As a suggested threshold from the paper, if the centroid of the predicted cell is within 10um of a label, then we identify them as a match and remove them both from the dataset. The following table summarize our results. 

|    | V1(precision, recall) | V2(precision, recall) | V3(precision, recall) |
|:---|:---|:---|:---|
| Dyer |  0.86, 0.84 |  0.83, 0.76  |  0.94, 0.78 |
| Our implementation of Superhuman UNet |  0.81, 0.68 | 0.67, 0.83   | 0.78, 0.88 |

To reduce inbalance in label weights, we have to apply a weight factor alpha on the labels. For V2 result, we set alpha=5, and for V1 and V3 result, we set alpha=20


One of the drawback of Dyer's paper is decision tree is used to classify the cells and blood vessels. This is a weak algorithm and doesn't produce accurate results. The test dataset (V3) is sparsely labelled and therefore, the precision and recall score calculated doesn’t accurately reflect the how well the decision tree algorithm performs. For example, a week prediction algorithm only predicts cells that are easily identifiable, and these cells happen to be those being sparsely labelled. This will achieve a high precision score, but it’s not a good benchmark. The Superhuman UNet achieves a higher recall score on both V2 and V3, which means it correctly identifies more correct cells compared to the decision tree algorithm while also finds cells not labelled in the dataset.

<p float="left">
  <img src="./img/v1_label.png" width="200" />
  <img src="./img/v1_cell_prediction.png" width="200" />
  <img src="./img/v1_cell_threshold.png" width="200" />
  <img src="./img/v1_bv_prediction.png" width="200" />
</p>

From left to right: V1 label, V1 cell prediction, V1 cell after thresholding, V1 blood vessel prediction.

<p float="left">
  <img src="./img/v3_cell_label.png" width="400" />
  <img src="./img/v3_cell_prediction.png" width="400" />
</p>

From left to right: V3 label, V3 cell prediction.
 

## Mouse(ZeissNag) Dataset

Compared to Dyer's dataset, mouse dataset has low resolution but comes in larger volume. Achieving good results on this dataset means we can have a prediction pipeline that is more scalable because image quality has been a limiting factor. We train the Superhuman UNet in two different ways on the mouse dataset. First, we train only with the mouse dataset. Second, we train with both Dyer's dataset and the mouse dataset. We plan to use the same evaluation metrics such as f2 score. We are still in the process of fine tuning the parameters.

# Instructions

We use Odyssey to run our experiments. Some of the sample instructions are shown below.
```
python /n/coxfs01/zhjin/EM-network/script/train_superhuman_kasthuri.py -di V1_img_255.h5@/n/coxfs01/vcg_connectomics/Dyer17/data/train/V2/V2_img.h5 -dl V1_anno_dense_bv_cell_corrected.h5@V2_anno_dense_bv_cell_corrected.h5 -do /n/coxfs01/zhjin/kasthuri/result --volume-total 11000 --volume-save 2000 --volume-valid 1000 --data-shape "64,64,64" -lr 0.005 -it 1 -bn 1 -rl 2 -dr 1.5 -g 2 -j 1 -bs 10
```
The github repository for [EM](https://github.com/donglaiw/EM-network). Refer to that for installation.

# Conclusion

Further work to be done: Explore the global prior part in more detail and come up with design and implementation.


# Citation

Eva L. Dyer, William Gray Roncal, Judy A. Prasad, Hugo L. Fernandes, Doga Gürsoy, Vincent De Andrade, Kamel Fezzaa, Xianghui Xiao, Joshua T. Vogelstein, Chris Jacobsen, Konrad P. Körding and Narayanan Kasthuri(2017) Quantifying Mesoscale Neuroanatomy Using X-Ray Microtomography. eNeruo.

Kisuk Lee, Jonathan Zung, Peter Li, Viren Jain, H. Sebastian Seung(2017) Superhuman Accuracy on the SNEMI3D Connectomics Challenge. NIPS.

Özgün Çiçek, Ahmed Abdulkadir, Soeren S. Lienkamp, Thomas Brox, Olaf Ronneberger(2016) 3D U-Net: Learning Dense Volumetric Segmentation from Sparse Annotation. MICCAI

