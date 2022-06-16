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
Download data files from URL:  https://drive.google.com/drive/folders/1JogBAg9AI0pUxY44w9_g8RHboLf7V5q7?usp=sharing

Run the following command to put the data files into the directories

```
mkdir ./ACComponents/ACDataset/data_files/raw_data
mkdir ./ACComponents/ACDataset/data_files/generated_datasets
mv all_smiles_target.csv ./ACComponents/ACDataset/data_files/raw_data/
mv mmp_ac_s_distinct.csv ./ACComponents/ACDataset/data_files/raw_data/
mv mmp_ac_s_neg_distinct.csv ./ACComponents/ACDataset/data_files/raw_data/
```

### Generate ACNet datasets

Run the following command to generate ACNet datasets with *Default Configuration*

```
python ./ACComponents/ACDataset/GenerateACDatasets.py
```

The configuration can be customized in *./ACComponents/ACDataset/DataUtils.py*




## Illustration
### Data files

### Data structure
The data sturcture in the ACNet benchmark is as following:
{'SMILES1':}


## Reproduce

The baseline experiments of ACNet are conducted by *self-made* training package.
To reproduce the results in the manuscript, this package and the experimental scripts are provided in this repository.
However, the usage of this package will not be introduced.
We only guarantee that the experimental scripts can work, other functions of the training package is not our point and will not be guaranteed.

Note that the PyG package uses torch.scatter function, which is not reproducable (See xxxx) So that the results of GCN, GIN, and SGC maybe slightly different with our reported results in the manuscript.
