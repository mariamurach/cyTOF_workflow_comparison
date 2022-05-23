
# F1000 Zurich Pipeline
## 1. Normalization 
* **ArcsinH transformation is used**
    * LOG SCALE DIDN'T WORK BECAUSE OF NEGATIVE VALUES. Flow cytometry data are traditionally displayed on a log scale. On older instruments, data would only be exported with positive numbers and scaled by base 10 log. Data from newer digital machines (e.g. LSRII) can have negative numbers. Our current mathematical laws don't allow taking the log of a negative number, so different transforms are used that can accommodate negative numbers while also displaying data in a log-like fashion.
    * CAN BE TWEAKED BY USER BY CHANGING SCALE (COFACTOR). Another feature of current transformation equations is that they can be tweaked by user-controlled settings to behave differently for regions around zero. For arcsinh scales, this user-controlled setting is called the scale argument. For data values on either side of zero to the magnitude of the scale argument, data are displayed in an linear-like fashion tracking with the raw data values. For values beyond the scale argument, data are displayed in a log-like fashion.
    * FUNCTION = ASINH(value/cofactor) Arcsinh values are calculated by applying the arcsinh equation divided by the scale argument to the measured intensity value. The scale argument defines the size of the linear region around 0. You can get the same answer in Excel by taking an intensity value and using the function ASINH. The syntax is "=ASINH(value/cofactor)"
    * SYMETRIC DISTRIBUTION AND COMPARABLE RANGE FOR CLUSTERING. From f1000 paper - Usually, the raw marker intensities read by a cytometer have strongly skewed distributions with varying ranges of expression, thus making it difficult to distinguish between the negative and positive cell populations. It is common practice to transform CyTOF marker intensities using, for example, arcsinh (inverse hyperbolic sine) with cofactor 58,29 to make the distributions more symmetric and to map them to a comparable range of expression, which is important for clustering.
## MDS plots
*   MDS PLOT IS BASED ON MEDIAN MARKER EXPRESSION median marker expression over all cells to calculate dissimilarities between samples (other aggregations are also possible, and one could reduce the number of top varying markers to include in the calculation). Ideally, samples should cluster well within the same condition, although this depends on the magnitude of the difference between experimental conditions. With this diagnostic, one can identify outlier samples and eliminate them if the circumstances warrant it. An MDS plot on the median marker expressions can be generated with plotMDS(), which internally calls the same-named limma function.
## PCA plots
## NRS score to pick non-redundant markers 
## FLOWSOM and ConsenusClusterPlus for clustering
* **FlowSOM**: 
    1.  building a self-organizing map (SOM), where cells are assigned according to their similarities to 100 (by default) grid points (or, so-called codebook vectors or codes) of the SOM; 
    2. building a minimal spanning tree, which is mainly used for graphical representation of the clusters, is skipped in this pipeline; and 
    3. metaclustering of the SOM codes, is performed with the ConsensusClusterPlus package. 
* uses function from CATALYST cluster() and cluster all cells together 
* uses NRS detected markers for clustering
## Heatmaps
*  **USES MEDIAN MARKER EXPRESSION TRANSFORMED 0-1. DENDROGRAM AND HIERARCHICAL STRUCTURE OF METACLUSTERS IS USING ARCSINH.** 
    * We can then investigate characteristics of identified clusters with heatmaps that illustrate median marker expression in each cluster (Figure 6). As the range of marker expression can vary substantially from marker to marker, we use the 0-1 transformed data for some visualizations (argument scale = TRUE in the respective plotting functions). However, to stay consistent with FlowSOM and ConsensusClusterPlus, we use the (arcsinh-transformed) unscaled data to generate the dendrogram of the hierarchical structure of metaclusters.
