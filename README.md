# WheresLensy
![WelcomeNAM](https://github.com/JoshWilde/WheresLensy/blob/main/NAM/NAM_Wordart.PNG)

Are you looking for more after a quick introduction or did you miss my talk? Here I will show you the improvements I’ve made to the work in [Wilde et al (2022)](https://academic.oup.com/mnras/article-abstract/512/3/3464/6544650) and [lensfindery-mcLensFinderFace](https://github.com/JoshWilde/LensFindery-McLensFinderFace).

![RoadSoFar](https://github.com/JoshWilde/WheresLensy/blob/main/NAM/supernatural-text.gif)

Previously I have trained a CNN (OU-200) to identify strong gravitational lenses in simulated Euclid data. I have investigated what features OU-200 associated with gravitational lensing. I have found that blue ring like features are detected as gravitational lenses by OU-200. I have applied OU-200 to HST data, the majority of images classified as ‘lens’ with high confidence were image artifacts.  

This is of course not the ideal outcome.

![Perry](https://github.com/JoshWilde/WheresLensy/blob/main/NAM/perry-the-platypus-phineas-and-ferb.gif)

The solution is that we need a method to highlight areas of the image that the ML associates with lensing. This would aid human visual inspection in confirming that the model is performing sensibly. 

Who do we do this?

I have used a U-Net to generate images indicating where the lensing feature is in the image. This U-Net I have called Naberrie and is shown below.
The next step was to remove the decoder section and attach a classification section. The encoder section weights were frozen and the classification section was trained using categorical cross entropy loss to classify lenses and non-lenses.
![Naberrie](https://github.com/JoshWilde/WheresLensy/blob/main/NAM/UNET_VIS_NAM2022.png)

The reconstructed lens images overall perform well. Below I show four examples of Naberrie's performance on the simulated VIS band, with the input image on the left, the target image in the centre, and the output image from Naberrie on the right.
![Naberrie_ring](https://github.com/JoshWilde/WheresLensy/blob/main/NAM/NAM2022_UNET_VIS_12009.png)
![Naberrie_Arc1](https://github.com/JoshWilde/WheresLensy/blob/main/NAM/NAM2022_UNET_VIS_12003.png)
![Naberrie_small_ring](https://github.com/JoshWilde/WheresLensy/blob/main/NAM/NAM2022_UNET_VIS_12001.png)
![Naberrie_Arc2](https://github.com/JoshWilde/WheresLensy/blob/main/NAM/NAM2022_UNET_VIS_12049.png)
What do these images show us? 


The table below shows the metrics for the performance of Naberrie on simulated Euclid data. The accuracy, recall, and precision show the performance of the classification of Naberrie. The loss is the root mean squared error of the target image compared to the reconstructed image. 

|     Band    |     Accuracy    |     Recall    |     Precision    |     Loss      |
|-------------|-----------------|---------------|------------------|---------------|
|     VIS     |     81.37%      |     0.62      |     0.69         |     0.0024    |
|     Y       |     60.84%      |     0.61      |     0.61         |     0.0013    |
|     J       |     63.32%      |     0.64      |     0.60         |     0.0012    |
|     H       |     64.24%      |     0.64      |     0.65         |     0.0013    |

Below is the AUROC curve for the Euclid simulated bands on the classification results of Naberrie.
![EuclidAUROC](https://github.com/JoshWilde/WheresLensy/blob/main/NAM/Euclid_AUROC.png)


Has this solved the problem?

Well for the key image artefact problem the short answer appears to be yes. The image artefacts are highlighted by the U-Net indicating that these are the features being identified as being associated with gravitational lensing. Chip gaps appear to be a very commonly associated feature with lensing that did not appear in OU-200.

I believe this U-Net will aid in the visual inspection of ML discovered gravitational lenses

![HappyMoment](https://github.com/JoshWilde/WheresLensy/blob/main/NAM/Fua7.gif)

I have tried to expand on Naberrie by training multiple Mask R-CNNs to identify gravitational lenses. Three of these models (MRC-95, MRC-99, MRC-995) only classify gravitational lenses and the difference between these models are the threshold value used to create the target mask from the target image. The other model is MRC-3 which includes the possibility to classify objects as non-lenses. The idea behind this was that by learning what a non-lens looks like it could help th model perform better at finding gravitational lenses.
![Mask_RCNN](https://github.com/JoshWilde/WheresLensy/blob/main/NAM/Input_Data_1.png)
What we can see in the image above is the input image on the left, the lens light in the middle, and the mask of each object on the right. The gravitational lens is shown in cyan, the lensing galaxy is shown in red, and any other colour is an additiational galaxy in the image. These galaxies were added to prevent the model from learning to add lenses around all the galaxies in the test set.


What's next? I need to improve on these results by creating a mask of the image artefacts explicitly. This should hopefully prevent the highest scoring images from being artefacts. I believe the reconstructed lens image and the prediction value have a correlation that could help reduce the number of false positives. 

![GreatInterest](https://github.com/JoshWilde/WheresLensy/blob/main/NAM/career-interests-phantom-menace-a3d0qzcnhlgig1v4.gif)


