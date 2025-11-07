# CNAttention: an attention-based deep multiple-instance method for uncovering CNA signatures across cancers

This tool aims to uncovering copy number abbresion (CNA) patterns of pan-cancer based on large CNA data from progenetix database.

![pipeline_diagram](https://github.com/ziyingyang96/cnv-signature/blob/main/workflow.png)

## Repository layout
```
CNAttention-TF/
├─ configs/
│ └─ config.yaml
├─ data/ # (put your CSVs here; not versioned)
├─ scr/
│ ├─ make_bags.py # optional: precompute bag indices (npz)
│ ├─ train.py # train MIL attention model
│ ├─ signatures.py # derive per-cancer DEL/DUP gene signatures
│ ├─ data_io.py # load/align data, label encoding
│ ├─ bags.py # create train/val/external bags
│ ├─ mil_layers.py # Keras MIL attention layer
│ ├─ model.py # build Keras model (shared MLP + attention)
│ ├─ train_utils.py # class weights, callbacks, seed, metrics
│ ├─ attribution.py # attention × probs → per-instance / per-gene
├─ environment.yml
├─ requirements.txt
├─ Dockerfile
├─ README.md
└─ LICENSE
```


---
## Quick start (Conda)
```bash
conda env create -f environment.yml && conda activate cnattention-tf
python scripts/train.py --config configs/config.yaml
python scripts/eval.py --config configs/config.yaml
python scripts/signatures.py --config configs/config.yaml --topn 100
```


## Docker
```bash
docker build -t cnattention-tf .
docker run -v $PWD:/app cnattention-tf
```


## Notes

- Bag construction matches the original logic (majority-class soft label for train; majority hard label for val).
- Attention weights are retrieved via the "alpha" layer and used for linear attribution to instances and genes.
- For external cohorts, provide CSVs with the same gene columns and a `cancer_label` column.



# Visualization

The users can visualize the selected CNA patterns on [progenetix](https://progenetix.org/) database by simply type in the feature gene names of a specific cancer type.

![sample plot](https://github.com/baudisgroup/CNAttention/blob/main/sampleplots.png)

