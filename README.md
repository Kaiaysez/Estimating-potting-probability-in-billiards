# Estimating Potting Probability in Billiards Using Convolutional Neural Networks

<p float="left">
  <img src="https://github.com/Kaiaysez/Portfolio/blob/main/images/11%25%20blue.png" width="400">
  <img src="https://github.com/Kaiaysez/Portfolio/blob/main/images/33%25.png" width="400"> 
</p>
<p float="left">
  <img src="https://github.com/Kaiaysez/Portfolio/blob/main/images/65%25%20Red.png" width="400">
  <img src="https://github.com/Kaiaysez/Portfolio/blob/main/images/95%25%20Darkblue.png" width="400"> 
</p>

# NOTE: This project is a work in progress and most of what is written below is just for documenting my thoughts. I will upload a python/jupyter notebook file when the project is closer to completion.

## Background and motivation

## Goal

The goal is to build a model that can take in an image of particular situation on the pool table (right before the shot is made) as input, and then estimate the probability of making the shot from that position. At first glance, it seemed like a straightforward image classification project, but I realized that this problem had some very unique challenges that textbook image classification problems do not have.


## The raw data and the project idea

Data taken from the 150+ videos I took, with approximately 4000 shots played (On average I make 25 shots in a video, which means that I pot more balls than I miss, and so the bales are imbalanced). 

Every video recording, I followed the exact same procedures:
1. Set up the balls with the triangle in their designated spot with no special ball arrangements except that the black ball be in the center.
2. The goal is to sink all 15 balls in as few shots as possible.
3. The black ball must be potted last, and striking the black ball first when there are other balls remaining is illegal.

Therefore, every shot is an attempt to pot a ball, with no restrictions on which ball to pot except that the black ball must go last.

## Some questions/ideas

* How well can a convolutional neural network classify images of pool positions (taken right before the player hits the cue ball) into successful and unsuccessful pots via supervised learning with a data set of approximately 3000 images?
* How can we tweak the CNN architecture to get better results? (Perhaps even surpass human performance?)
* If performance is disappointing, is it due to a lack of training data? Or is there a better alternative to CNN’s for this task? (Hand engineering features?)
* Can we write a program to predict which pocket the player is aiming for?
* Can we generate predictions for where the cue ball or the object ball will end up?


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
 


## What is unique about this problem?:

We know that convolutional neural networks perform very well at image classification when trained with sufficient data. For tasks such as distinguishing between different objects, convolutional layers are very useful as they help to extract useful local features of the image (e.g ear shapes unique to dogs) which can then be fed to a dense neural network which deals with the global features.

For this problem, clearly the most important objects in the images are:
* The pockets
* The colored balls
* The cue ball
* The direction in which the cue stick is pointing in

A CNN should have no problems detecting these objects. But for this particular classification problem, detecting these objects does us no good. It is the relative position of these objects with respect to each other that determines the success rate of the shot. 

## Huge problems:

* Local features have almost zero predictive power. This is the key difference between this problem, and say classifying cats and dogs. With cats and dogs, local features such as ear shapes have extremely high predictive power. In this case however, ALL of the predictive power comes from the global features, namely the relative position of the key objects.
* The CNN has no understanding of the physics and dynamics of how these objects interact with each other. 
* There is too little training data.

## What we can realistically hope for, and possible things we can try:

To be honest, the low predictive power of local features seems like an insurmountable problem. However, trying to look at it in a more optimistic light, there are some silver linings:

* It is possible that the convolutional layers might be able to pick out the key objects, and that the dense layer might be able to find some global heuristics that have good predictive power. For example, it might learn that when the cue ball is far from the other balls, there is a low chance of potting, or that if all the balls are close to the rails, there is a low chance of potting.
* The images are all taken from the same angle, and for now we are not concerned with attempting to build a model which generalizes to images taken at other angles. This means slightly lower model complexity.
* There is a lot of “useless” information in the picture. Every pixel that is not one of the key objects is wasting computational power and making the problem unnecessarily complex. If there were a way to remove all the “fluff” so that only the positions of the key objects remain, then we might improve our chances. Perhaps implementing some extreme form of down sampling?
