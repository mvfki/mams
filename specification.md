# General fields

**Field:** class  
**Value:** Character string  
**Description:** Denotes the class of the matrix, array, data frame. One of `fom`, `fam`, `oam`, `fid`, or `oid`.   
**Notes:** Each class will have a correpsonding set of metadata fields.

# FOM class 

**General description:** A feature and observation matrix (FOM) is a data matrix that contains measurements of molecular features in biological entities. Examples of features include genes, genomic regions or peaks, transcripts, proteins, antibodies derived tags, signal intensities, cell type counts. Examples of observations include cells, cell pools, beads, spots, subcellular regions, and regions of interest (ROIs). Measurements may include transcript counts, protein abundances, signal intensities and velocity estimates. The main elements of a fom are the central feature-observation data matrix (fom), observation ID vector or matrix (oid), feature ID vector or matrix (fid), feature annotation matrix (fam), and observation annotation matrix (oam).

**Field:** fom_id  
**Value:** Character string  
**Description:** Denotes the unique id of the FOM and should be unique within the scope of the dataset.  
**Notes and considerations for implementation:** This ID could be a randomized unique ID such as a UUID or could be a unique combination of other fields from the FOM schema. For example, the ID could be a combination of `modality` and `processing` fields with the option of including algorithm_name. In this scenario, the ids “rna.raw”, “rna.normalize”, “rna.scale” could be used describe the data matrices and “rna.reduction.pca” and “rna.embedding.umap” could be used to describe the reduced dimensional objects.  

**Field:** class  
**Value:** Charcater string  
**Description:** Must be equal to "fom".  

## Matrix description fields

**Field:** data_type  
**Value:** Character string    
**Description:** Explicitly describes the type of data stored in the FOM (e.g. int, int64, double, enum/categorical, etc). 
**Notes and considerations for implementation:**  

**Field:** representation  
**Value:** Character string    
**Description:** Preferred representation of the matrix.  
**Notes and considerations for implementation:**  

**Field:** representation_description  
**Value:** Character string    
**Description:** A brief description of the `representation` tag 

**Suggested categories for `representation` and `representation_description`:**

| representation | representation_description | Example |
| -------------- | ---------------------------| ----------------|
| sparse | The matrix contains zeros for the majority of the measurements. |
| dense | The matrix contains non-zeros for the majority of the measurements. |

**Field:** obs_unit  
**Value:** Character string    
**Description:** Biological unit of the observations  

**Field:** obs_unit_description  
**Value:** Character string    
**Description:** A brief description of the `obs_unit` tag 

**Suggested categories for `obs_unit` and `obs_unit_description`:**

| obs_unit | obs_unit_description | Notes/Examples |
| -------------- | ---------------------------| ----------------|
| bulk | Features are quantified for a collection of cells (e.g. bulk) such as tissue or culture | Bulk RNA-seq or ATAC-seq |
| cell | Features are quantified for individual cells | Single-cell RNA-seq or single-cell ATAC-seq |


**Field:** processing  
**Value:** Character string    
**Description:** Used to describe the nature of the data contained within the matrix.  
**Notes and considerations for implementation:** This field will help distinguish between the matrices that are produced at different stages of an analysis workflow. This field should not be used to infer a history of previous steps in the analysis workflow. So users should not have to use a particular sequence of tags and should not assume that each matrix has gone through a set of prior tags. Rather, the history of previous steps should be captured in the provenance field.   

**Field:** processing_description  
**Value:** Character string    
**Description:** A brief description of the `processing` tag.   

**Suggested categories for `processing` and `processing_description`:**

