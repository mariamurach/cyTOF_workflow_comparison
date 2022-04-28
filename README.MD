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
   * similiarity graph constructed based on the Jaccard similarity coefficient and based on gthe number of shared nearest neighbors
5. Metaclustering
    1. Automatic
        * Normalized Mutual Information used to score metaclustering. 
    2. Manual 
    3. Authors used secondary clustering analysis to merge clusters and identify subpopulations across patients co-hort after exploration of the clusters manually. (high reproducibility observed after validation). Used this method: https://pubmed.ncbi.nlm.nih.gov/19153861/
6. Differential abundance analysis
    * represent each graph subpopulation by its surface marker centroid. 
    * excluding "normal" cells from the analysis to zoom in on leukemia phenotype 
7. Differential expression analysis
    * Authors identified cells reminescing HSPC on the top of t-SNE plot and more developed cells on the bottom of the plot. 
    * Authors developed an algorithm yo harness the single cell nature of the data (Statistical analysis of response amplitude). SARA examines the entire single-cell distribution of phosphoprotein intensities to detect meaningful changes between two conditions. SARA incorporates a measure of statistical significance through permutation testing, producing estimates that are sensitive to small responsive subsets yet robust to sampling error and noise. 
    * Used CD34+/- to compare primitive vs mature signaling and observed changes only in healthy populationsl, suggesting that in AML CD34 signaling never matures.
    * no surface/signalling marker could be applied across all patients - intratumor heterogeinity 
    * created gene markers for identified surface/signaling markers and checked with gene expression data available for patients 
    * Infered signaling markers performed better in survival analysis (meaning poor outcome) 

Comments: 
* F-statistic - Clustering F-measure. The F-measure is the harmonic mean of the precision and recall according to the equation F = (2 × Pr × Re)/(Pr + Re). Precision (Pr) and recall (Re) can be described in terms of a 2 × 2 contingency table comparing results for a test method—in this case, the results of a cell population identification algorithm—with some reference method—in this case, the results of manual gating by the subject matter expert as the current standard practice—with true positive (TP) defined as the situation in which the positive assignment of the prediction algorithm matches a positive assignment of manual gating, false positive (FP) when the positive assignment of the prediction algorithm matches a negative assignment of manual gating, and false negative (FN) when the negative assignment of the prediction algorithm matches a positive assignment of manual gating. Recall is calculated as TP/(TP + FN); precision is calculated as TP/(TP + FP). F-measure values are always in the interval [0,1], with 1 indicating a perfect prediction. https://www.nature.com/articles/nmeth.2365#Sec22

This measure can be used to compare tested algorithms. 

* repos: https://github.com/jacoblevine/PhenoGraph
* https://github.com/scverse/scanpy/tree/2e98705347ea484c36caa9ba10de1987b09081bf


## Other interesting papers: 
* https://www.cell.com/cell/fulltext/S0092-8674(15)00637-6?_returnURL=https%3A%2F%2Flinkinghub.elsevier.com%2Fretrieve%2Fpii%2FS0092867415006376%3Fshowall%3Dtrue