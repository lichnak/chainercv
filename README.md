<!--[![travis](https://travis-ci.org/pfnet/chainercv.svg?branch=master)](https://travis-ci.org/pfnet/chainercv)-->

<!--[![pypi](https://img.shields.io/pypi/v/chainercv.svg)](https://pypi.python.org/pypi/chainercv)-->


# ChainerCV

ChainerCV is a collection of tools to train neural networks for computer vision tasks using [Chainer](https://github.com/pfnet/chainer).

You can find the documentation [here](http://chainercv.readthedocs.io/en/latest/).

This project is under development, and some API may change in the future.


# Installation

```
pip install chainercv
```


### Requirements

+ [Chainer](https://github.com/pfnet/chainer) and its dependencies
+ Pillow

For additional features

+ Matplotlib
+ OpenCV
+ Scikit-Learn


Environments under Python 2.7.12 and 3.6.0 are tested.


# Features

## Transforms

ChainerCV supports functions commonly used to prepare image data before feeding to neural networks.
We expect users to use these functions together with a dataset object (e.g. `chainer.dataset.DatasetMixin`).
Many of the datasets prepared in ChainerCV are very thin wrappers around raw datasets in the filesystem, and
the transforms work best with such thin dataset classes.
The users can create a custom preprocessing pipeline by defining a function that describes
procedures to transform data.

Here is an example where the user rescales input image and data augments it by randomly rotation.

```python
from chainer.datasets import get_mnist
from chainercv.datasets import TransformDataset
from chainercv.transforms import random_rotate

dataset, _ = get_mnist(ndim=3)

def transform(in_data):
    # in_data is the returned values of VOCSemanticSegmentationDataset.get_example
    img, label = in_data
    img -= 0.5  # rescale to [-0.5, 0.5]
    img = random_rotate(img)
    return img, label
dataset = TransformDataset(dataset, transform)
img, label = dataset[0]
```

As found in the example, `random_rotate` is one of the transforms ChainerCV supports. Like other transforms, this is just a
function that takes an array as input.
Also, `TransformDataset` is a new dataset class added in ChainerCV that overrides the underlying dataset's `__getitem__` by calling `transform` as post processing.


# Automatic Download
ChainerCV supports automatic download of datasets. It uses Chainer's default download scheme for automatic download.
All data downloaded by ChainerCV is saved under a directory `$CHAINER_DATASET_ROOT/pfnet/chainercv`.

The default value of `$CHAINER_DATASET_ROOT` is `~/.chainer/dataset/`.
