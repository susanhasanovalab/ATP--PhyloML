
# ATPβ-PhyloML
### Machine Learning-Based Evolutionary Analysis and Phylogenetic Assessment of Mitochondrial ATP Synthase Beta Subunits in *Arabidopsis thaliana*

## Overview

ATPβ-PhyloML is a computational bioinformatics project designed to investigate the sequence diversity, evolutionary relationships, and compositional patterns of mitochondrial ATP synthase beta subunit proteins.

The project combines protein sequence retrieval, quality control, multiple sequence alignment, sequence conservation analysis, machine learning-based clustering, and phylogenetic analysis to explore the relationships between *Arabidopsis thaliana* mitochondrial ATP synthase beta proteins and homologous proteins from other plant species.

The workflow integrates conventional bioinformatics methods with machine learning techniques to provide a comparative analysis of protein sequence characteristics and evolutionary relationships.

## Research Objectives

- Retrieve mitochondrial ATP synthase beta subunit protein sequences from UniProt.
- Investigate three mitochondrial ATP synthase beta paralogs of *Arabidopsis thaliana*.
- Perform protein sequence quality control and filtering.
- Generate a multiple sequence alignment.
- Identify conserved and variable regions using Shannon entropy.
- Calculate pairwise protein sequence identity.
- Extract amino acid and dipeptide composition features.
- Apply Principal Component Analysis (PCA) and K-Means clustering.
- Construct a phylogenetic tree.
- Compare machine learning clusters with phylogeny-derived clusters.

## Dataset

Protein sequences are retrieved directly from the UniProt REST API.

### Reference proteins

The analysis focuses on three mitochondrial ATP synthase beta subunit proteins from *Arabidopsis thaliana*:

| Accession | Protein | Gene |
|---|---|---|
| P83483 | ATP synthase subunit beta-1, mitochondrial | At5g08670 |
| P83484 | ATP synthase subunit beta-2, mitochondrial | At5g08690 |
| Q9C5A9 | ATP synthase subunit beta-3, mitochondrial | At5g08680 |

Additional homologous protein sequences from other plant species are included for comparative analysis.

### Data source

