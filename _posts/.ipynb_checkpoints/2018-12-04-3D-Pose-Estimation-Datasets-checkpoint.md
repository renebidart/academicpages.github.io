---
title:  "3D Pose Estimation Datasets"
date:   2018-12-04
author: Rene Bidart
---

## Overview
2D pose estimation has improved immensely over the past few years, partly because of wealth of data stemming from the ease of annotating any RGB video. 3D pose annotation is much more difficult because accurate 3D pose annotation requires using motion capture in indoor artificial settings.

| ![mocap.jpg](/images/post_imgs/pose/mocap.jpg) |
|:--:| 
| *Mocap requires artificial settings and unnatural clothing, as shown in the [Max Plank](https://ps.is.tuebingen.mpg.de/pages/motion-capture) lab* |

Ground truth vicon datasets can be augmented with synthesized poses using different body shapes, background, etc. but still are limited by the original collected data. Alternatively, synthetic datasets can be used to increase variety. In addition, some use RGB cameras and auxiliary measurement to estimate 3D poses in the wild.

Some promising datasets are [3D Poses in the Wild](http://openaccess.thecvf.com/content_ECCV_2018/papers/Timo_von_Marcard_Recovering_Accurate_3D_ECCV_2018_paper.pdf) for in the wild poses, [JTA](https://arxiv.org/pdf/1803.08319.pdf) if for simulated data, and [Human3.6m](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=6682899) and [Total Capture](https://cvssp.org/projects/totalcapture/TotalCapture/TrumbleBMVC2017.pdf) for controlled environments with ground truth labels.

## Ground Truth Datasets
These datasets include ground truth, including full 3d skeleton using mocap, which has negligible measurement error. The limitation is mocap tends to only work in controlled environments, so models trained on these datasets may struggle to generalize.

Alternatively, synthetic data can be generated either based on human data, as in SURREAL, or entirely synthetically using a game engine as in JTA. Both allow for good quality of annotations, but are limited because of the computer rendered graphics rather than true natural images.


### Human3.6m: Large scale datasets and predictive methods for 3d human sensing in natural environments (2014)
This is the standard in 3d pose estimation. A dataset of 11 people doing 17 common poses in an indoor environment, resulting in a total of 3.6 million frames. The following measurements are included:

* RGB views: 4 standard ones, one with depth
* Exact 3D joint positions from mocap
* Pixel-level body part labels for a subset of the data
* 3D laser scans of the actors (done once, not throughout the videos)

This dataset mostly includes everyday poses, not anything sports specific. natural scenarios, but no specific sports, etc. 

They include some mixed reality augmentation for the dataset as well. Details can be found in [Human3.6m: Large scale datasets and predictive methods for 3d human sensing in natural environments](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=6682899)

| ![36m.png](/assets/post_imgs/pose/36m.png) |
|:--:| 
| *Example poses from Human3.6m* |

### HumanEva (2009)
Similar to the 3.6m dataset, but more limited. It is actually composed of two datasets, HumanEva I & HumanEva II, both in controlled indoor environments.

The HumanEva I dataset consists of:

* RGB views: 3 standard ones 
* 60 Hz 3D joint positions from mocap

HumanEva II is similar, but with the addition of another camera, as well as multi action scenarios including running around a loop performing actions.

These datasets have more natural clothes, but this comes with the cost of moving sensors, and less accurate mocap data. Details can be found in [HumanEva](http://humaneva.is.tue.mpg.de)

| ![humaneva.png](/images/post_imgs/pose/humaneva.png) |
|:--:| 
| *Examples from HumanEva II* |

### Total Capture: 3D Human Pose Estimation Fusing Video and Inertial Sensors

The [Total Capture](https://cvssp.org/projects/totalcapture/TotalCapture/TrumbleBMVC2017.pdf) dataset contains indoor multi-view video, IMU, and vicon mocap with ∼1.9M frames. Very constrained environment.

This dataset is useful to compare 3d pose estimating using IMU and multiview cameras to ground truth using mocap, but again is quite limited by being in a controlled environment.

| ![total.png](/images/post_imgs/pose/total.png) |
|:--:| 
| *Different data types contained in Total Capture* |


### Learning from Synthetic Humans (2017)
SURREAL (Synthetic hUmans foR REAL tasks) dataset contains synthetically generated data rendered from 3D sequences of motion capture data. The original data is from Human3.6M.

The idea is that you can randomly sample the person's pose, appearance, lighting, camera position and background, creating a massive dataset from only a few annotated models. The SMPL model is used to decompose the body into pose and shape parameters, so we can sample these independently to produce an image. To generate realistic shapes, they use the CAESAR dataset to train SMPL, and then randomly select a participant and perturb the shape components slightly. 

Once the person is generated from the random shape and pose parameters, sample camera, texture, light and background to get the full scene. Details can be found in [Learning from Synthetic Humans](https://arxiv.org/abs/1701.01370)

| ![surreal.png](/images/post_imgs/pose/surreal.png) |
|:--:| 
| *Surreal pipeline for generating data* |

### JTA Dataset (2018)

| ![gta5.png](/images/post_imgs/pose/gta5.png) |
|:--:| 
| *Surreal pipeline for generating data* |

Massive simulated dataset created from Grand Theft Auto V. Consists of about 500 000 frames with almost 10 million body poses labeled with full 3D annotation.

Details can be found in [Learning to Detect and Track Visible and Occluded Body Joints in a Virtual World](https://arxiv.org/pdf/1803.08319.pdf)


## In the wild
Instead of using ground truth mocap annotations, annotations are estimated in order to get a wider variety of actions in the wild instead of being constrained to indoor setting with mocap. Ground truth can be estimated using auxiliary measurements such as:

* Inertial measurement units
* Multiview cameras
* RGBD cameras


### 3D Poses in the Wild Dataset
60 in the wild videos with 2D and 3D pose annotation, including camera pose and scanned models with different clothing variations. The poses are estimated using IMUs and a handheld 2d video. This is the only promising 3d pose in the wild dataset. 

| ![wild.png](/images/post_imgs/pose/wild.png) |
|:--:| 
| *Actual in the wild images with 3d poses* |


On TotalCapture dataset this gets 26mm error, and outperforms their method without using multiview. This is a best case scenario for accuracy because it was in a controlled environment, so we should expect this dataset to have error >26mm. Nevertheless, this could be very useful for situations where absolute accuracy isn't crucial, and we want to generalize to the wild. More details on their [paper.](http://openaccess.thecvf.com/content_ECCV_2018/papers/Timo_von_Marcard_Recovering_Accurate_3D_ECCV_2018_paper.pdf)


### Monocular 3D Human Pose Estimation In The Wild Using Improved CNN Supervision (2016) 

Using multi-camera markerless system, recording 3D pose in green screen to change background.  8 actors, 8 cameras, >1.3M frames. This is still indoor in a relatively controlled environment, so it isn't truly in the wild, and there may be some uncertainty with how accurate their ground truth is. More info in their [paper](https://arxiv.org/pdf/1611.09813.pdf)


### Unite the People: Closing the Loop Between 3D and 2D Human Representations (2017)

In [Unite the People](https://arxiv.org/abs/1701.02468) they use RGB to 3D estimation based on CNN, with some manual removal of bad results, and no uncertainty estimate. They tested their model on model on Human3.6m, getting 80mm average error. This is probably best case error, so this isn't useful for most applications.

