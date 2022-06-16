# ACNet

The code repository of paper *ACNet: A Benchmark for Activity Cliff Prediction*


## homepage
Introduction of this project: https://drugai.github.io/ACNet/


## requirement
- pytorch >= 1.11
- numpy >= 1.21.2
- pandas >= 1.2.3
- rdkit >= 2020.09.5
- ogb >= 1.3.3
- pyg >= 2.0.4
- scikit-learn >= 1.0.2

The environment used in our experiment is backuped and uploaded. 
To recover the environmen:

`conda create -f  ACNetEnviron.yml`

## Usage
### Download data files




## Illustration

### Data structure
The data sturcture in the ACNet benchmark is as following:
{'SMILES1':}


## Reproduce

The baseline experiments of ACNet are conducted by *self-made* training package.
To reproduce the results in the manuscript, this package and the experimental scripts are provided in this repository.
However, the usage of this package will not be introduced.
We only guarantee that the experimental scripts can work, other functions of the training package is not our point and will not be guaranteed.

Note that the PyG package uses torch.scatter function, which is not reproducable (See xxxx) So that the results of GCN, GIN, and SGC maybe slightly different with our reported results in the manuscript.