| processing | processing_description | Notes/Examples |
| ---------- | -----------------------| ------- | 
| raw | Original measurements have not been altered |
| counts | Raw data for assays that produce integer-like data such as scRNA-seq. Child of `raw`.|  |
| intensities | Raw data for assays that produce continuous data. Child of `raw`. | |
| decontaminated | Measurements have been corrected for background signal such as ambient RNA in single-cell RNA-seq. Child of `raw`. | |
| lograw | The log of the raw data |
| logcounts | The log of the raw counts Child of `lograw`. | |
| logintensities | The log of the raw intensity values. Child of `raw`.| |
| corrected | Measurements have been corrected for observation-level covariates | |
| normalized | Data that has been normalized for differences in overall signal abundance between observations. | |
| lognormalized | Data that has been log transformed after normalizing for differences in overall signal abundance between observations. Child of `normalized`.| |
| centered | Data with features have been made to center around a standard quantity such as the mean or median. | Mean-centered data |
| scaled | Data with features have been centered around a standard quantity and standardized to have similar variances or ranges. | Z-scored data |
| reduction | A matrix containing a data dimensionality reduction generally useful for input into tools for downstream analysis such as clustering or 2D-embedding. | PCA, ICA, Autoencoders |
| embedding | A matrix containing a low dimensional embedding (usually 2D or 3D) generally used for visualization. Child of `reduction` | UMAP, tSNE | 

**Field:** analyte  
**Value:** Character string    
**Description:** Used to describe the biological analytes being measured in the matrix.
**Notes and considerations for implementation:** This field will help distinguish between the matrices that are produced at different stages of an analysis workflow. This field should not be used to infer a history of previous steps in the analysis workflow. So users should not have to use a particular sequence of tags and should not assume that each matrix has gone through a set of prior tags. Rather, the history of previous steps should be captured in the provenance field.   

**Field:** analyte_description  
**Value:** Character string    
**Description:** A brief description of the `analyte` field.   

**Suggested categories for `analyte` and `analyte_description`:**

| analyte | analyte_description | Notes/Examples |
| ------- | ------------------- | --------------- |
| rna | Used for technologies that measure RNA expression levels. | This should generally be used for assays listed under “RNA assay” (EFO_0001457) from the OLS. |
| dna | Used for technologies that measure features of DNA. | This should generally be used for assays listed under “DNA assay” (EFO_0001456) from the OLS. |
| chromatin | Used for technologies that measure open chromatin regions of DNA. |
| protein | Used for technologies that measure protein expression levels. | This should generally be used for assays listed under “protein assay” (EFO_0001458) from the OLS. Examples include CITE-seq, Total-seq, CODEX, MIBI. |
| morphology | Used for morphological measurements often derived from imaging technologies (e.g. cell size or shape). |
| lipid | Used for technologies that measure lipid levels. |
| metabolite | Used for technologies that measure metabolite levels. |

**Field:** modality  
**Value:** Character string    
**Description:** Describes the modality of the matrix.  
**Notes and considerations for implementation:**  If features or observations are of mixed modalities, then `feature_modality` in the FAM class or `observation_modality` in the FOM class should be used, respectively. This field may often be the same as another field or a combination of other fields such as `analyte` or `species`.

## Subset fields

**Field:** obs_subset  
**Value:** Character string    
**Description:** Describes the subset of observations that are present in the FOM.  
**Notes and considerations for implementation:**  This field can be used to denote different subsets of observations that may be required at different stages of analysis after removing poor quality observations. Several suggested cateogories are provided for common quality control steps. This term can also be used describe a matrix containing a subset of observations based on biological characteristics. For example, after initial clustering and cell type identification, a subset of cells belonging to a particular cell type (e.g. T-cells) may be isolated and re-clustered to better characterize transitionary cell states within the cell type. This subset may have its own normalization, dimensionality reductions, embeddings, etc. 

**Field:** obs_subset_description
**Value:** Character string    
**Description:** A brief description of the `obs_subset` field.   
**Notes and considerations for implementation:**  

**Suggested categories for `obs_subset` and `obs_subset_description`:**

| obs_subset | obs_subset_description | Examples |
| ---------- | ---------------------- | --------------- |
| full | Observations have not been filtered or subsetted. |
| filtered | Observations that have enough signal above background. For example, droplets (or cell barcodes) that have enough counts to be considered to be non-empty. Similar to the “filtered” matrix from CellRanger. |
| threshold | Observations that have a total signal above a certain threshold. For example, only including cells with a total UMI or read count above a certain threshold across features. |
| detected | Observations that have minimum levels of detection across features. For example, only including cells with at least 3 counts in at least 3 genes. |
| nonartifact | A general term to describe filtering that may occur due other quality control metrics. Examples of other artifacts in single cell RNA-seq data include high contamination from ambient material, high mitochondrial percentage, or doublets/multiplets. |
| clean | An “analysis ready” set of observations that have been filtered for total signal, detection across features, outliers, and any other artifacts deemed to be important to filter against for quality control purposes. |


