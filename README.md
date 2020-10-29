# AudioDVP

This is the official implementation of **Photorealistic Audio-driven Video Portraits**.


## Major Requirements

* Ubuntu >= 18.04
* PyTorch >= 1.2
* GCC >= 7.5
* NVCC >= 10.1
* FFmpeg (with H.264 support)


FYI, detailed environment setup is in enviroment.yml.


## Major implementation differences against original paper

* Geometry parameter and texture parameter of 3DMM is now initialized from zero and shared among all samples during fitting, since it is more reasonable.

* Using OpenCV rather than PIL for image editing operation.


## Usage

### 1. Download face model data

* Download **Basel Face Model 2009** from https://faces.dmi.unibas.ch/bfm/main.php.
* Download **expression basis** from https://github.com/Juyong/3DFace.

* Put the data in `renderer/data` like the structure below.

    ```
    renderer/data
    ├── 01_MorphableModel.mat
    ├── BFM_exp_idx.mat
    ├── BFM_front_idx.mat
    ├── Exp_Pca.bin
    ├── facemodel_info.mat
    ├── select_vertex_id.mat
    ├── std_exp.txt
    └── data.mat(This is generated by the step 2 below.)
    ```

### 2. Build data

```zsh
cd renderer/
python build_data.py
```

### 3.Download pretrained model of ATnet
* The link is https://drive.google.com/drive/folders/1WYhqKBFX6mLtdJ8sYVLdWUqp5FJDmphg.
* Put `atnet_lstm_18.pth` in `vendor/ATVGnet/model`.

### 4.Download pretrained ResNet on VGGFace2
* The link is https://drive.google.com/open?id=1A94PAAnwk6L7hXdBXLFosB_s0SzEhAFU.
* Put `resnet50_ft_weight.pkl` in `weights`

### 5.Download Trump speech video
* The link is https://www.youtube.com/watch?v=6a1Mdq8-_wo. (Video courtesy of The White House.)
* Put it in `data/video`

### 6.Compile CUDA rasterizer kernel

```zsh
cd renderer/kernels
python setup.py build_ext --inplace
```


### 7.Running demo script

```zsh
# Explanation of every step is provided.
./scripts/demo.sh
```


Since we provide both training and inference code, we won't upload pretrained model for brevity at present.
We provide expected result in `data/sample_result.mp4` using synthesized audio in `data/test_audio`.

## Acknowledgment

This work is build upon many great open source code and data.

* **ATVGnet** in the vendor directory is directly borrowed from https://github.com/lelechen63/ATVGnet under MIT License.

* **neural-face-renderer** in the vendor directory is heavily borrowed from https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix under BSD License.

* The pre-trained **ResNet** model on VGGFace2 dataset is from https://github.com/cydonia999/VGGFace2-pytorch under MIT License.
We use resnet50_ft from https://drive.google.com/open?id=1A94PAAnwk6L7hXdBXLFosB_s0SzEhAFU.

* **Basel2009** 3D face dataset is from https://faces.dmi.unibas.ch/bfm/main.php?nav=1-0&id=basel_face_model.

* The **expression basis** of 3DMM is from https://github.com/Juyong/3DFace under GPL License.

* Our **renderer** is heavily borrowed from https://github.com/google/tf_mesh_renderer and inspired by https://github.com/andrewkchan/pytorch_mesh_renderer.

## Notification
* Our method is built upon *Deep Video Portraits*.
* Our method adopts a person-specific Audio2Expression module, which is not robust enough than a universal one trained on large dataset such as *Lip Reading Sentences in the Wild*. A universal one is encouraged! Fortunately, our method works quite well on WaveNet sythesized audio like provided in `data/test_audio`.
* The code IS NOT fully tested on another clean machine.

## Disclaimer

We made this code publicly available to benefit graphics and vision community.
Please DO NOT abuse the code for devil things. 

## Citation
```
@article{wen2020audiodvp,
    author={Xin Wen and Miao Wang and Christian Richardt and Ze-Yin Chen and Shi-Min Hu},
    journal={IEEE Transactions on Visualization and Computer Graphics}, 
    title={Photorealistic Audio-driven Video Portraits}, 
    year={2020}
}
```

## License

**BSD**
