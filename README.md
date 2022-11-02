# DLEPS
A Deep Learning based Efficacy Prediction System for Drug Discovery

# Setup

- ## Install package

This package requires the **rdkit**, **tensorflow >=1.15.0** and **Keras >=2.3.0**.

```
conda install -c rdkit rdkit
apt-get update
apt install libxrender1
apt install libxext6
pip install nltk
pip install tensorflow==1.15.0
pip install keras==2.3.0
```

- ## On Code ocean

The supporting files and sample input files for the model locates in the data folder. Results were saved in results folder.

# Run the model

- **Script options**

input files
1. The csv file with all the chemical SMILES in the column with string SMILES as the header, other columns will be copied to the output file and an efficacy score column will be appended.
2. The upregulated gene signatures using ENTREZGENE_ACC in a file without header, each gene occupy a row
3. The downregulated gene signatures using the same format

Conversion of gene names can be accomplished at https://biit.cs.ut.ee/gprofiler/convert

A sample command is as followed:
```python driv_DLEPS.py --input=../../data/Brief_Targetmol_natural_product_2719 --output=../../results/np2719_Browning.csv --upset=../../data/BROWNING_up --downset=../../data/BROWNING_down --reverse=False```

Batch jobs were put into run_script

Other options include:

    '--input', default=INPUTFILE,
                        'Brief format of chemicals: contains SMILES column. '
    '--use_onehot',  default=True,
                        'If use pre-stored one hot array to save time.'
    '--use_l12k',  default=None,
                        'Use pre-calculated L12k'
    '--upset',  default=None,
                        'Up set of genes'
    '--downset',  default=None,
                        'Down set of genes. '
    '--reverse',  default=True,
                        'If the drug Reverse the Up / Down set of genes. '
    '--output',  default='out.csv',
                        'Output file name. '

Jupyter notebook users may run DLEPS_tutorial.ipynb for better iterative computing and analysis.


# <span style="color:red">personal patch</span>

## keras version

currently, i'm using python/3.9.5; tensorflow/2.8.0; keras/2.8.0, via `module load python_ML_packages/3.9.5-gpu`. 
therefore, in my case, "objectives" has changed name to "losses" in later Keras versions. lines in
model_zinc.py (line 3 and 122) and model_zinc_str.py (line 3 and 96) are updated from *objectives* to *losses*

## missing data in `../../data/`

- the DLEPS authors refered `vae.hdf5` as https://github.com/mkusner/grammarVAE/blob/master/pretrained/zinc_vae_grammar_L56_E100_val.hdf5, see [here](https://github.com/kekegg/DLEPS/issues/1#issuecomment-869421988)

- `benchmark.csv` and `gene_info.txt`are recovered from https://github.com/kekegg/DLEPS/commit/b82b3be123958a24199daacfffe9921f0bcb1e6e

- `denseweight.h5` following this comment https://github.com/kekegg/DLEPS/issues/6#issuecomment-915908677 

    ```
    BiocManager::install("signatureSearch")
    library(signatureSearch)
    gctx2h5(gctx="GSE92743_Broad_OLS_WEIGHTS_n979x11350.gctx", cid=1:979,  h5file="denseweight.h5")

    ```
    
    but this approach has an error: `TypeError: unsupported operand type(s) for *: 'float' and 'NoneType'`, from line 55 in `dleps_predictor.py` ; self.full_con = A3.dot(self.W)



## file path

to be able run the DLEPS via `python ~/DLEPS/code/DLEPS/driv_DLEPS.py`,  `/../../data/vae.hdf5` is changed to `os.path.dirname(os.path.abspath(__file__))+'/../../data/vae.hdf5'`  



