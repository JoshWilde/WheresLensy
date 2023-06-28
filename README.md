# WheresLensy
![WelcomeNAM](https://github.com/JoshWilde/WheresLensy/blob/main/NAM_Wordart.PNG)

Are you looking for more after a quick introduction or did you miss my talk? Here I will show you the improvements I’ve made to the work in Wilde et al (2022) and lensfindery-mcLensFinderFace.

![RoadSoFar](https://github.com/JoshWilde/WheresLensy/blob/main/supernatural-text.gif)

Previously I have trained a CNN (OU-200) to identify strong gravitational lenses in simulated Euclid data. I have investigated what features OU-200 associated with gravitational lensing. I have found that blue ring like features are detected as gravitational lenses by OU-200. I have applied OU-200 to HST data, the majority of images classified as ‘lens’ with high confidence were image artifacts.  

This is of course not the ideal outcome.

![Perry](https://github.com/JoshWilde/WheresLensy/blob/main/perry-the-platypus-phineas-and-ferb.gif)

The solution is that we need a method to highlight areas of the image that the ML associates with lensing. This would aid human visual inspection in confirming that the model is performing sensibly. 

Who do we do this?

I have used a U-Net to generate images indicating where the lensing feature is in the image. This U-Net I have called Naberrie and is shown below.
The next step was to remove the decoder section and attach a classification section. The encoder section weights were frozen and the classification section was trained using categorical cross entropy loss to classify lenses and non-lenses.
![Naberrie](https://github.com/JoshWilde/WheresLensy/blob/main/NAM/UNET_VIS_NAM2022.png)

The reconstructed lens images overall perform well. 

Has this solved the problem?

Well for the key image artefact problem the short answer appears to be yes. The image artefacts are highlighted by the U-Net indicating that these are the features being identified as being associated with gravitational lensing. Chip gaps appear to be a very commonly associated feature with lensing that did not appear in OU-200.

I believe this U-Net will aid in the visual inspection of ML discovered gravitational lenses

![HappyMoment](https://github.com/JoshWilde/WheresLensy/blob/main/Fua7.gif)

What's next? I need to improve on these results by creating a mask of the image artefacts explicitly. This should hopefully prevent the highest scoring images from being artefacts. I believe the reconstructed lens image and the prediction value have a correlation that could help reduce the number of false positives. 

![GreatInterest](https://github.com/JoshWilde/WheresLensy/blob/main/career-interests-phantom-menace-a3d0qzcnhlgig1v4.gif)


