# Compressed Sensing using Generative Models

This repository provides code to reproduce results from the paper: [Compressed Sensing using Generative Models](https://arxiv.org/abs/1703.03208)

### Requirements: 
---

1. [Tensorflow](https://www.tensorflow.org/install/)
2. [PyPNG](http://stackoverflow.com/a/31143108/3537687)
3. [matplotlib](http://matplotlib.org/users/installing.html)
4. [PyWavelets](http://pywavelets.readthedocs.io/en/latest/#install)
5. [CVXOPT](http://cvxopt.org/install/index.html)

### Preliminaries
---

Download the datasets
```shell
$ python download.py mnist
$ python download.py celebA
```

Download and extract pretrained models

```shell
$ wget https://www.cs.utexas.edu/~ashishb/csgm/csgm_pretrained.zip
$ unzip csgm_pretrained.zip
$ rm csgm_pretrained.zip
```

To use wavelet based estimators, you need to run ```$ python ./src/wavelet_basis.py``` to create the basis matrix.

### Experiments
---
These are the supported experiments:

1. Reconstruction from Gaussian measurements
2. Super-resolution
3. Reconstruction for images in the span of the generator (gen-span)
4. Quantifying representation error (projection)
5. Inpainting

For a quick demo of these experiments on MNIST, run ```$ python ./quick_scripts/mnist_{expt}.sh```. For quick demo on celebA, identify an image on which you wish to run it, and run ```$ python ./quick_scripts/celebA_{expt}.sh "/path/to/image"```

### Reproducing quantitative results
---

For MNIST, we just use the standard test set. For celebA, we use a subset of images **not** used while training.
```shell
$ mkdir data/celebAtest
$ wget https://www.cs.utexas.edu/~ashishb/csgm/celebA_unused.txt
$ while read f; do mv data/celebA/$f data/celebAtest/; done <celebA_unused.txt
```

Now follow these steps:

1. Create a scripts directory ```$ mkdir scripts```

2. Identfy a dataset you would like to get the quantitative results on. Locate the file ```./quant_scripts/{dataset}_reconstr.sh```.

3. Change ```BASE_SCRIPT``` in ```src/create_scripts.py``` to be the same as given at the top of ```./quant_scripts/{dataset}_reconstr.sh```.

4. Optionally, comment out the parts of ```./quant_scripts/{dataset}_reconstr.sh``` that you don't want to run.

5. Run ```./quant_scripts/{dataset}_reconstr.sh```. This will create a bunch of ```.sh``` files in the ```./scripts/``` directory, each one of them for a different parameter setting.

6. Start running these scripts. They will save the results to appropriately named directories.
    - You can run ```$ ./utils/run_sequentially.sh``` to run them one by one.
    - Alternatively use ```$ ./utils/run_all_by_number.sh``` to create screens and start proccessing them in parallel. [REQUIRES: gnu screen][WARNING: This may overwhelm the computer]. You can use ```$ ./utils/stop_all_by_number.sh``` to stop the running processes, and clear up the screens started this way.

7. Create a results directory : ```$ mkdir results```

8. Reconstructed images, can be found in ```estimated/```. To get the plots, see ```src/metrics.ipynb```. To get the matrix of images (as in the paper), see ```src/view_est_{dataset}.ipynb```.