- [UniProt](https://www.uniprot.org/)
- [UniProt REST API](https://rest.uniprot.org/)

The dataset is retrieved programmatically, rather than relying on manually uploaded sequence files.

## Workflow

The computational workflow consists of the following stages:

### 1. Protein Sequence Retrieval

Protein sequence records are retrieved from UniProt using accession-based API requests.

Protein metadata includes:
- UniProt accession
- Protein name
- Organism
- Taxonomic ID
- Gene information
- Protein sequence
- Sequence length
- Subcellular localization annotation

### 2. Sequence Quality Control

Protein sequences undergo quality control before downstream analysis.

Filtering criteria include:
- Sequence length between 400 and 700 amino acids.
- Removal of sequences containing non-canonical amino acid symbols.
- Removal of exact duplicate sequences.
- Verification that all three *Arabidopsis thaliana* reference proteins are retained.

### 3. Multiple Sequence Alignment

Multiple sequence alignment is performed using **PyFAMSA**.

The resulting alignment is used for conservation analysis and phylogenetic reconstruction.

### 4. Shannon Entropy Analysis

Shannon entropy is calculated for each alignment position to evaluate amino acid variability.

The analysis excludes alignment columns containing more than 50% gaps.

This allows conserved and variable protein regions to be investigated.

### 5. Pairwise Sequence Identity

Pairwise protein sequence identity is calculated using Biopython's `PairwiseAligner`.

Parameters:
- Alignment mode: Global
- Substitution matrix: BLOSUM62
- Gap opening penalty: -10
- Gap extension penalty: -0.5

Sequence identity is evaluated between the analyzed proteins and the three *Arabidopsis thaliana* reference sequences.

### 6. Protein Feature Extraction

A total of 420 sequence-derived features are extracted from each protein:

- 20 amino acid composition (AAC) features.
- 400 dipeptide composition (DPC) features.

These features represent the amino acid and adjacent amino acid pair distributions within each protein sequence.

### 7. Machine Learning Analysis

The extracted feature matrix is standardized using `StandardScaler`.

Principal Component Analysis (PCA) is applied to reduce the feature space to two dimensions.

K-Means clustering is performed with candidate cluster numbers ranging from 2 to 6.

The optimal cluster number is selected based on the silhouette score.

### 8. Phylogenetic Reconstruction

Phylogenetic reconstruction is performed using the following approach:

- IQ-TREE Maximum Likelihood analysis, when IQ-TREE is available and runs successfully.
- Neighbor-Joining (NJ) with p-distance as a fallback method.

For the reported execution, the generated tree was constructed using Neighbor-Joining with p-distance, excluding gap-gap and gap-containing positions from pairwise comparisons.

The resulting tree is exported in Newick format.

### 9. Phylogenetic and Machine Learning Validation

Phylogenetic distances are used to generate hierarchical clusters.

The resulting groups are compared with the machine learning clusters using:

- Adjusted Rand Index (ARI).
- Permutation testing with 10,000 permutations.

This provides an exploratory assessment of agreement between the two clustering approaches.

## Key Results

The analyzed dataset contains:

- 9 protein sequences.
- 7 represented organisms.
- 3 *Arabidopsis thaliana* mitochondrial ATP synthase beta paralogs.
- 420 extracted protein composition features.

### PCA

The first two principal components explain:

| Component | Explained variance |
|---|---:|
| PC1 | 32.40% |
| PC2 | 23.65% |
| Combined | 56.05% |

### Machine Learning

- Clustering algorithm: K-Means.
- Selected number of clusters: 2.
- Silhouette score: 0.3155.
- Cluster distribution: 8 and 1 sequences.

### Phylogenetic Comparison

| Metric | Result |
|---|---:|
| Adjusted Rand Index | 1.0000 |
| Permutation p-value | 0.1086 |
| Number of permutations | 10,000 |

**Interpretation:** Although the clustering partitions showed an ARI of 1.0, the permutation test did not demonstrate statistically significant agreement at the 0.05 significance level (p = 0.1086). The result should therefore be interpreted cautiously, particularly given the small dataset and imbalanced cluster distribution.

## Technologies and Libraries

The project uses the following tools and libraries:

- Python
- Biopython
- PyFAMSA
- NumPy
- Pandas
- SciPy
- Scikit-learn
- Matplotlib
- Seaborn
- UniProt REST API

## Installation

Clone the repository:

```bash
git clone https://github.com/YOUR-USERNAME/ATPbeta-PhyloML.git
cd ATPbeta-PhyloML
```

Install the required Python packages:

```bash
pip install requests biopython pandas numpy matplotlib seaborn scikit-learn scipy pyfamsa
```

For the optional Maximum Likelihood phylogenetic analysis, IQ-TREE 2 must also be installed and accessible from the system PATH.

## Running the Project

1. Open the Jupyter Notebook.
2. Install the required dependencies.
3. Execute the notebook cells in the intended order.
4. Retrieve the protein sequences from UniProt.
5. Perform sequence quality control and alignment.
6. Run conservation, feature extraction, machine learning, and phylogenetic analyses.
7. Generate the figures and result files.

The notebook creates project directories for downloaded data and generated results.

## Expected Output Files

The workflow generates data and result files, including:

```text
ARABIDOPSIS_ATP_SYNTHASE/
│
├── data/
│   ├── protein_metadata.csv
│   ├── mitochondrial_atp_beta.fasta
│   ├── mitochondrial_atp_beta_aligned.fasta
│   ├── shannon_entropy.csv
│   └── pairwise_sequence_identity.csv
│
└── results/
    ├── protein_feature_matrix.csv
    ├── ML_results.csv
    ├── ML_phylogenetic_validation.csv
    ├── mitochondrial_ATP_tree.nwk
    ├── ML_PCA_clusters.png
    ├── shannon_entropy.png
    ├── identity_heatmap.png
    └── final_comparison.png
```

## Scientific Limitations

This project represents an exploratory comparative protein bioinformatics analysis.

Several limitations should be considered:

- The dataset contains a relatively small number of protein sequences.
- The analysis focuses on a limited number of plant species.
- Machine learning clusters are based on amino acid and dipeptide composition features.
- The phylogenetic reconstruction depends on the selected evolutionary distance method.
- The reported Neighbor-Joining tree does not have computed bootstrap support values.
- An ARI of 1.0 does not independently establish statistically significant evolutionary agreement.

Further research should include broader taxonomic sampling, additional sequence validation, model-based phylogenetic reconstruction, and statistical support assessment.

## Future Directions

Potential improvements include:

- Expanding the dataset with additional plant species.
- Investigating a broader range of ATP synthase subunits.
- Comparing alternative protein feature representations.
- Evaluating additional clustering algorithms.
- Performing Maximum Likelihood phylogenetic reconstruction.
- Incorporating bootstrap and branch-support analyses.
- Investigating evolutionary conservation across a larger taxonomic range.

## Author

**ATPβ-PhyloML**

A computational bioinformatics project combining protein sequence analysis, machine learning, and phylogenetic methods to investigate mitochondrial ATP synthase beta subunits.

## Acknowledgements

- UniProt for protein sequence and annotation resources.
- Biopython for computational sequence analysis.
- PyFAMSA for multiple sequence alignment.
- Scikit-learn for machine learning methods.
- The open-source scientific Python community.

---

*This repository is intended for research, educational, and computational bioinformatics purposes.*
