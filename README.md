# LSGAN
Inthis story, Least Squares Generative Adversarial Networks, (LSGAN), by City University of Hong Kong, The Education University of Hong Kong, Northwestern Polytechnical University, and CodeHatch Corp., is briefly reviewed.

Regular GANs hypothesize the discriminator as a classifier with the sigmoid cross entropy loss function may lead to the vanishing gradients problem during the learning process.

## Regular GANs
![Image](https://miro.medium.com/max/1002/1*qOy5eJ7FCTCCivH1BcxLvA.png)

- Regular GANs adopt the sigmoid cross entropy loss function. When updating the generator, this loss function will cause the problem of vanishing gradients for the samples that are on the correct side of the decision boundary, but are still far from the real data.
- e.g.: It gets very small errors for the fake samples (in magenta) for updating G as they are on the correct side of the decision boundary.

## LSGAN
![Image](https://miro.medium.com/max/1002/1*h3_JTnP-K6Lrp5C03AzKyA.png)

- To remedy this problem, LSGAN is proposed.
- Suppose we use the a-b coding scheme for the discriminator, where a and b are the labels for fake data and real data, respectively. Then the objective functions for LSGANs can be defined as follows:
![Image](https://miro.medium.com/max/1260/1*CO_5bIaspOnbrACds954Mg.png)

- where c denotes the value that G wants D to believe for fake data.
- First, the penalization will make the generator to generate samples toward the decision boundary, as shown above.

> Moving the generated samples toward the decision boundary leads to making them be closer to the manifold of real data.

![Image](https://miro.medium.com/max/1400/1*08-ojTEwRYdvLjHY858bCg.png)

> Second, penalizing the samples lying a long way to the decision boundary can generate more gradients when updating the generator, which in turn relieves the problem of vanishing gradients.

- This allows LSGANs to perform more stable during the learning process.
- As shown above, the least squares loss function is at only at one point, while the sigmoid cross entropy loss function will saturate when x is relatively large.

## Parameters Selection
- One condition is to set b-c=1 and b-a=2, such that minimizing the above loss functions yields minimizing the Pearson X² divergence between pd+pg and 2pg.
- (Please feel free to read the paper about the relation of LSGAN loss function to f-divergence, particularly the Pearson X² divergence.)
- For example, by setting a=1, b=1, and c=0, we got:

 ![Image](https://miro.medium.com/max/1400/1*osXXgK8CgHX12_oX_-FWJg.png)

- Another method is to make G generate samples as real as possible by setting c=b. For example, by using the 0-1 binary coding scheme:
![Image](https://miro.medium.com/max/1400/1*J-IvZM3ZC_zt2JfTwHI12A.png)

- In the paper, the latter one is used.

# Experimental Results
## LSUN Dataset
![Image](https://miro.medium.com/max/982/1*-ozBibzlDgTsSfa5VtuQ6A.png)

- DCGAN is used as baseline.
- Least square loss is applied on to DCGAN.

![Image](https://miro.medium.com/max/1400/1*Tp5mHURPWTSD9dnIbeCHjA.png)

- The images generated by LSGANs are of better quality than the ones generated by the two baseline methods, DCGANs and EBGANs.

![Image](https://miro.medium.com/max/1400/1*dkX8gEZh4ba3NTIMEZmbhg.png)

- The results of LSGANs trained on other scene datasets are also shown.

## Stability Comparison on LSUN Dataset
- Based on the network architectures of DCGANs, two architectures are designed to compare the stability.
- The first one is to exclude the batch normalization for the generator (BN_G), and the second one is to exclude the batch normalization for both the generator and the discriminator (BN_GD). With two optimizers Adam and RMSProp tested. Thus, there are 4 settings in total.
- These models are trained on LSUN bedroom dataset using regular GANs and LSGANs separately.
- First, for BNG with Adam, there is a chance for LSGANs to generate relatively good quality images. With 10 times of testing, and 5 of them succeeds to generate relatively good quality images.
- But for regular GANs, there is no successful learning. Regular GANs suffer from a severe degree of mode collapse.

![Image](https://miro.medium.com/max/1400/1*dSNbA4i98uJ8Sj6dDpQ7Ww.png)

- The generated images by LSGANs and regular GANs are shown above. LSGANs generate higher quality images than regular GANs which have a slight degree of mode collapse.
- LSGANs and regular GANs have similar performances for BN_G with RMSProp and BN_GD with Adam. Specifically, for BN_G with RMSProp, both LSGANs and regular GANs learn the data distribution successfully.
- For BN_GD with Adam, both the ones have a slight degree of mode collapse.
- Last, RMSProp performs more stable than Adam since regular.

## Stability Comparison on Gaussian Mixture Distribution Dataset
![Image](https://miro.medium.com/max/1400/1*h2uilNCC45jlpOBZuXQ81w.png)

- On this dataset, both the generator and the discriminator use a simple architecture, containing three fully-connected layers.
- Regular GANs suffer from mode collapse starting at step 15k. They generate samples around a single valid mode of the data distribution.
- But LSGANs learn the Gaussian mixture distribution successfully.

## Handwritten Chinese Characters
![Image](https://miro.medium.com/max/1400/1*VOqBiQ_lCCowepRItRjwoA.png)
- A conditional LSGAN model is trained.

![Image](https://miro.medium.com/max/1400/1*VV4IfCP73pSwB2FEjpY8AQ.png)

- LSGANs learn to generate readable Chinese characters successfully, and some randomly selected characters are shown above.
- The generated characters by LSGANs are readable.
- And the labels of the generated images through label vectors can be obtained correctly, which can be used for further applications such as data augmentation.
