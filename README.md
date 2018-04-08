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

### Deliverable 
A module simulating osmFISH per-cell transcript counts
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

 
## Task 2. Optimal selection of target gene panel for osmFISH

Given scRNA-seq data and a set of pre-defined clusters, the aim is to select genes for targeting by a spatial measurement that would optimize ability to recover classification of cells, as measured by osmFISH. 

We suggest to approach this task iteratively, starting with a very simple osmFISH measurement model, and then moving towards a model derived in Task 1. 

### Deliverable 
A module that will predict an optimal set of genes to target with osmFISH measurement.
* Input
   * scRNA-seq data representative of the tissue that will be measured
   * cell clusters that we aim to distinguish or a cell cluster hierarchy (see below)
   * target gene set size
* Output
   * A set of genes to target by osmFISH

For the initial test step, determine an optimal target gene set for osmFISH measurement
* Aiming to recover layer 1 clusters, as determined by Ziesel et al.
* Limiting target set size to: 20, 30, 50 and 100 genes
* Using naive perfect model of osmFISH measurmeent
   * suggested sampling model: binomial subsampling of scRNA-seq molecules, using observed osmFISH cell sizes

For the final step, generate optimal target sets,
* Aim to recover layer 2 clusters
* Use osmFISH measurement model derived in Task 1


### Evaluation
Please evaluate the performance of the target sets with respect to the following features:
1. Cell classificaiton performance
  a. Assess the ability to recover exact cluster classification of the simulated osmFISH cells. The method of classification itself is up to you. If you chose to use a learning-based method, please make sure to separate training and test sets.
  b. Assess the ability to recover broad cell classification within the cell cluster hierarchy. Distinguishing detailed cell subtype (e.g. In5 and In8 interneuron types) is harder than distinguishing major types (e.g. interneurons from pyramidal neurons). Because of that, we would like to compare different osmFISH target sets in their ability to provide broader classification of cells (in cases when the exact cell annotation cannot be determined). Derive a classification measure that takes into account the error of cell placement on the hierarchy (e.g. cophenetic distance between predicted and true class)
        * For deriving layer2 cluster hierarchy, one can use the following [R script](zeisel.hierarchy.Rmd). 

2. Cluster recovery performance. Evaluate ability to recover cell clusters from osmFISH data (i.e. perform de-novo clustering of cells as measured by osmFISH, determine how well the resulting clusters correspond to the scRNA-seq cluster hierarchy).

