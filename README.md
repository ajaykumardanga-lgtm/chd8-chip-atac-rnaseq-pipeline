**CHD8 ChIP-seq / ATAC-seq / RNA-seq Analysis Pipeline**

Analysis code supporting the manuscript "**CHD8 orchestrates chromatin landscapes during early female neuronal differentiation**" investigating CHD8 binding, chromatin accessibility, and transcriptional changes in a CHD8 knockout/rescue model.

**Code availability note:** This code accompanies a preprint on bioRxiv and a submission to _Molecular Autism_. It was developed iteratively in Google Colab; see "Notes on structure" below before running.

**Contents**

notebooks/

CHD8_.ipynb - Jupyter notebook version of the ChIP/RNA-seq analysis

differential_accessibility_analysis_.ipynb - Jupyter notebook version of the ATAC-seq analysis

scripts/

chd8_.py - ChIP-seq, RNA-seq DEG, GO enrichment, motif, and figure-generation code

differential_accessibility_analysis_.py - Differential ATAC-seq accessibility pipeline (DESeq2)

**What each script does**

**scripts/chd8\_.py**

End-to-end analysis covering:

- RNA-seq DEG filtering across multiple perturbation models — full knockout (KO) and two independent shRNA knockdown clones (KD1, KD2) and one siRNA knockdown — from DESeq2 output (baseMean > 10, padj &lt; 0.05, |log2FC| &gt; 0.5)
- GO Biological Process enrichment (Enrichr via gseapy, GO_Biological_Process_2023)
- CHD8 and H3K4me3 ChIP-seq peak processing (pybedtools/bedtools) and TSS-distance / promoter vs. distal classification
- Statistical comparisons (Mann-Whitney U, Kruskal-Wallis, Fisher's exact test, Benjamini-Hochberg FDR correction) between target and non-target gene sets
- Overlap analysis between ChIP targets, DEGs, and ATAC-seq changes
- Rescue-construct analysis comparing knockout to three rescue conditions: full-length (FL), ΔChromo (dC), and ΔHelicase (dH) CHD8
- SFARI gene-list cross-referencing
- Generation of main and supplementary figures/tables (associated Fig. 1–7, supplementary Fig. 1-5 and supplementary tables: S1-S17)

**scripts/differential_accessibility_analysis\_.py**

Focused ATAC-seq differential accessibility workflow:

- Consensus peak set construction across replicates (DiffBind, bedtools multicov)
- Read counting and DESeq2-based differential accessibility testing
- PCA and hierarchical clustering QC (pheatmap, RColorBrewer), outlier exclusion
- A parameter-sensitivity sweep across consensus/window parameters

**Requirements**

- **Python 3.x** — pandas, numpy, matplotlib, seaborn, pybedtools, gseapy, matplotlib_venn, scipy, statsmodels
- **R** — DESeq2, DiffBind, ggplot2, pheatmap, RColorBrewer (called via Rscript subprocess calls from Python)
- **Command-line tools** — bedtools (installed in-script via apt-get in the original Colab environment)
- Developed and run in Google Colab; not tested outside that environment

A requirements.txt / environment.yml is not yet included — see Notes below.

**Notes on structure (please read before running)**

This code was developed iteratively in Google Colab across many analysis sessions and exported directly to .py/.ipynb. As a result:

- **Paths are hardcoded** to Colab's Google Drive mount point (/content/drive/MyDrive/...), including variables such as BASE_DIR, DESEQ2_DIR, PEAK_PATH, and TSS_PATH. These reflect the original working directory structure only and must be edited to point to your own data before running. They do not correspond to any private or identifying location.
- **The scripts are long and include exploratory/iterative code** (e.g., multiple versions of the same analysis step, debugging output, intermediate sanity checks) alongside the code that produced final manuscript figures. They are provided for transparency and reproducibility of the reported results, not as a polished package.
- Google Drive mounting (drive.mount(...)) and Colab-specific shell/magic commands (!pip install, !apt-get install) are called directly in the scripts and will not run outside Google Colab without modification.
- chd8_.py copies reference files (e.g., the GTF annotation) from Drive to a local scratch directory (/content/local_data) to speed up I/O; this step is Colab-specific and can be skipped when running locally.

**Data availability**

Raw sequencing data are available at **GSE346086**.

**Citation**

If you use this code, please cite: Danga AK, Colantoni A, Pomella N, Ruiz Blanes N, Genovesi M, Ersoz F, Tartaglia GG, Fakhry MM, Cerase A. CHD8 orchestrates chromatin landscapes during early female neuronal differentiation. bioRxiv. 2026. doi: <https://doi.org/10.64898/2026.09.09.750428 

**Contact**

Questions about the code can be directed to Andrea Cerase: [andrea.cerase@unipi.it](mailto:andrea.cerase@unipi.it); Ajay Kumar Danga: [ajaykumardanga@ymail.com](mailto:ajaykumardanga@ymail.com)
