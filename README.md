# 

![Project Image](https://github.com/ACDBio/GCDPipe/blob/main/app_default_assets/gcdbanner_small.png)
> An easy-to-use radnom forest-based tool for risk gene, disease-relevant cell type and drug ranking for complex traits using GWAS-derived genetic evidence.
---
### Warning! Drug ranking produced by the pipeline is for scientific purposes only and should not be used as a guidance for clinical practice.
---

### Table of Contents

- [DESCRIPTION](#description)
- [INSTALLATION](#installation)
- [INPUT](#input)
- [SETTINGS](#settings)
- [OUTPUT](#output)
- [USAGE EXAMPLE](#example)
- [CITATION](#cite)

---

## Description

 - The pipeline is designed to use the data on known risk genes (which can be obtained from GWAS fine-mapping) and expression profiles characterizing cell types/tissues to construct a random forest classifier to distinguish risk genes.   
- A modification of feature importance analysis with SHAP values is used to rank the cell types/tissues based on their importance for risk class assignment.   
- The information on drug gene-targets can then be used to rank the drugs by maximal risk class assignment probability of any of their targets.   

### Pipeline performance checking
- The pipleline was tested on IBD, schizophrenia, Alzheimer's disease. 
- For schizophrenia, it displays ROC-AUC above 0.9 on test gene set in a risk gene discrimination task.
- The risk genes identified with GCDPipe for schizophrenia link dopaminergic synapse, retrograde endocannabinoid signaling, nicotine addiction ad other dysregulations at molecular level; for Alzheimer’s disease it shows diuretics as a leading enriched drug target category, and for IBD it prioritizes TH1/17 and other CD4+ T cells.  
- In the case studies on IBD and schizophrenia, the obtained gene ranking is significantly enriched with drug targets for the corresponding diseases and drug ranking - in the corresponding disease drugs.
  
For further details on the pipeline, see the publication ...  
### General pipeline scheme
![Pipeline Scheme](https://github.com/ACDBio/GCDPipe/blob/main/app_default_assets/gcdpipe_scheme.png)  

[Back To The Top](# )

---
## Installation
  
```shell
# A virtual environment can be created and activated with:
python3 -m venv GCDpipe_env
source GCDpipe_env/bin/activate
# Downloading the code:
git clone https://github.com/ACDBio/GCDPipe.git
# It might be required to upgrade pip with: 
python -m pip install --upgrade pip
# Downloading the required packages: 
pip install -r ./GCDPipe/requirements.txt
# Launching the GCDpipe Dash App:
python ./GCDPipe/GCDPipe.py
# To use GCDPipe interface, open up the link depicted after the phrase 'Dash is running on' in the console. 
```  
---
## Input
For gene classification, only first two fields need to be filled. The files to the other fields are uploaded in cases when drug prioritization and its initial quality assessment are required   

 #### Field 1: Gene Data (a data on risk and non-risk genes used for classifier training and testing)  
 Two types of .csv files can be uploaded in this field:  
 - A file with gene identifiers in the first column and their attribution to risk (1) or non-risk (0) class.  
   
| pipe_genesymbol | is_True  |
| :-----: | :-: |
| ACP5 | 0 |
| GRIN2A | 1 |  
 - A file, specifying locations of the significant loci in GRCh38 coordinates and the corresponding risk genes for each locus. In this case, all genes lying within 500 kbase window around the locus center (which is expected to be a genome-wide significant variant (a 'leading variant') from GWAS), except for the risk ones are considered as 'false', and a gene set for classifier training and testing is constructed in an automated manner. If not variant ID is unknown, an rsid field can be left blank.  
  
| pipe_genesymbol | rsid  |  location  |  chromosome  |
| :-----: | :-: | :-: | :-: |
| NOD2 | rs2066844 | 50712015 | 16 |  
  
#### Field 2: Feature data (expression profiles)
- Here, a .csv file with expression profiles characterizing cell types/tissues of interest can be uploaded. The data can be obtained from a range of publicly available expression atlases and other sources (such as Allen Brain Mep, DropViz, DICE  Immune Cell Atlas, GTEx etc.). These profiles are used as features to build a risk gene classifier. It requires pipe_genesymbol column with gene identifiers and other columns with custom names (for example, names of cell types).
  
| pipe_genesymbol | Exc.L5.6.FEZF2.ANKRD20A1  |  Exc.L5.6.THEMIS.TMEM233  |  Inh.L1.LAMP5.NDNF  |
| :-----: | :-: | :-: | :-: |
| A2M | 4.15 | 3.83 | 0 |  



  
---
