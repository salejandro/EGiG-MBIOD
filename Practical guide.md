# Comparative Genomics - Master in Genetics and Genomics

## Introduction

The objective of these sessions is to do some practical work to increase your understanding of the main concepts behind evolutionary inference based on the comparison of genomic sequences. You will work with some (just some examples) of the state-of-the-art computational tools to estimate phylogenetic relationships among genes or genomes, and to determine the functional consequences of mutations in these genomes. This fundamental knowledge has great importance and applicability in genetic and biomedical research, and in biodiversity studies.

The biological models used in this practices are Sarbecoviruses, and more specifically, those closelly related with SARS-Cov2, the virus that...., and the Omicron variant BA.1 (apeared....)

</br>

<p align="center">
<img src="http://www.ub.edu/molevol/CG-MGG/sars2.png" width="1000" heigh="1000">
</p>

> **Fig. 1** Schematic diagrams of the SARS-CoV-2 virus particle (a) and genome (b). The genome encodes sixteen nonstructural proteins (nsp1–11, 12–16) and six accessory proteins. Plpro: papain like protease, 3CLPro: 3C-like proteinase, *__RdRp: RNA-dependent RNA polymerase__*, Hel:
Helicase, *__S: spike__*. Modified from Signal Transduction and Targeted Therapy (2021)6:233.

---
  
## Data

The genome sequences for this practice were retrieved either from [GenBank](https://www.ncbi.nlm.nih.gov/genbank/) or [GISAID](https://gisaid.org/) database and correspond to:

1. The genomic sequences of the sarbecoviruses used for figure 1 in [Temmam et al. (2022)](https://www.nature.com/articles/s41586-022-04532-4), excluding 'Wuhan/IME-WH01/2019' (which is identical to the reference sequence 'Wuhan-Hu-1 China 2019'), and '_R. malayanus_ RmYN02 China 2019' (which is not complete)

2. A subset of 4717 high-quality genomes of the BA.1
variant (Omicron) collected world wide between 05/01/2020 and 22/06/2022. You can find the metadata associated with these genomes in the file BA.1.metadata, and the instructions for retrieve these sequences in the file [omicron_seqs.md](https://github.com/salejandro/Comparative-Genomics-MGG/blob/main/omicron_seqs.md).

### Data files:

[sarbecoviruses](http://www.ub.edu/molevol/CG-MGG/sarbecoviruses.fasta)   
[omicron.BA1](http://www.ub.edu/molevol/CG-MGG/omicron-BA1.fasta)
___

## Bioinformatics software and tools

Before starting the practical, you must create and activate a new conda environment, and install all the necesary tools. To do that, open the terminal app in your computers
and type:

```bash
conda create --name CGenv
```
  > You can name the environment whatever you want, as long as you activate the correct environment to work on the rest of the practice
 
 To activate the environment:
 
```bash
conda activate CGenv
```
  > The commands below install the programs in currently activated environment. Make sure that your environment is activated (the name of the environment appears at starting the command line...

Now, you are ready to install the programs and utilities for the practial. For the sessions on filogenomics, you will need....

```bash
conda install .....
```
For the second part of the practical, you will need the software `hyphy` and some extra utilities, including `Python 3.9`, the module `python-bioext`, and the program `tn93` (version >=1.0.7):

```bash
conda install python=3.9 python-bioext 'tn93>=1.0.7'
```
In addition to this programs, you will also use the python scripts developed by ...and that you can find in the folder ....


