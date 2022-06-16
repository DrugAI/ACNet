## ACNet: a benchmark for Activity Cliff Prediction

<center>
  Ziqiao Zhang<sup>1</sup>, Bangyi Zhao<sup>1</sup>, Ailin Xie<sup>1</sup>,  Yatao Bian<sup>2</sup>, Shuigeng Zhou<sup>1</sup>
  <br>
<sup>1</sup>School of computer science, Fudan University <br>
<sup>2</sup>Tencent AI Lab <br>
<br>
xxx
</center>


### Project Description
The Activity Cliffs (ACs), which are generally defined as pairs of structurally similar molecules that are active against the same bio-target but have large difference in the binding potency, are of great interest to the community of drug discovery.
However, the AC prediction task, i.e. to predict whether a pair of similar molecules exhibit AC relationship, have not been fully explored yet.
This work introduces ACNet, a large-scale benchmark for Activity Cliff prediction tasks.
ACNet curates over 400K Matched Molecular Pairs (MMPs) against 190 targets, including over 20K MMP-cliffs and 380K non-AC MMPs, and provides five subsets for model development and evaluation.
The performance of 15 baseline molecular property prediction models adopted as molecular representation encoders for the AC prediction task are evaluated in the experiments.
The traditional FingerPrint-based method shows superiority over other complex deep learning models on these tasks.
And the imbalanced, low-data and out-of-distribution features of the ACNet benchmark make it challenging for the existing molecular property prediction models to cope with.
Our work is the first contribution of large-scale benchmark for the AC prediction task, with the hope to stimulate the study of AC prediction models and prompt further breakthrough in AI-aided drug discovery.


#### Data Collection
The data of ACNet are collected from publicly available database ChEMBL (version 28).
Over 17 million of activities, each of which records the binding affinity of a molecule against a target, are screened by the rules shown in the following figure.
As a result, 142,307 activities are reserved in our benchmark.

Next, to identify pairs of molecules exhibiting AC relationships against each target, all of the activities are treated separately according to the targets.
Matched Molecular Pair is selected as the similarity criterion due to the widely application in previous AC prediction works.
All possible MMPs are identified by the algorithm proposed by Hussain *et al.*.
Size restrictions of the substituents are also applied, referring to the rules in previous work.
These restrictions make the identified MMPs consistent with the typical structural analogues in practice.

For each MMP, if the difference in potency is greater than 100-fold, then the MMP is considered as an MMP-cliff with a positive label.
And if the potency difference is lower than 10-fold, then the MMP is denoted as a non-AC MMP with a negative label.
This criterion involve a distinct margin between the potency differences of positive samples and those of negative samples, so that the influence of the observational error induced by the source assays can be restricted.
Based on the above-mentioned data collection method and screening rules, we have found a total of 21,352 MMPs exhibiting AC relationships, and 423,282 negative non-AC MMPs.

<img width="1247" alt="image" src="https://user-images.githubusercontent.com/49937476/174082762-24f300d7-2218-4134-9a6f-780c35011372.png">

#### Data Organization
In ACNet, each sample represents an MMP, and the label of each sample indicates whether it exhibits an AC relationship against a certain target.
It is intuitive to organize the samples against different targets into different prediction tasks.
To construct dataset for each task, positive samples and negative samples against the same target should be gathered first.
In this step, a threshold is applied to screening out the tasks with extremely few positive samples, since the scarceness of positive samples brings little information of the tasks, and it is too tough for a deep learning model to be trained on these tasks.
The threshold can be adjusted via the configuration file customized by users, and tasks with fewer than 10 positive samples will be discarded by default.

In addition, the number of positive samples is much smaller than that of the negative ones in each task, so that the tasks in the ACNet benchmark are generally imbalanced.
When constructing the dataset of a task, users can customize whether to use the overall negative samples to construct an imbalanced dataset, or randomly choose negative samples to generate a relatively balanced set.
By default, we use all of the negative samples for all tasks to generate imbalanced datasets.

Under the default configuration, ACNet contains MMPs against 190 targets, i.e., 190 tasks.
And the numbers of samples in each task range from 36 to 26,376.
As the number of tasks is large and the data volume of each task varies greatly, for the convenience of model evaluation and comparison, we divide the original 190 tasks into several groups according to the task size.
By default, tasks with more than 20,000 samples are organized as the Large subsets, tasks with 1,000 to 20,000 samples forms the Medium subsets, and tasks with 100 to 1,000 samples are curated as the Small subsets, finally tasks with less than 100 samples constitute the Few subsets.
The thresholds for dividing tasks into subsets can also be customized by the configuration file.

Subsets | #tasks | threshold | #samples
--- | --- | --- | ---
Large | 3 | > 20000 | 72,233
Medium | 64 | [1000,20000] | 275,927
Small | 110 | [100,1000] | 53,084
Few | 13 | < 100 | 835
--- | --- | --- | ---
Mix | 1 | - | 278,367


#### Domain Generalization via Target Splitting

In the previous subsection, samples against different targets are organized into different predictive tasks.
Models can be trained on these tasks separately to learn the knowledge about the chemical modifications leading to large potency difference against a certain target.
However, there may be common knowledge unveiling what chemical modifications are more probable to cause a large difference in binding potency.
Thus, an AC prediction model is expected to learn such common knowledge from 
samples against different targets to better understand the latent principles behind the  ACs phenomenon and the structural similarities between molecules from a potency-based perspective.

In addition, in the field of AI-aided drug discovery, the *low-data* phenomenon is ubiquitous.
Research-valuable targets are typically newly discovered and lack of labeled data, so that it is obviously difficult to train deep models to accurately make predictions only by the data of these low-data tasks.
Consequently, a model may be expected to be trained on other tasks with adequate data to learn common knowledge about AC relationships to benefit the tasks with low-data feature.

Motivated by the above-mentioned observations, we propose an extra Mix subset where all of the samples against different targets are organized into a single task to construct a \textit{mixed} dataset.
To avoid ambiguity, MMPs with conflicting labels against different targets are discarded.
The number of samples in the Mix subset is 278,367, as shown in Tab.~\ref{tab:arrangement}.
To force deep models to learn common knowledge from the Mix subset, a \textbf{target splitting} method is proposed, referring to the scaffold splitting method in the molecular property prediction tasks.
Specifically, when splitting the Mix subset into train/valid/test sets, samples that against the same target must be split into the same set.

The Mix subset consists of samples against different targets, of which the data distribution are discrepant. 
And the target splitting method makes the data for training and evaluating be sampled from different data distributions.
In this case, the prediction task of the Mix subset is a \textbf{domain generalization} problem, which consequently brings Out-Of-Distribution (OOD) feature to ACNet.

Domain generalization, i.e. out-of-distribution generalization, focuses on the problem that learning a model from one or several different but related domains (data distributions) to generalize on unseen testing domains, which is ubiquitous in real-world scenarios~\cite{wang2022generalizing}.
As traditional deep learning models are trained based on the independent identically distributed (i.i.d.) hypothesis, i.e., data for training and testing are sampled independently from identical distribution~\cite{zhang2021deep}, tasks with OOD feature is of great challenge for deep learning models and will lead to performance degradation in \textit{distribution shifting} situations~\cite{2022arXiv220109637J}.
So, 
the OOD feature of the Mix subset will dramatically increase the difficulty for deep models.


### Paper

- Under review by NeurIPS 2022 Track Datasets and Benchmarks

### Code & Data

- Code: https://github.com/DrugAI/ACNet

- Data: https://drive.google.com/drive/folders/1JogBAg9AI0pUxY44w9_g8RHboLf7V5q7?usp=sharing
