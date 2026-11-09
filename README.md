[![DOI](https://img.shields.io/badge/DOI-10.1093/bib/bbaf713-blue)](https://doi.org/10.1093/bib/bbaf713)
# CoBRA
CoBRA is a GPU-only pipeline that leverages RiNALMo to generate RNA embeddings and compute residue-level binding-site predictions.

## Requirements
- NVIDIA GPU with CUDA support (Ampere architecture, SM 8.0 or higher required).
- Conda for environment management.
- Git submodule access to clone RiNALMo.

## Clone the repo
```bash
git clone https://github.com/kucm-lsbi/CoBRA
cd ./CoBRA
git clone https://github.com/lbcb-sci/RiNALMo
```

## Create conda enviroment for CoBRA
```bash
conda env create -f ./environment.yml
conda activate CoBRA
```

## Usage
```bash
python main.py sample/sample.fasta sample/embeddings sample/sample.csv
```

- `sample/sample.fasta`: Input FASTA file path (only .fa/.fasta allowed).
- `sample/embeddings`: Directory where embedding (.pt) files will be stored.
- `sample/sample.csv`: CSV file to save results (.csv only).

### Input preparation
- Ensure sequences are valid RNA sequences; unexpected characters will be rejected.
- Place multiple sequences in the FASTA to process them in one run.

### Outputs
- Embedding tensors are saved per sequence to the specified embeddings directory.
- The CSV includes sequence identifiers and the predicted metrics described in `main.py`.

### Tips
- Run in an environment with sufficient GPU memory; longer sequences require more memory.
- Confirm that the RiNALMo repository remains alongside CoBRA when running the script.
- Use `python main.py -h` to see available CLI options and defaults.


## Dataset

All datasets are available under `./CoBRA/Dataset/`.

### Raw Dataset

The raw dataset contains 432 unique RNA chains from HARIBOSS and TR60, excluding non-biological ligands (HOH, SO4, PO4, GOL, EDO, and PEG) and RNA chains longer than 161 nucleotides.

### Structure-Based Split Dataset

RNABindSet was used to evaluate model generalizability with sequence- and structure-level non-redundancy between training and test sets.

### External Benchmarks

RB9, JL10, TL12, and TE18 were used for independent evaluation.


### bindingsite.txt Format

Each `bindingsite.txt` file provides residue-level binding-site annotation and follows a three-line repeating structure:
```bash
>SEQUENCE_ID
SEQUENCE
LABELS
```

- **SEQUENCE_ID**: Identifier of the RNA sample.  
- **SEQUENCE**: RNA nucleotide sequence (A, C, G, U).  
- **LABELS**: Binary string indicating binding-site positions  
  - `1` = binding site  
  - `0` = non-binding site  

Each group of three lines represents a single fully annotated RNA sequence.
