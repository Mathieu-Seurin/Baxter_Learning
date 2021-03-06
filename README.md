# Project

This project is about a paper written by Rico Jonschkowski and Oliver Brock:  " Learning State Representation with Robotic Prior ". The goal is to learn state representations based on images and robotics priors to make a network able to produce high level representation of images.
This approach uses deep neural network to achieve the task of learning representation.
The input images are simulate images of Baxter's head camera. The camera is moving from right to left or left to right. The ouput of the neural network should be a representation in one dimension of the head joint angle. 
![Data example](/Data/pose10_head_pan/Images/frame0010.jpg)<br />
*Example of data from the simulation program*

# Installation

```bash
Install  torch : http://torch.ch/docs/getting-started.html
sudo apt-get install luarocks
luarocks install image (without sudo)
luarocks list (without sudo)
```

Go to your main folder :
```bash
th
dofile("script.lua")
```

# Data

200*200 pixels RGB images generated by baxter gazebo simuation.

# Training 

To train the neural network we use the robotics priors from the article "Learning State Representations with Robotic Priors".
We also use siamese networks to apply those priors, with convolutional network.
All the data is in this github's folder.
The output of the training should be a value for a given image strongly correlated with the true value of joint.
In training, we achieved 97% of correlation between the real signal of the head and the estimate signal. (the correlation is calculate between signal after normalization of the mean to 0 and std to 1 for the two signals)

![Data example](/Images/The_Truth.jpg)<br />
*Ground truth of the head positino for each image of the validation set*

![Data example](/Images/stateSave7_103_Test.jpg)<br />
*Example of representaion learned after training*

![Data example](/Images/model_en-page-001.jpg)<br />
*Model Used*

# Training with Data augmentation

When training without data augmentation, the result analysis  shows that the neural network only uses the position of the blue button in the image to compute its position. This seems a good way to solve the problem but if for example the button becomes green then the robot will not be able to solve the problem anymore. The network thus appears to be too specialized and using an easy trick to solve the problem. To improve the robustness of the network, we want it to use several pieces of information to compute its state.
To force the network to learn a wider range of features and not only the button position, we choose to artificially make the images more difficult to analyze. If the neural network cannot solve the task by only detecting the blue button it will search other ways. On this purpose we add noise and apply a random color filter on it. This is called data augmentation, the illustration shows different possible data augmentations for an image. The upper left image is the original picture. On bottom left is the image with only noise and other images are application of both noise and random color filters.

![Data example](/Images/imageDAtaAugmentation.jpg)<br />
*Data Augmentation example*


One possible solution for training : 
- Learning Rate : 0.001
- Batch Number : 5 by epoch
- Epoch Number : 20
- Batch size : 12 images


# Activation results

The visualization of activation at the last convolution are following. There is two different case of training : with data augmentation and without. The images are split in three part, the left hand side is the activation produce by the image on the rifht hand side. In the middle is the superpositon of both images to figure out which part of the image produce an activation.

![Data example](/Images/Unsupervised_woda.jpg)<br />
*Example of activation after training without data Augmentation*

![Data example](/Images/Unsupervised.jpg)<br />
*Example of activation after training with data Augmentation*

# Conclusion

This project is actually the beginning of a bigger project in which we could use this approach for learning higher dimension representations to be used in reinforcement learning algorithms. For example we could train a neural network to learn more complex representations like objects positions in three dimensions and use it within reinforcement learning process to check if a robot can use the learned representations to perform various tasks.
However,this project provides us with some evidence that a deep network trained by the method of robotics priors can learn representations states. This technique makes it possiblemakes it possible to learn a one dimension representation and furthermore to train a network to be resistant to both noise and color filters. 

