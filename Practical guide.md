# Comparative Genomics - Master in Genetics and Genomics

## Introduction

The objective of these sessions is to increase your understanding of the main concepts behind evolutionary inference by doing some practical worl with a representation (just a few examples) of the state-of-the-art computational tools to estimate phylogenetic relationships between genes and genomes, and to determine the functional consequences of mutations in these sequences. This fundamental knowledge has great importance and applicability in genetic and biomedical research, and in biodiversity studies.

The biological model used in this practices are Sarbecoviruses,and more specifically, a group of viruses closelly related with SARS-Cov2 (the the type 2 coronavirus that causes severe acute respiratory syndrome) (part 1), and the BA.1 subvariant of the SARS-CoV2 variant of concern (VOC) Omicron (part 2).

</br>
</br>

<p align="center">
<img src="http://www.ub.edu/molevol/CG-MGG/sars2.png" width="1000" heigh="1000">
</p>

> **Fig. 1** Schematic diagrams of the SARS-CoV-2 virus particle (a) and genome (b). The genome encodes sixteen nonstructural proteins (nsp1–11, 12–16) and six accessory proteins. Plpro: papain like protease, 3CLPro: 3C-like proteinase, *__RdRp: RNA-dependent RNA polymerase__*, Hel:
Helicase, *__S: spike__*. Modified from Signal Transduction and Targeted Therapy (2021)6:233.

</br>
</br>

In the fisrt part of this practice you will reconstruct the phylogenetic relationships of a group of sarbecoviruses using different parts of the genome with (partially) different evolutionary histories. This analysis demonstrates that SARS-CoV2 genome is a mosaic and originated after recombination between different strains of sarbecoviruses. In the second part, you will estimate the effect on virus fitness of the minssense mutations (amino acid replacements) acculated in the subvariant BA.1 of Omicron, and compare this effect with the estimated impact for the same positions prior to the emergence of this VOC (all SARS-CoV-2 near full-length genome sequences present in GISAID on 21/11/2021 that passed some quality control checks; [Martin et al. (2022)](https://academic.oup.com/mbe/article/39/4/msac061/6553617)). This analysis in an example of how the accumulation of mutations with  individual fitness costs, but that are adaptive in combination with other mutations, could alter the function of a protein (Spike), while remained completely undetected despite unprecedented global genomic surveillance efforts.

<p align="center">
<img src="http://www.ub.edu/molevol/EGiG/UB-biol-150.jpg" width="1000" heigh="1000">
</p>

> **Fig. 2** Frecuency of the different SARS-CoV2 variants (colored by clade) at different time points as given in the Latest global SARS-CoV-2 analysis with GISAID data ([Nextstrain SARS-CoV-2 resources](https://nextstrain.org/ncov/gisaid/global/all-time), last accessed 16/11/2022). The subvariant BA.1 of Omicron correspond to the clade 21K.

---
  
## Data

The genome sequences for this practice were retrieved either from [GenBank](https://www.ncbi.nlm.nih.gov/genbank/) or [GISAID](https://gisaid.org/) database and correspond to:

1. The genomic sequences of the sarbecoviruses used for figure 1 in [Temmam et al. (2022)](https://www.nature.com/articles/s41586-022-04532-4), excluding 'Wuhan/IME-WH01/2019' (which is identical to the reference sequence 'Wuhan-Hu-1 China 2019'), and '_R. malayanus_ RmYN02 China 2019' (which is not complete)

2. A subset of 4717 high-quality genomes of the BA.1
variant (Omicron) collected world wide between 05/01/2020 and 22/06/2022. You can find the metadata associated with these genomes in the file BA.1.metadata, and the instructions for retrieve these sequences in the file [omicron_seqs.md](https://github.com/salejandro/Comparative-Genomics-MGG/blob/main/omicron_seqs.md).

### Data files availability:

[Sarbecoviruses](http://www.ub.edu/molevol/CG-MGG/sarbecoviruses.fasta) (FASTA format)  
[Omicron BA1](http://www.ub.edu/molevol/CG-MGG/omicron-BA1.fasta) (Compressed FASTA format)
___

## Software and bioinformatics tools

Before starting the practice, you must (i) create and activate a new conda environment, (ii) install all the necesary tools and (iii) check that everything works correctly.

1. To create the environment, open the terminal app and type:

   ```bash
   conda create --name cgenv python=3.9
   ```
   > Notice that you are creating an enviroment based on `Python 3.9`. This is an important requirement of some of the utilities that you will use during the practice, and one of the main raisons for which you are using conda. You can name the environment whatever you want, as long as you activate the correct environment to work on the rest of the practice

</br>
 
2. To activate the environment:
 
   ```bash
   conda activate cgenv 
   ```
   > Make sure that your environment is activated (the name of the environment appears in brackets at starting the command line) before moving on to the next steps

</br>

3. Now, you are ready to install the programs and utilities for the practice. For the first part of the practice (session on filogenomics), you need tools for multiple sequence alignment (mafft) and maximum likelihood phylogenetic reconstruction (raxml-ng):

   **1. `mafft`**
   
   - Linux and Mac:
   
   ```bash
   conda install -c bioconda mafft
   ```  
   
   **2.`raxml-ng`**
   
   - Linux and Mac:
   
   ```bash
   conda install -c bioconda raxml-ng
   ```
   > You will also use some `Python` scripts that are available in the 'scripts' folder

</br>

4. Finally, for the second part of the practice (session on selective pressures), you need a progam to identify and exclude identical and very similar sequences (tn93), tools for codon-guided multiple sequence aligment (bealign and bam2msa - from the Bioext package), a the program to estimate selective constraints on amino acid positions (hyphy):

   **1. `tn93`**    

   - Linux and Mac:
   
   ```bash
   conda install -c bioconda 'tn93>=1.0.7'
   ```

   **2.` Bioext`**

   - Linux:

   ```bash
   conda install -c bioconda python-bioext
   ```
   > If you encountered problems with this command in Linux, try to install this tool using pip commands (see instructions for Mac)
   
   - Mac:
   
   ```bash
   pip install Cython pysam Bio Bioext
   ```
   
   **3.` hyphy`**

   - Linux and Mac:

   ```bash
   conda install -c bioconda hyphy
   ```  
   > You will also use some `Python` scripts that are available in the 'scripts' folder

</br>
   
5. Once all the bioinformatics tools have been installed, test that they work correctly by typing the name of the program with the flag -`h`: 
   
   ```bash
   mafft -h
   raxml-ng -h
   tn93-cluster -h
   bealign -h
   bam2msa -h
   hyphy -h
   ```
   > If the installation has been successful, the options for each program will be listed on the screen

---

## 