* **OPTION TO PLOT EXPRESSION FOR EACH MARKER** Instead of using only medians, which do not give a full representation of cluster specifics, one can plot the entire marker expression distribution in each cluster (Figure 7). Such a plot gives more detailed profile of each cluster, but represents a larger amount of information to interpret. Heatmaps give an overall overview of clusters, are quicker and easier to interpret, and together with the dendrogram can be a good basis for further cluster merging (see Cluster merging and annotation section).
* **OPTION TO PLOT FUNCTIONAL MARKERS AND SIGNALING MARKER**
We propose a heatmap that depicts median expression of functional markers in each sample (Figure 8) such that the potential differential expression can be investigated already at this data exploration step before the formal testing is done. In order to plot all the heatmaps in one panel, we use the ComplexHeatmap package16.
Heatmap on the right represents the median of the arcsinh, 0-1 transformed marker expression for a signaling marker pS6 calculated over cells in each sample (columns) individually.
* For all heatmaps, ComplexHeatmap package is used 
* specify distance as a pre-defined option. The valid values are the supported methods in dist() function and in "pearson", "spearman" and "kendall". The correlation distance is defined as 1 - cor(x, y, method). All these built-in distance methods allow NA values.
a self-defined function which calculates distance from a matrix. The function should only contain one argument. Please note for clustering on columns, the matrix will be transposed automatically.
a self-defined function which calculates distance from two vectors. The function should only contain two arguments. Note this might be slow because it is implemented by two nested for loop.
*   fun = c("median", "mean", "sum"),
* distance	
character string specifying the distance metric to use in dist for hierarchical clustering.
linkage	
character string specifying the agglomeration method to use in hclust for hierarchical clustering.
*   distance = c("euclidean", "maximum", "manhattan", "canberra", "binary", "minkowski"),
* linkage = c("average", "ward.D", "single", "complete", "mcquitty", "median","centroid", "ward.D2")
## Exploration plots 
* UMAP 
* t-SNE
    * The t-SNE algorithm comprises two main stages. 
    1. First, t-SNE constructs a probability distribution over pairs of high-dimensional objects in such a way that similar objects are assigned a higher probability while dissimilar points are assigned a lower probability. 
    2. t-SNE defines a similar probability distribution over the points in the low-dimensional map, and it minimizes the Kullback–Leibler divergence (KL divergence - how simnilar or unsimilar are 2 distributions) between the two distributions with respect to the locations of the points in the map. While the original algorithm uses the Euclidean distance between objects as the base of its similarity metric, this can be changed as appropriate
* MDS
* PCA

## Metaclustering 
* Manual
* Number of clusters picked by elbow criterium 

## Differential analysis
### Differential cell population abundance
* Uses diffcyt package (package specifically for cytometry results)
* Diffcyt allows for:
  method_DA = c("diffcyt-DA-edgeR", "diffcyt-DA-voom", "diffcyt-DA-GLMM"),
  method_DS = c("diffcyt-DS-limma", "diffcyt-DS-LMM")
  DA - differential abundance, DS - differential State
* Limma : linear regression plus Bayes smoothing to the standard errors 
$$Y_i = \beta_0 + \beta_1 X_i + \epsilon_i$$
  Benchmark article: https://www.nature.com/articles/s42003-019-0415-5
* voom: Transform count data to log2-counts per million (logCPM), estimate the mean-variance relationship and use this to compute appropriate observation-level weights. The data are then ready for linear modelling.
* edgeR uses the quantile-adjusted conditional maximum likelihood (qCML) method for experiments with single factor. https://bioconductor.org/packages/release/bioc/vignettes/edgeR/inst/doc/edgeRUsersGuide.pdf
quantile-adjusted conditional maximum likelihood
https://www.pathwaycommons.org/guide/primers/data_analysis/rna_sequencing_analysis/

![](https://www.pathwaycommons.org/guide/primers/data_analysis/rna_sequencing_analysis/figure_quantile_adjust.png)
* Can identify (comprise) random effect in samples to model overdispersion in proportions. 
* Can use GLMM and logit transformation (GENERALIZED LINEAR MIXED MODELS)

Generalized linear mixed models (or GLMMs) are an extension of linear mixed models to allow response variables from different distributions, such as binary responses. Alternatively, you could think of GLMMs as an extension of generalized linear models (e.g., logistic regression) to include both fixed and random effects (hence mixed models). The general form of the model (in matrix notation) is:

<math xmlns="http://www.w3.org/1998/Math/MathML" display="block">
  <mrow data-mjx-texclass="ORD">
    <mi mathvariant="bold">y</mi>
  </mrow>
  <mo>=</mo>
  <mi mathvariant="bold-italic">X</mi>
  <mi mathvariant="bold-italic">&#x3B2;</mi>
  <mo>+</mo>
  <mi mathvariant="bold-italic">Z</mi>
  <mi mathvariant="bold-italic">u</mi>
  <mo>+</mo>
  <mi mathvariant="bold-italic">&#x3B5;</mi>
</math>

Where  is a  column vector, the outcome variable;  is a  matrix of the  predictor variables;  is a  column vector of the fixed-effects regression coefficients (the s);  is the  design matrix for the  random effects (the random complement to the fixed ;  is a  vector of the random effects (the random complement to the fixed ; and  is a  column vector of the residuals, that part of  that is not explained by the model, . To recap:

$$\overbrace{\mathbf{y}}^{{N \times 1}} \quad = \quad
\overbrace{\underbrace{\mathbf{X}}_{{N \times p}} \quad \underbrace{\boldsymbol{\beta}}_{{p \times 1}}}^{{N \times 1}} \quad + \quad
\overbrace{\underbrace{\mathbf{Z}}_{{N \times q}} \quad \underbrace{\boldsymbol{u}}_{{q \times 1}}}^{{N \times 1}} \quad + \quad
\overbrace{\boldsymbol{\varepsilon}}^{{N \times 1}}$$

