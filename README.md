# Perceptual Weighting Filter Loss

Please find here the scripts referring to the paper [A Perceptual Weighting Filter Loss for DNN Training in Speech Enhancement](https://arxiv.org/pdf/1905.09754.pdf). In this repository we provide the source code for training/validation data preparation (including the amplitude response for the perceptual weighting filter), network training/validation (including the proposed perceptual weighting filter loss), network inference, and enhanced speech waveform reconstruction. 

The code was written by [Ziyue Zhao](https://ziyuezhao.github.io/) and some of the contributions are from Ziyi Xu. 

## Introduction

In this project, instead of applying the commonly used mean squared error (MSE) as the loss function during DNN training for single-channel speech enhancement, we designed a perceptual weighting filter loss. The proposed loss is motivated by the perceptual weighting filter in analysis-by-synthesis speech coding, e.g., in code-excited linear prediction (CELP). The proposed approach outperforms the reference DNN trained with MSE loss in terms of better PESQ and higher noise attenuation.

## Prerequisites

- [Matlab](https://www.mathworks.com/) 2014a or later
- [Python](https://www.python.org/) 3.6
- CPU or NVIDIA GPU + [CUDA](https://developer.nvidia.com/cuda-toolkit) 9.0 [CuDNN](https://developer.nvidia.com/cudnn) 7.0.5


## Getting Started

### Installation

- Install [TensorFlow](https://www.tensorflow.org/) 1.5.0 and [Keras](https://www.tensorflow.org/) 2.1.4
- Some Python packages need to be installed, please see detailed information in the Python scripts.
- Install [Matlab](https://www.mathworks.com/)

### Datasets

Note that in this project the clean speech signals are taken from the [Grid corpus](https://doi.org/10.1121/1.2229005) (downsampled to 16 kHz) and noise signals are taken from the [ChiMe-3](https://ieeexplore.ieee.org/abstract/document/7404837/) database. In order to run the scripts in this project, the abovementioned two databases are assumed to be available locally and placed under the directory of `./Audio Data/grid corpus 16khz/` and `./Audio Data/16khz noise/`, respectively (see `GitHubTrain_part_1_CleanAndNoisyMixture.m` for the detailed directory structure of the datasets).

### Training and validation data preparation

 - Run the Matlab script to generate the frame-wise spectral amplitudes for clean and noisy speech under various SNRs: 
```bash
matlab GitHubTrain_part_1_CleanAndNoisyMixture.m
```
 - Run the Matlab script to generate the frame-wise spectral amplitudes response for the perceptual weighting filter:
```bash
matlab GitHubTrain_part_2_WghFilterResponse.m
```
 - Run the Matlab script to generate the training/validation data for the DNN model based on the output data from part 1 and 2:
```bash
matlab GitHubTrain_part_3_TrainValidDataPrepare.m
```

### Train the DNN models

 - Run the Python script to train the DNN model with the **proposed loss** based on the prepared training/validation data:
```bash
python GitHub_mask_dnn_weight_filter_train.py
```

 - As a baseline approach, run the Python script to train the DNN model with the **MSE loss** based on the same training/validation data:
```bash
python GitHub_mask_dnn_baseline_train.py
```

### Test data preparation 

 - Run the Matlab script to generate the test input data for the inference of DNN models:
```bash
matlab GitHubTest_GenerateInputData.m
```

### Inference of the DNN models

 - Run the Python script to test the trained DNN model with the **proposed loss** using the prepared test data:
```bash
python GitHub_all_test_mask_dnn_weight_filter.py
```

 - As the baseline approach, run the Python script to test the trained DNN model with the **MSE loss** using the same test data:
```bash
python GitHub_all_test_mask_dnn_baseline.py
```

### Enhanced speech reconstruction

 - Run the Matlab script to reconstruct the enhanced speech signals with DNN models using the proposed loss and the MSE loss, respectively:
```bash
matlab GitHubTest_GenerateAudioFiles.m
```

Note that the frame-wise amplitudes response for the perceptual weighting filter is only needed in the DNN training, not in the DNN inference. That means the proposed loss function can be advantageously applied to an existing DNN-based speech enhancement system, without changing the DNN topology or speech enhancement framework.

## Citation

If you use the scripts in your research, please cite

```
@article{zhao2019perceptual,
  author =  {Z. Zhao and S. Elshamy and T. Fingscheidt},
  title =   {{A Perceptual Weighting Filter Loss for DNN Training in Speech Enhancement}},
  journal = {arXiv preprint arXiv: 1905.09754},
  year =    {2019},
  month =   may
}
```
## Acknowledgements
- The author would like to thank Maximilian Strake and Samy Elshamy for the advice concerning the construction of the project in GitHub.
