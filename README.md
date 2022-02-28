## BI-DIRECTIONAL NORMALIZATION AND COLOR ATTENTION-GUIDED GENERATIVE ADVERSARIAL NETWORK FOR IMAGE ENHANCEMENT

#### 2022 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)
[Shan Liu](https://github.com/33KU), Guoqiang Xiao, Xiaohui Xu, Song Wu

##### [[Paper-official](https://XXXXXXXXXXXXX)] 

College of Computer and Information Science, Southwest University, Chongqing, China

## Introdcurion

This website shares the codes of the "BI-DIRECTIONAL NORMALIZATION AND COLOR ATTENTION-GUIDED GENERATIVE ADVERSARIAL NETWORK FOR IMAGE ENHANCEMENT", 2022 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP). 

### Abstract

Most existing image enhancement methods require paired images for supervised learning, and rarely considering the aesthetic quality. This paper proposes a bi-directional normalization and color attention-guided generative adversarial network (BNCAGAN) for unsupervised image enhancement. An auxiliary attention classifier (AAC) and a bi-directional normalization residual (BNR) module are designed to assist the generator in flexibly controlling the local details with the constraint from both the low/high-quality domain. Moreover, a color attention module (CAM) is proposed to preserve the color fidelity in the discriminator. The qualitative and quantitative experimental results demonstrated that our BNCAGAN is superior to the existing methods on image enhancement. It can effectively improve the authenticity and naturalness of the enhanced images. The source code is available at https://github.com/SWU-CS-MediaLab/BNCAGAN.

<!-- **The framework of BNCAGAN:** -->
<img src="./figures/overview.png" width = "100%" height = "100%" div align=center>


## Requirements and Installation
We recommended the following dependencies.
*  Python 3.7
*  PyTorch 1.7.1
*  tqdm 4.42.1
*  munch 2.5.0
*  torchvision 0.8.2

## The method of obtaining the image of the MIT-Adobe FiveK Dataset can be found in [UEGAN](https://github.com/eezkni/UEGAN). 
## Preparing Data for the MIT-Adobe FiveK Dataset 
You can follow the instructions below to generate your own training images. Or, you can directly download exported images [FiveK_dataset_nzk](https://drive.google.com/drive/folders/1Jv0_9CnYxh_2ReFaVrwG19O3F7xBtdZT?usp=sharing). (~6GB)

### Getting the MIT-Adobe FiveK Dataset
 - Download the dataset from https://data.csail.mit.edu/graphics/fivek/. (~50GB, SHA1)
 - Extract the data.
 - Open `fivek.lrcat` with Lightroom. Just click "upgrade" if Lightroom asks you to upgrade.

### Generating the Low-quality Images
 - Import the FiveK dataset into Adobe Lightroom.
 - In the `Collections` list (bottom left), select collection **`Inputs/InputAsShotZeroed`**.
 - Export all images in the following settings:
   - Select all images at the bottom or in the middle (select one and press `Ctrl-A`), right-click any of them and select `Export/Export...`. 
   - Export Location: `Export to`=`Specific folder`, `Folder`=`Your folder for low-quality images`.
   - File Settings: `Image Format`=`PNG`, `Color Space`=`sRGB`, `Bit Depth`=`8 bit/component`
   - Image Sizing: `Resize to Fit`=`Short Edge`, select `Don't Enlarge`, Fill in `512 pixels`, `Resolution` doesn't matter to ignort it.
   - Finally, click `Export`.

### Generating the High-quality Images
 - Import the FiveK dataset into Adobe Lightroom.
 - In the `Collections` list (bottom left), select collection **`Experts/C`**.
 - Export all images in the following settings:
   - Select all images at the bottom or in the middle (select one and press `Ctrl-A`), right-click any of them and select `Export/Export...`. 
   - Export Location: `Export to`=`Specific folder`, `Folder`=`Your folder for high-quality images`.
   - File Settings: `Image Format`=`PNG`, `Color Space`=`sRGB`, `Bit Depth`=`8 bit/component`
   - Image Sizing: `Resize to Fit`=`Short Edge`, select `Don't Enlarge`, Fill in `512 pixels`, `Resolution` doesn't matter to ignort it.
   - Finally, click `Export`.


## Getting the DPED Dataset 
You can download the dataset from [DPED](http://people.ee.ethz.ch/~ihnatova/#dataset). 


## Training
Prepare the training, testing, and validation data. The folder structure should be:
```
data
└─── fiveK
	├─── train
	|	├─── exp
	|	|	├──── a1.png                  
	|	|	└──── ......
	|	└─── raw
	|		├──── b1.png                  
	|		└──── ......
	├─── val
	|	├─── label
	|	|	├──── c1.png                  
	|	|	└──── ......
	|	└─── raw
	|		├──── c1.png                  
	|		└──── ......
	└─── test
		├─── label
		| 	├──── d1.png                  
		| 	└──── ......
		└─── raw
			├──── d1.png                  
			└──── ......
```
```raw/```contains low-quality images, ```exp/``` contains unpaired high-quality images, and ```label/``` contains corresponding ground truth.

To train BNCAGAN on FiveK, run the training script below.
```
python main.py --mode train --version BNCAGAN-FiveK --use_tensorboard False \
--is_test_nima True --is_test_psnr_ssim True
```

This script will create a folder named ```./results``` in which the resulting are saved. 
- The PSNR results will be saved to here: ```./results/psnr_val_results``` (including PSNR for each valiaded epoch and the summary)
- The SSIM results will be saved to here: ```./results/ssim_val_results``` (including SSIM for each valiaded epoch and the summary)
- The NIMA results will be saved to here: ```./results/nima_val_results``` (including NIMA for each valiaded epoch and the summary)
- The training logs will be saved to here: ```./results/BNCAGAN-FiveK/logs```
- The models will be saved to here: ```./results/BNCAGAN-FiveK/models```
- The intermediate results will be saved to here: ```./results/BNCAGAN-FiveK/samples```
- The validation results will be saved to here: ```./results/BNCAGAN-FiveK/validation```
- The test results will be saved to here: ```./results/BNCAGAN-FiveK/test```


The summary of PSNR test results will be save to ```./results/psnr_val_results/PSNR_total_results_epoch_avgpsnr.csv```. Find the best epoch in the last line of ```PSNR_total_results_epoch_avgpsnr.csv```.

To test BNCAGAN on FiveK, run the test script below.
```
python main.py --mode test --version BNCAGAN-FiveK --pretrained_model xx (best epoch, e.g., 100) \
--is_test_nima True --is_test_psnr_ssim True

```


## Additional experimental results pictures
<img src="./figures/fiveK.png" width = "100%" height = "100%" div align=center>
The Fig. 1 shows the comparison results with the state-of-the-art methods on FiveK dataset.

<img src="./figures/dped.png" width = "100%" height = "100%" div align=center>
The Fig. 2 shows the comparison results with the state-of-the-art methods on FiveK dataset.

<img src="./figures/loss.png" width = "100%" height = "100%" div align=center>
The Fig. 3 shows the effectiveness of our designed loss function in BNCAGAN.

<img src="./figures/module.png" width = "100%" height = "100%" div align=center>
The Fig. 4 shows the effectiveness of our proposed modules in BNCAGAN.

## Citation

If this code/BNCAGAN is useful for your research, please cite our paper:

```
@article{
}
```


## License

[MIT License](https://opensource.org/licenses/MIT)


