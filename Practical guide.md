# Practial objectives

The objectives of this practical are....

# Data

The genome sequences for this practical have been retrieved from [GISAID](https://gisaid.org/) database and correspond to a subset of 4717 high-quality genomes of the BA.1
variant (Omicron) sequenced between xxxxx and xxxxxx. You can find the metadata associated with these genomes in the file BA.1.metadata.

# Bioinformatics software and tools

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


