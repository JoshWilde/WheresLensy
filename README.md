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

The reconstructed lens images overall perform well. The table below shows the metrics for the performance of Naberrie on simulated Euclid data. The accuracy, recall, and precision show the performance of the classification of Naberrie. The loss is the root mean squared error of the target image compared to the reconstructed image. 

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

What's next? I need to improve on these results by creating a mask of the image artefacts explicitly. This should hopefully prevent the highest scoring images from being artefacts. I believe the reconstructed lens image and the prediction value have a correlation that could help reduce the number of false positives. 

![GreatInterest](https://github.com/JoshWilde/WheresLensy/blob/main/NAM/career-interests-phantom-menace-a3d0qzcnhlgig1v4.gif)


