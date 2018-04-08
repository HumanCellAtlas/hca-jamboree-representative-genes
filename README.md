# hca-jamboree-representative-genes

## Datasets: 
- Mouse scRNA-seq data from SC ([Zeisel et al. 2015](http://science.sciencemag.org/content/early/2015/02/18/science.aaa1934)), 
- Mouse Oligodendrocytes ([Marques et al. 2016](http://science.sciencemag.org/content/352/6291/1326)) 
- Mouse V1 clusters ([Tasic et al. 2016](https://www.nature.com/articles/nn.4216)). 
- Spatial detection: osmFISH dataset ([Codeluppi et al. 2018](https://www.biorxiv.org/content/early/2018/03/04/276097))
 
## Tasks:

## Task 1. Design a model of osmFISH measurement 
Just like any other technique, osmFISH provides a sampling of the true transcription state. The aim of this exercise is to come up with a statistical description of counts that would be detected by an osmFISH measurement per cell. To approximate this, we will assume that the scRNA-seq data (from the matching tissue), aggregated across a specific cell subpopulation, provides a close approximation of the true transcriptome. 

Some of the features of osmFISH transcript counts that you should consider:
* Cell size variation
* Dropouts
* Mis-assignment of transcript spots to adjacent cells

### Deliverable: module simulating osmFISH per-cell transcript counts
* Input 
   * Target gene panel
   * scRNA-seq data capturing a set of cells to be measured
* Output
   * Per cell gene counts, as expected from detection by osmFISH 

### Suggested route
* Map osmFISH cell clusters to existing  scRNA-seq annotation or custom clusters
   * Define / extract clusters on Zeisel et al. and Marques et al. datasets
   * Use pooled osmFISH gene frequencies to find best matching clusters
* Examine osmFISH count distributions
   * as a function of scRNA-seq expression frequency in different clusters
   * as a function of scRNA-seq expression magnitude
   * variation of detection parameters (e.g. TP/FP) between different genes
* Build a predictor module
   * parameterize distributions
   * code implementation module for simulating osmFISH per-cell counts

 
## Task 2. Find genes to optimize cell classification in spatial experiments

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
