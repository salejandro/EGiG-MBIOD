<p align="left">
<img src="http://www.ub.edu/molevol/CG-Master/biodiversity.png" width="300" height="130"><img align="right" src="http://www.ub.edu/molevol/CG-Master/logo2.png" width="400" height="80">  
</p>

# Comparative Genomics
## Computer lab 3: Selection analyses in Omicron BA.1 lineage

Instructor: **Alejandro Sánchez**  

</br>

## Introduction

The objective of this practice is to increase your understanding of the main concepts behind inference of selection at the molecular level by doing some practical work with a small representation (just a few examples) of the state-of-the-art computational tools to estimate phylogenetic relationships between genes and genomes, and to determine the functional consequences of mutations in these sequences. This fundamental knowledge has great importance and applicability in genetic and biomedical research, and in biodiversity studies.

The biological model used in this practices is the BA.1 sublineage of the SARS-CoV2 variant of concern (VOC) Omicron.

</br>
</br>

<p align="center">
<img src="http://www.ub.edu/molevol/CG-MGG/sars2.png" width="900" heigh="900">
</p>

> **Fig. 1** Schematic diagrams of the SARS-CoV-2 virus particle (a) and genome (b). The genome encodes sixteen nonstructural proteins (nsp1–11, 12–16) and six accessory proteins. Plpro: papain like protease, 3CLPro: 3C-like proteinase, *__RdRp: RNA-dependent RNA polymerase__*, Hel:
Helicase, *__S: spike__*. Modified from [Zang et al. (2021)](https://www.nature.com/articles/s41392-021-00653-w).

</br>

In the first part of this practice you will reconstruct the phylogenetic relationships of the surveyed sequences. Due to the mosaic nature of the genomes of viruses such as the SARS-CoV2, which originated after recombination between different strains of Sarbecoviruses, it is highly recomendable to use the local tree of the region studied rather than all the genomes to built the phylogenetic tree for the selection analyses ([Temmam et al. 2022](https://www.nature.com/articles/s41586-022-04532-4)). Here, you will reconstruct the gene tree of the S-gene with most informative sequences. In the second part, you will estimate the effect on virus fitness of the missense mutations (amino acid replacements) accumulated in the sublineage BA.1 of Omicron, and compare this effect with the estimated impact for the same positions prior to the emergence of this VOC (all SARS-CoV-2 near full-length genome sequences present in GISAID on 21/11/2021 that passed some quality control checks; [Martin et al. 2022](https://academic.oup.com/mbe/article/39/4/msac061/6553617)). This analysis in an example of how the accumulation of mutations with individual fitness costs but adaptive in combination with other mutations, could alter the function of a protein (Spike), while remained completely undetected despite unprecedented global genomic surveillance efforts.

</br>
</br>

<p align="center">
<img src="http://www.ub.edu/molevol/CG-MGG/omicronfreq.png" width="900" heigh="900">
</p>

> **Fig. 2** Frequency of the different SARS-CoV2 variants (colored by clade) at different time points as given in the Latest global SARS-CoV-2 analysis with GISAID data ([Nextstrain SARS-CoV-2 resources](https://nextstrain.org/ncov/gisaid/global/all-time); last accessed 16/11/2022). The sublineage BA.1 of Omicron correspond to the clade 21K.

</br>

---

## Unix based OS

If you are not familiarized with the terminal app in Unix based operating systems, please take a look at these introductory manuals:

   + [Linux](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview)
   + [Mac](https://support.apple.com/en-in/guide/terminal/welcome/mac)

## Software and Data

Before starting the practice, please confirm that you have installed `Docker` in your computer (you can find system requirements and regular installation instructions [here](https://docs.docker.com/engine/install/ubuntu/). The reason for installing `Docker` and working within a container of a prebuilt image based on a conda environment is to perform the computer lab in a stable environment with a specific `Python` version (3.9), as well as to avoid the complexity of software (iqtree, raxml-ng or hyphy) compilation on different operating systems with different configurations.

Once `Docker` is installed and working correctly, it is time to download the image with which you will work in this practice. Open the terminal app in your computer and type:

First, you can create the docker group and add your user:
   
   ```bash
   sudo groupadd docker
   sudo usermod -aG docker <USER>
   newgrp docker
   ```

Then, download the image form the Docker hub:

   ```bash
   docker pull asancheg/cg-mbiod:r160323v2
   ```

Check that the image has been downloaded correctly:

   ```bash
   docker images
   ```

Now, you can run a container to do the work of this practice:

   ```bash
   docker run -it --name <CONTAINER_NAME> asancheg/cg-mbiod:r160323v2 /bin/bash
   cd home
   ```
   
> Some useful commands to work with containers and images:
> `exit` to exit a container   
> `docker stop <CONTAINER_NAME>`, to stop a container   
> `docker ps -a`, to list containers (to know, for example, containers names)   
> `docker rm <CONTAINER_NAME>`, to remove a container    
> `docker start <CONTAINER_NAME>`, to start a stopped container (this command only starts the container, see the next command)    
> `docker exec -it <CONTAINER_NAME> \/bin/bash'`, to run the started container (see command above)    
> `docker rmi <IMAGE_ID>`, to remove images (use _docker images_ to know the <IMAGE_ID>)   
> `docker system prune -a`, to remove all images  

</br>

**IMPORTANT WARNING: Many of the tools that will be used in this practice are not available for Windows operating systems, even when conda (anaconda) environment is installed. In general, bioinformatics programs for manipulating and analyzing genomic data are only available or tested for Linux and Mac.** If you have a Windows based PC or laptop, _I strongy recommend to install a Linux distribution on a virtual machine_ (for example [wsl](https://learn.microsoft.com/en-us/windows/wsl/install)).

### Data files availability:

The genome sequences for this practice were retrieved either from [GenBank](https://www.ncbi.nlm.nih.gov/genbank/) or [GISAID](https://gisaid.org/) databases and correspond to a subset of 4717 high-quality genomes of the BA.1 variant (Omicron) collected world wide between 05/01/2020 and 22/06/2022. You can find the metadata associated with these genomes in the file BA.1.metadata, and the instructions for retrieve these sequences in the file [GISAID-BA.1-sequences.md](https://github.com/salejandro/Comparative-Genomics-MBIOD/blob/main/GISAID-BA.1-sequences.md).

The [FASTA](https://es.wikipedia.org/wiki/Formato_FASTA) files with the data decribed above will be in the folder "/data" of your home (/home).

</br>

<p align="center">
<img src="http://www.ub.edu/molevol/CG-MGG/conda.jpeg" height="60">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="http://www.ub.edu/molevol/CG-MGG/python.png" height="60">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="http://www.ub.edu/molevol/CG-MGG/mafft.png" height="40">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="http://www.ub.edu/molevol/CG-MGG/iqtree-logo.svg" height="70">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="http://www.ub.edu/molevol/CG-MGG/raxmlng.png" height="40">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="http://www.ub.edu/molevol/CG-MGG/logo.svg" height="70">
</p>

---

## Part 1. Phylogenetic tree

Phylogenomics aims at establishing the evolutionary relationships between organisms using genome data. However, not all parts of a genome evolve at the same rate or are completely independently since their split from a common ancestor, due to, for example, past recombination events or strong positive selection acting deferentially on particular genes and lineages. These scenarios may cause phylogenies to reflect different histories across the genome. Sarbecoviruses genomes, including SARS-CoV2, are not an exception. Since the program `hyphy` estimates synonymous and non-synonymous substitution rates in a maximum likelihood (ML) phylogetic framework using different (complementary) [methods](https://www.hyphy.org/methods/selection-methods/), you will first need a phylogenetic tree with the relationships among the Spike coding sequences after some filtering:

### Workflow:

1.  You have first to trim down genomic sequences to the S-gene neighborhood using a `Python` script:

      ```bash
      FILE="omicron-BA1.fasta" # rename accordingly
      python3.9 scripts/filter-sites.py data/$FILE  20000,26000 > ${FILE}.S.raw
      
      ``` 
   > You trim sequences to this wide range be sure to include the whole S-gene of all the sequences
   
</br>

2.  Then, you will use `bealign` to align trimmed Sarbecovirus sequences to the S-gene of the SARS-CoV2 reference ([NC_045512](https://www.ncbi.nlm.nih.gov/nuccore/NC_045512)), using a codon alignment algorithm:

      ```bash
      bealign -r CoV2-S ${FILE}.S.raw ${FILE}.S.bam
      bam2msa ${FILE}.S.bam ${FILE}.S.msa
      
      ```
      > This step allows to delimit the alignment to only the coding region of the S-gene (which is already available as an option for -r in `bealign`)

</br>

3.  Because selection analyses gain no or minimal power from including identical or very similar sequences, you will filter BA.1 sequences using pairwise genetic distances. To do that, you will use some `Python` scripts and the program `tn93`:

      + To identify and remove all identical sequences up to ambiguous nucleotides:
         ```bash
         python3.9 scripts/exact-copies.py  ${FILE}.S.msa > ${FILE}.u.clusters.json
         python3.9 scripts/cluster-processor.py ${FILE}.u.clusters.json > ${FILE}.S.u.fas
         
         ```

      + To identify and remove all remaining sequences that are within t= 0.0 ([Tamura-Nei 93](https://pubmed.ncbi.nlm.nih.gov/8336541/) distance) genetic distance:
         ```bash
         tn93-cluster -t 0.0 ${FILE}.S.u.fas > ${FILE}.t0.clusters.json
         python3.9 scripts/cluster-processor.py ${FILE}.t0.clusters.json > ${FILE}.S.all.fas
         
         ```

      + To reduce the set of sequences to clusters that are all within t=0.00075 distance of one another:
         ```bash
         tn93-cluster -t 0.00075 ${FILE}.S.all.fas > ${FILE}.t1.clusters.json
         python3.9 scripts/cluster-processor.py ${FILE}.t1.clusters.json > ${FILE}.S.uniq.fas
         
         ```

      + To identify and remove outliers, low quality or misclassified sequences that are t=0.002 away from the gererated clusters:

         ```bash
         tn93-cluster -t 0.002 ${FILE}.S.uniq.fas > ${FILE}.t2.clusters.json
         python3.9 scripts/cluster-processor.py ${FILE}.t1.clusters.json ${FILE}.t2.clusters.json > ${FILE}.S.uniq-all.fas 2> ${FILE}.S.blacklist.txt
         
         ```
   
      + Finally, to build a majority consensus for each remaining cluster and remove clusters that comprise fewer than 3 sequences:
         ```bash
         python3.9 scripts/cluster-processor-consensus.py 3 ${FILE}.S.msa ${FILE}.S.uniq-all.fas ${FILE}.t1.clusters.json ${FILE}.t0.clusters.json ${FILE}.u.clusters.json > ${FILE}.S.uniq.fas
         
         ```
   
 </br>

4.  And, lastly, you will use the program `raxml-ng` to obtain the tree: 

      ```bash
      threads=4
      raxml-ng --redo --threads $threads --msa ${FILE}.S.uniq.fas --tree pars{5} --model GTR+G+I
      cp ${FILE}.S.uniq.fas.raxml.bestTree ${FILE}.S.uniq.tree
      
      ```
      > Remember to update the variable 'threads'  according to your resources

---

## Part 2. Analysis of selection in the BA.1 sublineage

In the second part of the practice, we are particularly interested in identifying Spike amino acids (codon sites) displaying evolutionary patterns that differed between Omicron sublineage BA.1 and other SARS-CoV-2 lineages that circulated prior to the emergence of Omicron. The rationale of this analysis is to investigate how could so many amino acid changes that adaptively alter the function of Spike accumulate at these sites, without being detected despite unprecedented global genomic surveillance efforts. You will first apply the codon substitution models implemented in the `hyphy` package to estimate functional constraints acting on non-synonymous sites of this gene after the emergence of Omicron, and then, you will compare your results with those from same analyses on non-Omicron genomes (Table 1).

##### **Table 1**. Frequencies in non-Omicron SARS-CoV-2 genomes of some Spike amino acid mutations ([Martin et al. 2022](https://academic.oup.com/mbe/article/39/4/msac061/6553617)). Since the length of the Spike protein is highly conserved across the BA.1 sequences, the numbering of the positions in the table will match that of the alignment that your will use in this practice

<p align="center">
<img src="http://www.ub.edu/molevol/CG-MGG/table1.png" width="800">
</p>

### Workflow:

Once you have the tree, you are ready to run a selection analysis with different methods implemented in the software hyhpy. For this practice, you will run [BUSTED](https://academic.oup.com/mbe/article-lookup/doi/10.1093/molbev/msv035), [FEL](https://academic.oup.com/mbe/article/22/5/1208/1066893), [MEME](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1002764) and [BGM](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.0030231) analyses:</br>

   + The first method is **BUSTED** (**B**ranch-Site **U**nrestricted **S**tatistical **T**est for **E**pisodic **D**iversification). By applying this method, you are asking whether the S-gene has experienced **positive selection at at least one site on at least one branch** of the tree (=one BA.1 genome):

      ```bash
      HYPHYMPI="mpirun --allow-run-as-root -np $threads HYPHYMPI"
      $HYPHYMPI busted --alignment ${FILE}.S.uniq.fas --tree ${FILE}.S.uniq.tree --branches Internal --rates 3 --starting-points 5
      
      ```
   
   + When applying **FEL** (**F**ixed **E**ffects **L**ikelihood) to your data, you obtain the ML estimate of non-synoymous (dN) and synonymous (dS) substitution rates in each codon site (amino acid position) for the entire Spike CDS alignment acroos the phylogeny. Therefore, you are assuming **constant selection pressures along the entire history of BA.1 sequences**:
   
      ```bash
      $HYPHYMPI fel --alignment ${FILE}.S.uniq.fas  --tree ${FILE}.S.uniq.tree --branches Internal  --ci Yes
      
      ```
   
   + Whith **MEME** (**M**ixed **E**ffects **M**odel of **E**volution) you will identify episodes of **positive selection** in the S-gene affecting a **small subset of branches at individual sites**. In other words, episodic positive or diversifying selection:
   
      ```bash
      $HYPHYMPI meme --alignment ${FILE}.S.uniq.fas  --tree ${FILE}.S.uniq.tree --branches Internal
      
      ```
        > Notice that in the two last models (FEL and MEME) you use the MPI (Message Passing Interface) version of the program hyphy. This version allows multiprocessing (parallel programing), which is very helpful when running computational intensive models  
     
   + Finally, the **BGM** (**B**ayesian **G**raphical **M**odel) method is a tool for detecting correlated amino acid substitutions in the Spike protein of Omicron BA.1. This correlation should be suggestive of **coevolutionary interactions between amino acid positions** in this protein:
     
      ```bash
      $HYPHYMPI bgm --alignment ${FILE}.S.uniq.fas --tree ${FILE}.S.uniq.tree --branches Internal --min-subs 2 --steps 1000000 --burn-in 100000
      
      ```

To visualize json results use [hyphy-vision tool](http://vision.hyphy.org/)

***

<p><strong>Doubts?</strong></p>

Contact A. Sánchez: elsanchez(at)ub.edu 


