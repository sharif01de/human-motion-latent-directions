# Goal: find a direction inside the latent space — slide it one way and the character walks faster, slide another and they jump higher — without breaking anything else.
(we can change our goals depend on the findings)

##Tile1: Discovering and Controlling Interpretable Latent Directions for (Walking Speed/jumping height)(we can argue here what is suitable) in 3D Human Motion Representations.

##Tile2: please add your thoughts here.....


## IDEA
1. Encode human motion recordings into latent vectors, then investigate whether one shared direction in that latent space represents a measurable physical parameter.

Start with walking speed or jump height.

The goal is to:
- Discover the direction.
- Predict the physical parameter from its projection score.
- Test whether moving along that direction changes the generated movement.

.....feel free to add here


## Literature review:
Work on interpretable latent spaces shows that generative models often encode meaningful attributes as linear directions, so moving a latent code along one of them changes a single property of the output. Härkönen et al. (2020) showed in GANSpace that such directions can be found without labels. They applied Principal Component Analysis (PCA) to the latent or intermediate feature space of pretrained image GANs such as StyleGAN and BigGAN. The leading components matched interpretable edits like pose, lighting and age, and were similar to directions that earlier methods found with supervision. Their limits matter for this project. The components are often partly entangled, so their meaning has to be identified by looking at the results. The work also covers only images, where attributes are judged visually rather than measured in physical units.

For human motion, Xu et al. (2024) proposed Semantic Latent Directions (SLD) for motion prediction. SLD learns a set of orthogonal latent directions during training and represents each predicted future motion as a linear combination of them. Changing the coefficients at inference time gives controllable and varied predictions. This shows that a motion latent space can be organised into directions that control movement. However, SLD builds the directions into the model’s training, and their meanings are described qualitatively. None of them is linked to a measurable physical quantity.

.....feel free to add here

##Research gap: 
These two papers leave a gap. GANSpace shows that unsupervised discovery works for images, but it has not been tested on human motion. SLD shows that latent directions can control motion, but only when the model is trained to have them, and the directions have no physical meaning. This project discovers directions after training, in an existing motion model, and tests them against physical quantities measured from the motion itself (walking speed in m/s, jump height in m). It also tests whether the supervised direction matches an unsupervised principal component.

.....feel free to add here

## Hypothesis

1. A learned human-motion latent space contains a direction whose projection correlates strongly with a continuous physical parameter, and manipulating that direction can control the corresponding parameter in generated motion.

2. The direction for the physical parameter closely matches one of the main directions of variation in the latent space, so it can be found without any labels.

3. Moving along the direction changes the parameter smoothly and predictably within the range seen in training, but motion quality and control accuracy drop when pushing beyond that range.

.....feel free to add here

##References

Härkönen, E., Hertzmann, A., Lehtinen, J., & Paris, S. (2020). GANSpace: Discovering interpretable GAN controls. Advances in Neural Information Processing Systems (NeurIPS), 33.

Xu, G., Tao, J., Li, W., & Duan, L. (2024). Learning semantic latent directions for accurate and controllable human motion prediction. Proceedings of the European Conference on Computer Vision (ECCV).

