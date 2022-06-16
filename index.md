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
Over 17 million of activities, each of which records the binding affinity of a molecule against a target, are screened by the rules shown in Fig.1.
Referring to the rules in previous work, compounds trialed against single human targets (organism = Homo sapiens) in direct interaction binding assays (assay_type = B, relationship_type = D) at highest assay confidence (confidence_score = 9) are reserved to construct the benchmark.
Assay-independent equilibrium constants ($pK_i$ values) are used as the potency measurement.
Salt compounds are discarded.
Multiple measurements of the same compound against the same target are averaged if all values fell within the same order of magnitude.
Otherwise, this activity record is discarded.
As a result, 142,307 activities are reserved in our benchmark.

Next, to identify pairs of molecules exhibiting AC relationships against each target, all of the activities are treated separately according to the targets.
Matched Molecular Pair is selected as the similarity criterion due to the widely application in previous AC prediction works.
All possible MMPs are identified by the algorithm proposed by Hussain~\textit{et al.}.
Size restrictions of the substituents are also applied, referring to the rules in previous work.
First, a substituent is restricted to contain at most 13 heavy atoms, and the core have to be at least twice as large as the substituent.
Second, the difference between the substituents of an MMP is restricted to be at most 8 heavy atoms.
These restrictions make the identified MMPs consistent with the typical structural analogues in practice.

For each MMP, if the difference in potency is greater than 100-fold ($\Delta pK_i \geq 2.0$), then the MMP is considered as an MMP-cliff with a positive label.
And if the potency difference is lower than 10-fold, then the MMP is denoted as a non-AC MMP with a negative label.
This criterion involve a distinct margin between the potency differences of positive samples and those of negative samples, so that the influence of the observational error induced by the source assays can be restricted.
Based on the above-mentioned data collection method and screening rules, we have found a total of 21,352 MMPs exhibiting AC relationships, and 423,282 negative non-AC MMPs.
Examples of the data in ACNet are shown in Fig.~\ref{fig:dataset}.

![1](construction.pdf)


### Paper

- Under review by NeurIPS 2022 Track Datasets and Benchmarks

### Code & Data

- Code: https://github.com/DrugAI/ACNet

- Data: https://drive.google.com/drive/folders/1JogBAg9AI0pUxY44w9_g8RHboLf7V5q7?usp=sharing
