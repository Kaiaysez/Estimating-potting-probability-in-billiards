# Estimating Potting Probability in Billiards Using Convolutional Neural Networks

<p float="left">
  <img src="https://github.com/Kaiaysez/Portfolio/blob/main/images/11%25%20blue.png" width="400">
  <img src="https://github.com/Kaiaysez/Portfolio/blob/main/images/33%25.png" width="400"> 
</p>
<p float="left">
  <img src="https://github.com/Kaiaysez/Portfolio/blob/main/images/65%25%20Red.png" width="400">
  <img src="https://github.com/Kaiaysez/Portfolio/blob/main/images/95%25%20Darkblue.png" width="400"> 
</p>

# NOTE: This project is a work in progress. Will update and do a more in depth write-up as more progress is made.

## Background and motivation

## The problem and the goals

We want to build a machine learning model that can take in an image (such as the ones shown above) as input, and then estimate the probability of potting a ball (Shooting the ball into one of the pockets). Convolutional neural networks (CNN) tend to perform very well on most image classification problems as they are optimized for picking up local features unique to an object such as the ears of a cat or the wheels on a vehicle. What is unique about this problem is that we have the same objects in all the training images. What differentiates the potting probability being high or low depends not on the objects in the image per se, but their **relative positions**. The aim is to design a procedure that will allow us to build a CNN that performs well despite these difficulties. 

Goals:
* Designing and implementing a deep convolutional neural network (CNN) to predict potting probability in billiards in a given position.
* Testing different CNN architectures in an attempt to overcome the domain specific problem of requiring a **large effective receptive field** size to capture important information such as the position of the object balls, cue ball, cue stick, pockets and rails relative to each other.
* Experimenting with different image processing techniques such as image segmentation to make it easier to train the CNN. 
* Visualizing the filters and feature maps to see if the model comes up with intuitive heuristics for prediction.
* Using temperature scaling to calibrate the model so that the output can be **interpreted as probabilities** instead of just black and white classification.

## Image processing 



## The raw data 

Data taken from the 150+ videos I took, with approximately 4000 shots played (On average I make 25 shots in a video, which means that I pot more balls than I miss, and so the labels are imbalanced). 

Every video recording, I followed the exact same procedures:
1. Set up the balls with the triangle in their designated spot with no special ball arrangements except that the black ball be in the center.
2. The goal is to sink all 15 balls in as few shots as possible.
3. The black ball must be potted last, and striking the black ball first when there are other balls remaining is illegal.

**Therefore, every shot is an attempt to pot a ball, with no restrictions on which ball to pot except that the black ball must go last.**


## Data gathering process

1.Open google photos and type in “pool”

2.Select a video, making sure that it is not already in the “pool images in data set” album.

3.Create 2 word docs, success and fail.

4.Save the docs as “Web page”, then save as word doc again.

5.Now when images are pasted, they will be much larger (Higher resolution)

6.Play the chosen video, making sure that the “Info” tab is visible on the right.

7.Print screen just before each shot, and paste the image in the correct word doc.

8.After running through the video, add it to the “pool images in data set” album.

9.Save the word documents as “Web page” and all the pictures will automatically be saved into    
 a file!!!

## What is unique about this problem?

We know that convolutional neural networks perform very well at image classification when trained with sufficient data. For tasks such as distinguishing between different objects, convolutional layers are very useful as they help to extract useful local features of the image (e.g ear shapes unique to dogs) which can then be fed to a dense neural network which deals with the global features.

For this problem, clearly the **key objects** in the images are:
* The pockets
* The colored balls
* The cue ball
* The direction in which the cue stick is pointing in
* The rails

A CNN should have no problems detecting these objects. But for this particular classification problem, **detecting these objects does us no good**. It is the **relative position of these objects with respect to each other** that determines the success rate of the shot. 

## The bad news

* Local features have almost no predictive power. This is the key difference between this problem, and say classifying cats and dogs. With cats and dogs, local features such as ear shapes have extremely high predictive power. In this case however, ALL of the predictive power comes from the global features, namely the relative position of the key objects.
* The CNN has no understanding of the physics and dynamics of how these objects interact with each other. 
* The training data might not be sufficient.

## The good news

* Designing a CNN architecture that prioritizes increasing the receptive field size of neurons (especially in the deeper layers) and at the same time not allowing the parameter space to grow too large might work, but a lot of research and experimentation will be required.
* It is possible that the convolutional layers might be able to pick out the key patterns if we design a model that has a large receptive field size. At the very least, it might be able to detect common local patterns such as a ball hanging over a pocket, or a ball touching the rails.
* The images are all taken from the same angle, and for now we are not concerned with attempting to build a model which generalizes to images taken at other angles. This means slightly lower variation in the data.
* There is a lot of “useless” information in the picture. Every pixel that is not one of the key objects is wasting computational power and making the problem unnecessarily complex. Image segmentation techniques allow us to remove all the “fluff” so that there is less irrelevant information which might help with training.

## Some (additional) questions/ideas for future projects

* How can we tweak the CNN architecture to get better results? (Perhaps even surpass human performance?)
* If performance is disappointing, is it due to a lack of training data? Or is there a better alternative to CNN’s for this task? (Hand engineering features?)
* Can we write a program to predict which pocket the player is aiming for?
* Can we generate predictions for where the cue ball or the object ball will end up?
