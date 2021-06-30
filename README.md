# Near field Acoustic Holography on arbitrary shapes using Convolutional Neural Network
Repository of ["Near field Acoustic Holography on arbitrary shapes using Convolutional Neural Network"](https://arxiv.org/abs/2103.16935) 

M. Olivieri, M. Pezzoli, F. Antonacci, A. Sarti

EUSIPCO, 2021, Dublin

Near-field Acoustic Holography (NAH) is a well-known problem aimed at estimating the vibrational velocity field of a structure by means of acoustic measurements. In this paper, we propose a NAH technique based on Convolutional Neural Network (CNN). The devised CNN predicts the vibrational field on the surface of arbitrary shaped plates (violin plates) with orthotropic material properties from a limited number of measurements. In particular, the architecture, named Super Resolution CNN (SRCNN), is able to estimate the vibrational field with a higher spatial resolution compared to the input pressure. The pressure and velocity datasets have been generated through Finite Element Method simulations. We validate the proposed method by comparing the estimates with the synthesized ground truth and with a state-of-the-art technique. Moreover, we evaluate the robustness of the devised network against noisy input data.

## Proposed architecture
The architecture consists of a modified version of the UNet structure adopted in [1](https://github.com/polimi-ispl/nah-cnn) with the addition of a super-resolution section. The additional section allows us to deal with a higher resolution of the output with respect to the input. 
The detailed structure of the CNN architecture is depicted below along with the description of the layers dimensions.

The devised SRCNN-NAH network can be split into four parts: encoder, bottleneck, decoder, and super-resolution section.
The proposed encoder processes an input of 8x8 and it is composed of four layers. Each one consists of two consecutive layers of 2D convolutions with the same number of filters, kernel size 3×3 and ReLU activation functions followed by Batch Normalization. The compression is achieved with 2×2 max pooling operations. The number of filters follows this sequence: i) 16, ii) 32, iii) 32, iv) 64, thus the innermost layer has dimensions 1×1×64.

The decoder D reverses the encoder architecture taking as input the bottleneck embedding and performing up-convolutions with stride 2×2 with the following number of filters: v) 32, vi) 16, vii) 8. The skip connections are obtained by concatenating pairwise the encoder layers with the corresponding layers of the decoder.

Furthermore, the super-resolution section consists of two up-convolution blocks with asymmetric strides 1×2 followed by ReLU and Batch Normalization, and a last layer with strides 2×2. This allows us to reach the desired dimensions 16×64 at the output.

![alt text](https://github.com/polimi-ispl/nah-srcnn/blob/main/images/srcnn_architecture.png)

## About the code
The repo code is structured in the following folders:

* _"src"_ contains the following scipts:
  * _"srcnn_architecture.ipynb"_ contains the proposed architecture.
  * _"example.ipynb"_ contains a complete explanation for using the architecture. In particular you can find the test phase related to the paper reconstruction examples.
* _"data"_ contains the weights to test the trained model, the acoustic pressure measurements and velocity, ground truth and the violin binary masks realted to the reconstruction examples to use in the _example.ipynb_ file. All data are save in pickle format.

All the code file is given in Notebook files in order to provide complete explanations.
Scripts are exportable in Python language and the installation of the following modules is required:
* numpy
* tensorflow
* keras
* sklearn.metrics
* pickle (for reading and writing files such as datasets and saved models)
* matplotlib.pyplot (for data visualization with plots)

## Other materials
* _"media"_ folder contains the following materials:
  * [_"EUSIPCO21_poster.pdf"_](https://github.com/polimi-ispl/nah-srcnn/blob/main/media/EUSIPCO21_poster.pdf) is the poster of the paper presented at EUSIPCO21
  * [_"EUSIPCO21_presentation.mov"_](https://github.com/polimi-ispl/nah-srcnn/blob/main/media/EUSIPCO21_presentation.mov) is the video presentation of the paper showed at EUSIPCO21