**Field:** feature_subset  
**Value:** Character string    
**Description:** Describes the subset of observations that are present in the FOM.  
**Notes and considerations for implementation:**  
subset - Denotes different subsets of features that may be required at different stages of analysis after removing poor quality or not detected features. This is a general term to describe a subset of features that is usually based on biological characteristics rather than filtering for quality control purposes. Specification considerations: See “subset” in obs_subset.

**Field:** feature_subset_description
**Value:** Character string    
**Description:** A brief description of the `feature_subset` field.   
**Notes and considerations for implementation:**  

**Suggested categories for `feature_subset` and `feature_subset_description`:**

| feature_subset | feature_subset_description | Examples |
| -------------- | -------------------------- | ------- |
| full | Features have not been filtered or subsetted. | |
| threshold | Features that have a total signal above a certain threshold | Only including features with a total UMI or read count above a certain threshold across observations. | 
| detected | Features that have a minimum level of detection across observations. Only including features with at least 3 counts in at least 3 observations. |
| variable | Features that have minimum level of variability across all cells | The top 2,000 most variable features across observations |


**Field:** parent_id  
**Value:** Character string or list    
**Description:** Character string or list to denote the matrix id(s) that were used to produce the matrix.

**Field:** record_id  
**Value:** Character string  
**Description:** ID used to link FOMs to a specific record. This should match an ID in the `LOG` class.  

## Grouping fields

**Field:** dataset_id   
**Value:**  Character string  
**Description:** All FOMs within this group should have observations and features that belong to a superset of observations and features that encompass an entire dataset.  

**Field:** fom_group_id  
**Value:** Character string  
**Description:** A group of FOMs with the same sets of features and observations.   
**Notes and considerations for implementation:** If the FOM is stored in an array-like format, it is recommended that the features and observations be in the same order across FOMs. 

**Field:** obs_group_id  
**Value:** Character string    
**Description:** A group of FOMs with the same set of observations.  
**Notes and considerations for implementation:** If the FOM is stored in an array-like format, it is recommended that the observations be in the same order across FOMs. 

**Considerations for implementation:**  The obs_subset and feature_subset fields can be used to describe the observations and features that are included in the matrix. This has a similar function as the fields obs_group_id and fom_group_id which can be used to group matrices with similar dimensions. While the obs_group_id and fom_group_id fields can be any unique string, using a combination of obs_subset and feature_subset fields may provide a more informative ID. For example, obs_subset could be used as the obs_group_id. If a dataset contains multiple modalities, then a combination of modality and obs_subset could be used (e.g. “RNA.clean”). Similarly the fom_group_id could be a combination of obs_subset and feature_subset fields. For example “full.full” could be used to describe an original matrix without any filtering on either dimension while “clean.variable” could be used to describe the matrix that contains a subset of cells which passed all quality control filters and contains a subset of the top variable features. 


# Observation ID class
**General Description:** An observation_id is character vector or combination of character vectors used to denote the unique ID of each observation. The number of elements in the vector(s) should be the same length as the number of observations in the FOM. If multiple vectors, then the combination of elements across the vectors for each combination should be unique. The number of elements in the vector(s) should be the same length as the number of observations in the FOM. Often compound IDs are created by concatenating multiple types of IDs together when matrices from different sources need to be combined. For example, a cell barcode may uniquely define a cell within a given sample, but the same cell barcode may be used across samples. To combine cell matrices from different samples, a sample ID can be concatenated with the cell barcode to make each observation ID unique across a group of samples. If the OID consists of multiple vectors, then the combination of elements at each position across the vectors should be unique. If the OID is a single character vector that is a compound ID, then a character delimiter should be used to separate the fields within a string and denoted with the optional delim field. For example, if “Sample_A” has a cell with a barcode of “ACGT” and the delimiter is chosen to be “.”, then the compound ID would be “Sample_A.ACGT”.

**Field:** oid  
**Value:** Character string  
**Description:** Denotes the unique id of the matrix containing the observation IDs and should be unique within the scope of the dataset.  

