**Background**
Tasic et al. (2016) used scRNA sequencing to explore the extent of cell types of mouse primal visual cortex. In the study, 1723 high-quality cells with thousands of genes in primal visual cortex in male adult mice were sequenced on a single cell resolution for cell classification. This study identified 49 cell types, including 23 GABAergic, 19 glutamatergic and 7 non-neural types in the 1723 cells.

The cell classification result from Tasic et al. (2016) was used by Zhu et al. (2018) to design a study to distinguish the difference between intrinsic and extrinsic effect on gene expression. Intrinsic effect means the regulatory gene network, while extrinsic means the cellular microenvironment. The study was conducted by combining scRNA sequencing data and smFISH data (single-molecule fluorescence in situ hybridization). The former one has high molecular resolution of transcriptomics (thousands of genes) without spatial information, while the latter keeps the spatial information but loses the high resolution (only a few hundred genes).

Zhu et al. (2018) mapped the sRNA sequencing data to the seqFISH data to enhance the molecular resolution of the cells using SVM (support vector machine). The model was trained to identify the major cell type difference by training on scRNA data of 8 groups, GABAergic and Glutamatergic are the major neuron types, and other non-neuronal types, including Astrocytes, Mndothelial cells, microcytes, and three types of Oligocytes. The selected features (genes) were the top 43 differentially expressed (DE) genes in the identified 113 genes. The classification result was FURTHER validated by different evidence like cell type specific staining and previously reported marker genes.

**Task**

Now, given a dataset of gene expression levels of 1723 cells and 113 genes and the cell types (from Tasic et al. 2016), our task is to predict the cells provided by Zhu et al. (2018), 1597 cells with 113 genes.

**Outline of analysis**

We will adopt a semi-supervised approach to classify the cells of seqFISH data.

- Train an sPLS-DA model on scRNA dataset.
- Select genes by limiting the number of genes of "keepX" argument of each component during hyperparameter tuning.
- Predict seqFISH data using the trained sPLS-DA model.
- The predictions with high probabilities by the sPLS-DA model will be combined to the original training data for further training.
- Train a MINT model on the combined training dataset and predict the rest cells of seqFISH data.
- Identify the marker genes by examining the loading factors.

This semi-supervised method can borrow the information from the seqFISH data and make the model customised. The combination of the data will be used to train a MINT.sPLS-DA model. MINT (Multivariate INTegrative method) (Rohart et al. 2017) is robust for integrating data from different sources regardless of the batch effect. The top discriminative genes identified from the model will be validated using previous literature as evidence. The determination of the minimal number of genes will be done by restricting the values of keepX and the performance will be monitored by balanced error rate (BER). BER is the average of the proportion of wrong classifications in each class. The lower the BER, the more accurate the model is.

