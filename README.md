My MangaGAN!- Building My First Generative Adversarial Network
===
A complete walk through to build a Generative Adversarial Network to make your very own anime characters with Keras.
---
I have always been amazed by vivid animations, especially Manga and their bold looks and strokes. Wouldn't it be awesome to be able to draw a few ourselves, with the thrill of creating them with the help of a self-developed Neural Network Architecture!!!


So, what makes a GAN different?
---

The best way to master a skill is to practice it and improvise it until you're satisfied with yourself and your efforts. For a MACHINE or a NEURAL NETWORK, the best output it can generate is the one that Matches Human Generated outputs or even fool a human to believe that a human ACTUALLY produced the output. That's exactly what a GAN does, well, at least figuratively;)
Generative Adversarial Networks have lately been a hot topic in the Deep Learning field. Introduced by Ian Goodfellow et al., they have the ability to generate outputs from scratch.

Quick Overview of Generative Adversarial Networks
---
In Generative Adversarial Networks, two networks train and compete against each other, resulting in mutual improvisation. The generator misleads the discriminator by creating compelling fake inputs and tries to fool the discriminator into thinking of these as real inputs . The discriminator tells if an input is real or fake.

**There are 3 major steps in the training of a GAN:**
1. using the generator to create fake inputs based on random noise or in our case., random normal noise.
2. training the discriminator with both real and fake inputs(either simultaneously by concatenating real and fake inputs, or one after the other, the latter one being preferred).
3. train the whole model: the model is built with the discriminator combined with the generator.

An important point to Note is that discriminator's weights are frozen during the last step.

The reason for combining both networks is that there is no feedback on the generator's outputs. The ONLY guide is if the discriminator accepts the generator's output.
> You can say that they are rival destined to each other. The main character is the Generator who strive better and better to make our   purpose realized by learning from the fight from its rival.

That was a brief overview of GAN's architecture. If that doesn't suffice, you can refer to this elaborate introduction.
OUR GAN
We use a DCGAN(Deep convolutional generative adversarial networks) for the task at hand.
A few key points about DCGAN-
Replace all max pooling with convolutional strides
Use transposed convolution for UpSampling.
Eliminate fully connected layers.
Use Batch normalization except the output layer for the generator and the input layer of the discriminator.
Use ReLU in the generator except for the output which uses tanh.
Use LeakyReLU in the discriminator.

Setup Details
keras version==2.2.4 
tensorflow==1.8.0
jupyter notebook
matplotlib and other utility libraries like NumPy, Pandas.
python==3.5.7

DATASET
The dataset for anime faces can be generated by curling through various manga websites and downloading images, cropping faces out of them and resizing them to a standard size. Given below is the link for A Python code depicting the same:-
https://github.com/pavitrakumar78/Anime-Face-GAN-Keras/blob/master/anime_dataset_gen.py
I managed to be lucky enough to find a pre-processed(faces-cropped) dataset from the following sites:-
Good Quality Manga faces dataset(around 22,000 images)
drive.google.com
https://www.kaggle.com/aadilmalik94/animecharacterfaces


A Glimpse of the Dataset-
glimpse of the datasetTHE MODEL
Now, Let's have a look at the Neural Networks Architectures! Do remember the points we discussed earlier about DCGANs.
This implementation of GAN uses deconv layers in Keras . I have tried various combinations of layers such as :
Conv + Upsampling
Conv + bilinear
Conv + Subpixel Upscaling 
Note:- You can refer to my repository for the complete code.
THE GENERATOR
It consists of Convolution Transpose Layers followed by Batch Normlisation and A Leaky ReLU activation function for Upsampling. We will use strides parameter in the Convolution Layer. This is done to avoid unstable training of the GAN. Leaky ReLUs are one attempt to fix the "dying ReLU" problem. Instead of the function being zero when x < 0, a leaky ReLU will instead have a small negative slope (of 0.01, or so).
Code
GeneratorTHE DISCRIMINATOR
It also consists of Convolution Layers where we use strides to do Downsampling and Batch Normalisation for stability.
Code
DiscriminatorTHE COMPILED GAN
To conduct backpropagation for the Generator for keeping a check on it's outputs, we compile a network in Keras, which is Generator followed by Discriminator. In this network, the input would be the random noise for the generator, and the output being the generator's output fed to the discriminator, keeping the Discriminator's weights frozen to avoid the adversarial collapse. Sounds cool right? Look it up!
Code
Combined GANTRAINING THE MODEL
minimax objective realised during trainingBasic Configurations of the model


Generate random normal noise for input

2. Concatenate real data sampled from dataset with generated noise
3. Add noise to the input label
4. Training only the Generator
5. Training only the Discriminator
6. Train the combined GAN
7. Saving instances of Generator and Discriminator
I trained this code on my Acer-Predator helios 300 which took a time of almost half an hour for 10000 steps and around 32k images, with an nvidia GTX GeForce 1050Ti GPU.
MANGA-GENERATOR RESULTS
After training for 10000 steps, the results came out to be pretty cool ad satisfying. Have A Look!


Transition of Images throughout TrainingFinal Output ImagesAlthough I think training for a longer duration and on a Bigger dataset would improve the results further(Some of the faces were scary weird!. Not the conventional Manga, I must say:D)
CONCLUSION
The task of generating manga-style faces was sure interesting. 
But, there is a lot of scope for improvement with better training, models and dataset. Our Model cannot make a Human wonder if these faces generated were real of fake, even so, it does an appreciably good task of generating manga-style images. Go ahead and try this with complete manga postures too.
REPOSITORY
Here is the link to my github repository. Feel free to have a look, clone or improvise it. All kinds of suggestions and criticisms are welcome.
SOURCES
https://towardsdatascience.com/generate-anime-style-face-using-dcgan-and-explore-its-latent-feature-representation-ae0e905f3974
https://github.com/pavitrakumar78/Anime-Face-GAN-Keras
https://medium.com/@jonathan_hui/gan-dcgan-deep-convolutional-generative-adversarial-networks-df855c438f
