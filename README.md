# LINGER
## Introduction
LINGER (LIfelong neural Network for GEne Regulation)is a novel deep learning-based method to infer GRNs from single-cell multiome data with paired gene expression and chromatin accessibility data from the same cell. LINGER incorporates both 1) atlas-scale external bulk data across diverse cellular contexts and 2) the knowledge of transcription factor (TF) motif matching to cis-regulatory elements as a manifold regularization to address the challenge of limited data and extensive parameter space in GRN inference.
## Requirements
We use the following Python packages with python 3.10.9: 
torch: 1.13.1+cu117; numpy: 1.23.5; scipy: 1.10.1; pandas: 1.5.3; sklearn: 1.2.1; shap: 0.41.0; joblib: 1.2.0; 
### Run
#### Step 1, install the package
For the first step, we can download the code by
```sh
git clone https://github.com/Durenlab/LINGER.git
cd LINGER
```
Then download the datasets:
```sh
wget [https://drive.google.com/file/d/1miQkV1mUjBa7wFoPKcKXQwHR9ReCCBfO/view?usp=sharing](https://drive.google.com/file/d/1qmMudeixeRbYS8LCDJEuWxlAgeM0hC1r/view?usp=sharing) #ATAC-seq
wget [https://drive.google.com/file/d/1Rj7RbzY-8Tc8sRWJ_dfbsUlWTx9PFX1A/view?usp=sharing](https://drive.google.com/file/d/1dP4ITjQZiVDa52xfDTo5c14f9H0MsEGK/view?usp=sharing) #RNA-seq
wget [https://drive.google.com/file/d/1ZeEp5GnWfQJxuAY0uK9o8s_uAvFsNPI5/view?usp=sharing](https://drive.google.com/file/d/1ZeEp5GnWfQJxuAY0uK9o8s_uAvFsNPI5/view?usp=sharing) #label
wget [https://drive.google.com/file/d/1miQkV1mUjBa7wFoPKcKXQwHR9ReCCBfO/view?usp=sharing](https://drive.google.com/file/d/1miQkV1mUjBa7wFoPKcKXQwHR9ReCCBfO/view?usp=sharing) #TF-RE
wget [https://drive.google.com/file/d/1OGsdqGupPEN7x7JD4xEbEAxo3bI4giS0/view?usp=sharing](https://drive.google.com/file/d/1OGsdqGupPEN7x7JD4xEbEAxo3bI4giS0/view?usp=sharing) #TF-TG
wget [https://drive.google.com/file/d/1JK5K3k7bW4Dp2BIDZUB4Inw7HvY41Blw/view?usp=sharing](https://drive.google.com/file/d/1JK5K3k7bW4Dp2BIDZUB4Inw7HvY41Blw/view?usp=sharing) #RE-TG
```
#### Step2, gene regulatory network inference
##### Input. We need to input the count matrix of single-cell RNA-seq and ATAC-seq, as well as the cluster annotations.
1. sc RNA-seq. The row names are gene symbol; the column names are cell barcode; the values are the reads count, representing the gene expression. Here, we use 'RNA.txt' as the name of this file.
2. sc ATAC-seq. The row names are regulatory element, for example chr1:191491-191736; the column names are cell barcode; the values are the reads count, representing the chromatin accessibility of regulatory element. Here, we use 'ATAC.txt' to represent this file.
3. Cell annotation.  There is one column in the file, representing the cluster or cell type. Here, we use 'label.txt' to represent this file.
```python
import LL_net
RNA_file='RNA.txt'
labels='label.txt'
ATAC_file='ATAC.txt'
```
##### cell population level gene regulatory network
1. TF-RE binding. The output is 'cell_population_TF_RE_binding.txt', a matrix of the TF-RE binding strength.
```python
LL_n.TF_RE_binding(RNA_file, ATAC_file, labels)
```
2. *cis*-regulatory. The output is 'cell_population_cis_regulatory.txt', a list of 3 columns in which the first column is the regulatory element, the second column is the target gene, and the third is the regulatory strength.
```python
LL_n.cis_regulatory(RNA_file, ATAC_file, labels)
```
3. *trans*-regulatory.  The output is 'cell_population_trans_regulatory.txt', a matrix of the TF-TG *trans*-regulatory strength.
```python
LL_n.trans_regulatory(RNA_file, ATAC_file, labels)
```
##### cell type specific gene regulatory network
1. TF-RE binding potential. The output is 'cell_type_specific_TF_RE_binding_{cluster}.txt'. The cell type specific TF-RE binding potential for each {cluster}/{cell type}.
```python
LL_n.cell_type_specific_TF_RE_binding(RNA_file, ATAC_file, labels)
```
2. Cell type specific *cis*-regulatory potential. The output is 'cell_type_specific_cis_regulatory_{cluster}.txt'.  Each file is a list of 3 columns in which the first column is the regulatory element, the second column is the target gene, and the third is the regulatory strength.
```python
LL_n.cell_type_specific_cis_regulatory(RNA_file, ATAC_file, labels)
```
3. Cell type specific *trans*-regulatory potential. The output is 'cell_type_specific_trans_regulatory_{cluster}.txt'
```python
LL_n.cell_type_specific_trans_regulatory(RNA_file, ATAC_file, labels)
```
