MN-BaB <img width="100" alt="portfolio_view" align="right" src="http://safeai.ethz.ch/img/sri-logo.svg">
======== 
Multi-Neuron Guided Branch-and-Bound ([MN-BaB](https://www.sri.inf.ethz.ch/publications/ferrari2022complete)) is a state-of-the-art complete neural network verifier that builds on the tight multi-neuron 
constraints proposed in [PRIMA](https://www.sri.inf.ethz.ch/publications/mueller2021precise) and leverages these constraints within a BaB framework to yield an efficient, GPU based dual solver.
MN-BaB is developed at the [SRI Lab, Department of Computer Science, ETH Zurich](https://www.sri.inf.ethz.ch/) as part of the [Safe AI project](http://safeai.ethz.ch/).

This version is an adaptation of the [VNN-COMP'22](https://arxiv.org/abs/2212.10376) entry allowing for the certification of models trained with the novel certified training method [SABR](https://openreview.net/forum?id=7oFuxtJtUMH), without modifications. 

### Cloning
This repository contains a submodule. Please make sure that you have access rights to the submodule repository for cloning. After that either clone recursively via 

```
git clone --branch SABR_ready --recurse-submodules https://github.com/eth-sri/mn-bab
```

or clone normally and initialize the submodule later on

```
git clone --branch SABR_ready https://github.com/eth-sri/mn-bab
git submodule init
git submodule update
```

There's no need for a further installation of the submodules.


### Installation
Create and activate a conda environment:

```
  conda create --name MNBAB python=3.8 -y
  conda activate MNBAB
  ```

This script installs a few necessary prerequisites including the ELINA library and GUROBI solver and sets some PATHS. It was tested on an AWS Deep Learning AMI (Ubuntu 18.04) instance.

```
source setup.sh
```

Install remaining dependencies:
```
python3 -m pip install -r requirements.txt
PYTHONPATH=$PYTHONPATH:$PWD
```

Download the full MNIST, CIFAR10, and TinyImageNet test datasets in the right format and copy them into the `test_data` directory:  
[MNIST](https://files.sri.inf.ethz.ch/sabr/mnist_test_full.csv)  
[CIFAR10](https://files.sri.inf.ethz.ch/sabr/cifar10_test_full.csv)  
[TinyImageNet](https://files.sri.inf.ethz.ch/sabr/tin_val.csv) 

### Containers
We provide Container Definitions for Docker and Apptainer (formerly Singularity), which is more often found on HPC Clusters as it does not require root privileges.

#### Docker
1. Refer to https://docs.docker.com/engine/install/ for installation of Docker.
2. Make sure you have installed the CUDA Toolkit and a corresponding NVIDIA driver with CUDA >= 12.4 (https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html) as well as the nvidia-container-toolkit (https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
3. Make sure the docker service is running
  ```
  sudo su
  systemctl start docker
  ```
4. Build the Docker Image with Example Tag `mn-bab-docker`
```
docker build -t mn-bab-docker -f ./containers/Dockerfile .
```

For an interactive shell inside of the Docker Container, execute:
```
docker run -it --gpus all mn-bab-docker /bin/bash
```
#### Apptainer
1. Refer to https://apptainer.org/docs/admin/main/installation.html for the installation of Apptainer.
2. Build the Apptainer `.sif` image.
```
apptainer build ./mn_bab.sif ./containers/apptainer.def
```
3. Create Apptainer Overlay to make container filesystem writable
```
mkdir ./apptainer_overlay
```
Now, you can run a shell inside the container with GPU Support and using the previously created overlay using:

```
apptainer shell --nv --overlay ./apptainer_overlay mn_bab.sif
```
The MN-BaB files can be found under `/app/`

Additionally, you can start verification directly by running, e.g., 

```
apptainer exec --overlay ./apptainer_overlay --nv ./mn_bab.sif python3 /app/mn-bab/src/verify.py -c /app/mn-bab/configs/baseline/mnist_conv_big.json
```

### Example usage

```
python src/verify.py -c configs/cifar10_conv_small.json
```

Contributors
----------------------
* [Claudio Ferrari ](https://github.com/ferraric) - c101@gmx.ch
* [Mark Niklas Müller](https://www.sri.inf.ethz.ch/people/mark) - mark.mueller@inf.ethz.ch  
* [Nikola Jovanovic](https://www.sri.inf.ethz.ch/people/nikola) - nikola.jovanovic@inf.ethz.ch
* [Robin Staab]()
* [Dr. Timon Gehr](https://www.sri.inf.ethz.ch/people/timon)

Citing This Work
----------------------

If you find this work useful for your research, please cite it as:

```
@inproceedings{
    ferrari2022complete,
    title={Complete Verification via Multi-Neuron Relaxation Guided Branch-and-Bound},
    author={Claudio Ferrari and Mark Niklas Mueller and Nikola Jovanovi{\'c} and Martin Vechev},
    booktitle={International Conference on Learning Representations},
    year={2022},
    url={https://openreview.net/forum?id=l_amHf1oaK}
}
```

License and Copyright
---------------------

* Copyright (c) 2022 [Secure, Reliable, and Intelligent Systems Lab (SRI), Department of Computer Science ETH Zurich](https://www.sri.inf.ethz.ch/)
* Licensed under the [Apache License](https://www.apache.org/licenses/LICENSE-2.0)