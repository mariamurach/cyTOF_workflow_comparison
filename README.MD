# Comparison of cyTOF analysis workflows

## Criteria: 
1. Input Data
2. QC 
3. Preproccesing
4. Clustering
5. Metaclustering
    1. Automatic
    2. Manual 
6. Differential abundance analysis
7. Differential expression analysis


## 1. Zurich Paper pipeline
https://doi.org/10.12688/f1000research.11622.4

1. Input Data 
    * Object of class SingleCellExperiment
2. QC 
    * Manual, based on heatmaps, UMAP, PCA, tSNE plots 
3. Preproccesing
    * Build in wrapper function to use archsin inverse transform on all data
4. Clustering
    * Self Organizing Maps
5. Feature Selection 
    * Non-redundancy score
6. Metaclustering
    1. Automatic
    2. Manual 
7. Differential abundance analysis
    * GLMM, LMM, LM models

## 2. PhenoGraph
https://www.cell.com/cell/fulltext/S0092-8674(15)00637-6?_returnURL=https%3A%2F%2Flinkinghub.elsevier.com%2Fretrieve%2Fpii%2FS0092867415006376%3Fshowall%3Dtrue

New method of dimensionality reduction.

1. Input Data
2. QC 
3. Preproccesing
4. Clustering
   * Non-parametric dimensions reduction. Constructing a nearest-neighbor graph, finding highly interconnected nodes (density detection problem)
   * "To this end, we borrow from the social network field, which has developed powerful algorithms to partition large social networks into communities" 
   * similiarity graph constructed based on the Jaccard similarity coefficient
5. Metaclustering
    1. Automatic
        * Normalized Mutual Information used to score metaclustering. 
    2. Manual 
6. Differential abundance analysis
    * represent each graph subpopulation by its surface marker centroid. 
    * excluding "normal" cells from the analysis to zoom in on leukemia phenotype 
7. Differential expression analysis






## Other interesting papers: 
* https://www.cell.com/cell/fulltext/S0092-8674(15)00637-6?_returnURL=https%3A%2F%2Flinkinghub.elsevier.com%2Fretrieve%2Fpii%2FS0092867415006376%3Fshowall%3Dtrue