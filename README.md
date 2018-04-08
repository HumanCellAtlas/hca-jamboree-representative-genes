# hca-jamboree-representative-genes

## Datasets: 
- Mouse scRNA-seq data from SC (Zeisel et al. 2015), 
- Oligodendrocytes (Marques et al. 2016) and V1 clusters (Tasic et al. 2016). 
- an example of spatial detection: osmFISH dataset (Linnarsson lab)
 
## Tasks:

## 1. Design a model of spatial measurement efficiency

A model of spatial measurement efficiency simulates the number of detected transcripts in the cell in the context of a spatial measurement, given a set of genes and a cell of pre-defined expression profile. For the purpose of this task, we will assume that scRNA-seq data is close to ground truth in terms of expression.

### Problems to be solved:

- Gene detection rates vary from one gene to another
	- examine mean scRNA-seq vs. spatial counts
- Transcripts in cell processes can be incorrectly assigned to neighboring cells after segmentation. For each gene, estimate the probability of false counts (should be inversely related to localization of transcripts to cell soma).
	- dropout and leakage of transcripts
	- look at examples of genes that are expected to have 0 spatial counts in specific cell types (based on scRNA-seq), but have some non-0 counts.
 
## 2. Find genes to optimize cell classification in spatial experiments

Given scRNA-seq data and a set of pre-defined clusters, select genes for targeting by a spatial measurement that would optimize ability to recover classification of cells given a set of constraints. For example: 

- fewer than N total genes 
- restrictions on gene length 
- gene expression magnitude? 

### Task goals:

- optimize target gene selection under a perfect measurement model
- optimize target gene selection under a simple failure model where there’s an X% probability that a particular gene will fail to be detected (?) in the spatial measurement. 
- optimize target gene selection under the spatial measurement model derived in task 1.
- optimize recovery of broad classes over leaves
- calculate trade-off between cell type mapping resolution and robustness (i.e. use extra markers for redundancy vs. mapping to leaves)
- order target gene selection based on mapping information provided (e.g. SLC17A7 may be considered more informative than CHODL since the former can map many cells into broad classes while the latter identifies only a small number of cells as a rare interneuron type)

## Evaluation:

Multiple teams will take part in the challenge, and they will be asked to evaluate cell recovery (Task 2) under each-other’s model (Task 1).

### Evaluation metrics:

- scRNA-seq => select genes ⇒ generate FISH counts (ignore x,y coordinates) ⇒ evaluate consistency of structure with scRNA (clusters, in our case; similar to task 1)

- generate FISH counts:
  - Clean model -- uniformly sample cells from scRNA-seq; downsample counts (binomial with a “good” fixed rate); report downsampled counts of the selected genes.
  - Less clean model - vary the binomial loss rate
  - Complete gene loss
- Learn leakage and loss model:
  - Assume 1-1 mapping of FISH/seq clusters
  - “Ground truth” from single cell clusters (average, other stats)
  - Compare to respective clusters from FISH
- Consistency of the local neighborhood