**Field:** oid_header  
**Value:** Character string  
**Description:** Describes headers contained with the observation IDs.
**Notes and considerations for implementation:**  For example if the matrix contains RNA expression data for cells, then this field can be labeled “cell_id”. If the OID is a compound ID with multiple vectors, then this should be a vector of the same length, essentially representing the headers of the OID matrix. If the OID is a single-string compound ID, then this field should denote each component of the ID separated by the delim character. For example, if the sample_id and cell_id are present in the id, then this field should be “sample_id.cell_id”.

**Field:** oid_header_delim  
**Value:** Character string  
**Description:** A character string denoting the delimiter that can be used to separate compound observation IDs in the header.
**Notes and considerations for implementation:** The recommended delimiter is a period (e.g. “.”). If this field is not included, then it will be assumed that the OID is not a compound ID. 

# Feature ID class

**General description:** A feature_id is a character vector or combination of character vectors used to denote the unique ID of each feature. The number of elements in the vector(s) should be the same length as the number of features in the FOM. If multiple vectors, then the combination of elements across the vectors should be unique. 

**Field:** fid  
**Value:** Character string  
**Description:** Denotes the unique id of the matrix containing the feature IDs and should be unique within the scope of the dataset.  


# Observation Annotation Matrix (OAM)
**General description:** OAM objects are matrices or data frames with the same number of observations as its corresponding `FOM`. It is used to store annotations and observation-level metadata such as quality control metrics (e.g. total counts), sample demographics (e.g. age, disease status), and analysis results (e.g. cluster labels, trajectory scores).


**Field:** oam  
**Value:** Character string  
**Description:** Denotes the unique id of the matrix containing observation annotations and should be unique within the scope of the dataset.  

**Field:** observation_modality  
**Value:** Character string or vector  
**Description:** Vector denoting the modality of each observation.
**Notes and considerations for implementation:** This field may often be the same as another field or a combination of other fields such as analyte or species.


# Feature Annotation Matrix class
**General description:** `FAM` obects are matricies or data frames with the same number of features as its corresponding `FOM`. It is used to store annotations and feature-level metadata such as IDs (e.g. Ensembl, Gene Symbol), reference information (e.g. chromosome coordinates, gene biotype), and analysis results (e.g. variability metrics, cluster labels).

**Field:** fam
**Value:** Character string  
**Description:** Denotes the unique id of the matrix containing feature annotations and should be unique within the scope of the dataset.  

**Field:** feature_modality  
**Value:** Character string or vector  
**Description:** Vector denoting the modality of each feature.
**Notes and considerations for implementation:** This field may often be the same as another field or a combination of other fields such as analyte or species.


# Observation Graph (OGR) class
**General description:** TODO. 

**Field:** ogr  
**Value:** Character string  
**Description:** Denotes the unique id of the matrix containing similarities or distances between observations and should be unique within the scope of the dataset.  

**Field:** edge_metric  
**Value:** Character string  
**Description:** Name of the distance or similarity metric used to create the edges.  

**Field:** metric_type  
**Value:** Character string. One of “distance” or “similarity”.  
**Description:** “distance” indicates that smaller values denote more relatedness between observations (e.g. euclidean distance) while “similarity” indicates that larger values denote more relatedness between observations (e.g. Pearson correlation). 


# Provenance (LOG) class

**General description:** These fields are used to capture information about the tool, software package, version, functions, and parameters used to create the various FOMs, annotations, and graphs. 

**Field:** record_id  
**Value:** Character string  
**Description:** ID used to link FOMs, annotations, and graphs to a specific set of `record_package_name`, `record_package_version`, `record_function_name`, and `record_function_parameters` fields. 

**Field:** record_package_name  
**Value:** Character string  
**Description:** Name of the package, tool, or software that ran the algorithm to produce the matrix, annotation, or graph.

**Field:** record_package_version  
**Value:** Character string  
**Description:** Version of the package, tool, or software that ran the algorithm to produce the matrix, annotation, or graph.

**Field:** record_function_name  
**Value:** Character string    
**Description:** Name of the function or mathematical operation used to produce the matrix, annotation, or graph.

**Field:** record_function_parameters  
**Value:** Character string or list   
**Description:** Key/value pairs describing the primary parameters and their values used in the function call.


