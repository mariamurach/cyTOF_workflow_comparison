# Comparison of cyTOF analysis workflows

## Citeria: 
1. Input Data
2. QC 
3. Preproccesing
4. Clustering
5. Metaclustering
    1. Automatic
    2. Manual 
6. Differential abundance analysis
7. Differential expression analysis


# 1. Zurich Paper pipeline
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
