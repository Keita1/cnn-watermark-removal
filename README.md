# Fully convolutional watermark removal attack

Deep learning architecture to remove transparent overlays from images.

![example](example.png)

Top: left is with watermark, middle is reconstruction and right is the mask the algo predicts (the neural net was never trained using text or this image)

Bottom: Pascal dataset image reconstructions. When the watermarked area is saturated, the reconstruction tends to produce a gray color.

At train time, I generate a mask. It is a rectangle with randomly generated parameters (height, width, opacity, black/white, rotation). The mask is applied to a picture and the network is trained to find what was added. The loss is abs(prediction, image_perturbations)**1/2. The loss is not on the entire picture. An area around the mask is used to make the problem more tractable.

The network architecture does not down-sample the image. The prediction with a down-sampling network were not accurate enough. To have a large enough receptive field and not blow up the compute, I use dilated convolution. So concretely, I have a densenet style block, a bunch of dilated convolutions and final convolution to output a picture (3 channels). I did not spend much time doing hyper-parameters optimization. There's room to get better results using the current architecture.


## Usage

This project uses Tensorflow. Install packages with`pip install -r requirements.txt`

You will also need to download the pascal dataset from http://host.robots.ox.ac.uk/pascal/VOC/voc2012/ or CIFAR10 python version from https://www.cs.toronto.edu/~kriz/cifar.html

To train the network `python3 watermarks.py --logdir=save/`. It starts to produce some interesting results after 12000 steps.

To use the network for inference, you can run `inference` and `show_images` in Jupyter.
