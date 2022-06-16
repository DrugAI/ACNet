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
To recover the environment:

`conda create -f  ACNetEnviron.yml`

## Usage 
### Download data files
Download data files from [here](https://drive.google.com/drive/folders/1JogBAg9AI0pUxY44w9_g8RHboLf7V5q7?usp=sharing)

Run the following command to put the data files into the directories

```
mkdir ./ACComponents/ACDataset/data_files
mkdir ./ACComponents/ACDataset/data_files/raw_data
mkdir ./ACComponents/ACDataset/data_files/generated_datasets
mv all_smiles_target.csv ./ACComponents/ACDataset/data_files/raw_data/
mv mmp_ac_s_distinct.csv ./ACComponents/ACDataset/data_files/raw_data/
mv mmp_ac_s_neg_distinct.csv ./ACComponents/ACDataset/data_files/raw_data/
```

### Generate ACNet datasets

Run the following command to generate ACNet datasets with **Default Configuration**

```
python ./ACComponents/ACDataset/GenerateACDatasets.py
```

The genearated dataset files are in `./ACComponents/ACDataset/data_files/generated_datasets/`

The configuration can be customized in `./ACComponents/ACDataset/DataUtils.py`


```
class Config(object):
    def __init__(self):
        super(Config, self).__init__()
        self.mixed = True                       # Whether to generate the Mix dataset, default True
        self.random_sample_negative = False     # Whether to use randomly sample the negative samples toward certain target, default False.
        self.random_sample_negative_seed = 8    # Random seed for sample negative samples if self.random_saple_negative == True.
        self.discard_extreme_imbalance = False  # Whether to discard the subsets that are extremely imbalanced, default False.
        self.pn_rate_threshold = 0.2            # The threshold of Pos/Neg for extremely imbalanced subsets if self.discard_extreme_imbalance == True.
        self.discard_few_pos = True             # Discard the subsets that have only few positive samples. Default True.
        self.few_pos_threshold = 10             # The threshold to identify few positive, deault 10.
        self.large_thres = 20000                # Thresholds for grouping tasks into subsets.
        self.medium_thres = 1000
        self.small_thres = 100
```

### Reproducing

Our experimental results are obtained on RTX 3090 GPU, E5-2667 CPU, 256GB memory, and Ubuntu 18.04.5.

To reproduce the results, execute the following steps.



- Create the directories for model checkpoints.

```
mkdir ./TestExp
mkdir ./TestExp/Large
mkdir ./TestExp/Medium
mkdir ./TestExp/Small
mkdir ./TestExp/Few
mkdir ./TestExp/Mix
```

- Run the scripts in `./`

For instance, to run the ECFP+MLP on Large subset:

```
mkdir ./TestExp/Large/FPMLP
python ./FPMLPLarge.py
```

**Note**:
The GNNs (GCN, GIN, SGC) in the baseline experiments are implemented by PyG package, which uses `torch.scatter_` function.
Remember that the `torch.scatter_` function is not reproducible (See [here](https://pytorch.org/docs/stable/generated/torch.Tensor.scatter_.html#torch.Tensor.scatter_) ), so the results of the GNNs may be slightly different with our reported results in the manuscript.


**Note**:
The baseline experiments of ACNet are conducted by *self-made* training framework.
It is not as well-constructed as other training frameworks, e.g. *torchdrug*.
It is just served as an example to show how our benchmark works and to show the reproducibility of our results reported in the manuscript.
We can only guarantee that the experimental scripts can work to reproduce the results, but the stability of the training framework is not guaranteed and the illustration of this training framework is not our point.




## Illustration
### Data files

### Data structure
The data sturcture in the ACNet benchmark is as following:
{'SMILES1':}

