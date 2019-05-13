# Abstract 
This project is Zhaohong Jin's research project at [Harvard's Visual Computing Group](https://vcg.seas.harvard.edu/) Lab supervised by [Professor Hanspeter Pfister](https://vcg.seas.harvard.edu/people) and [Donglai Wei, PhD](https://donglaiw.github.io/). It is primarily concerned with studying how to re-construct 3D shaped neurons from Electron Microscope(EM) or Computed Tomography(CT) scanned images of brain tissues. Traditionally, this is done through semantic analysis where deep learning approaches(such as 3D U-Net) predicts which class each pixel of the image belongs to. EM generated images have better qualities at the cost of time and computation power (EM only looks at a small region of the brain). CT based approaches are more scalable at the cost of low image quality. In this project, we are exploring a way to re-construct 3D shapd neurons from low quality CT images by combining semantic segmentation with a global prior on the neuron shape to fix prediction errors. This means large 3D re-construction at scale is possible.

# Background and Problem Statement
Currently the most popular way of doing volumetric segmentation on Biomedical images is through variants of 3D U-Net[https://arxiv.org/abs/1606.06650]. Taking a cubic volume of the brain which has shape (z, x, y), 3D U-Net looks at each 2-D slices with sparsely annotated labels during training, and it is able to generalize to unlabelled part. The state of the art 3D U-Net is developed by Lee et.al[https://arxiv.org/abs/1706.00120], which achieves superhuman accuracy on the SNEMI3D Connectomics Challenge. Leveraging this model, we will apply it to segment the cubic volume into blood vessels and cells (including the cell body and dendrite). The volume we get is from CT images. This means the image is not high quality, and 3D U-Net will make some wrong predictions which cause our reconstruction to go wrong. The solution we propose is to construct a global prior of cells such that we can combine the segmentation and the global prior to correct the reconstructed models. For the global prior, the initial solution is to predict a set of anchor points and build a spline representation of the cell body and dendrite (this goal might be updated in the future upon further study). 

# Algorithm / Model Introduction

# Experiments

# Instructions

# Conclusion
