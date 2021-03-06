NGSpeciesID
===========

NGSpeciesID is a tool for clustering and consensus forming of targeted ONT reads. This repository is a modified version of [isONclust](https://github.com/ksahlin/isONclust), where consensus and polishing feautures have been added.

NGSpeciesID is distributed as a python package supported on Linux / OSX with python v3.6. [![Build Status](https://travis-ci.org/ksahlin/NGSpeciesID.svg?branch=master)](https://travis-ci.org/ksahlin/NGSpeciesID).

Table of Contents
=================

  * [INSTALLATION](#INSTALLATION)
    * [Using conda](#Using-conda)
    * [Testing installation](#testing-installation)
  * [USAGE](#USAGE)
    * [Filtering and subsampling](#filtering-and-subsampling)
    * [Removing primers](#removing-primers)
    * [Output](#Output)
  * [CREDITS](#CREDITS)
  * [LICENCE](#LICENCE)



INSTALLATION
----------------

**NOTE**: If you are experiencing issues (e.g. [this one](https://github.com/rvaser/spoa/issues/26)) with the third party tools  [spoa](https://github.com/rvaser/spoa) or [medaka](https://github.com/nanoporetech/medaka) in the all-in-one installation instructions below, please install the tools manually with their respective installation instructions [here](https://github.com/rvaser/spoa#installation) and [here](https://github.com/nanoporetech/medaka#installation).  

### Using conda
Conda is the preferred way to install NGSpeciesID.

1. Create and activate a new environment called NGSpeciesID

```
conda create -n NGSpeciesID python=3.6 pip 
conda activate NGSpeciesID
```

2. Install NGSpeciesID 

```
pip install NGSpeciesID
conda install --yes -c conda-forge -c bioconda medaka==0.11.5 openblas==0.3.3 spoa racon minimap2
```
3. You should now have 'NGSpeciesID' installed; try it:
```
NGSpeciesID --help
```

Upon start/login to your server/computer you need to activate the conda environment "NGSpeciesID" to run NGSpeciesID as:
```
conda activate NGSpeciesID
```



### Testing installation

0. Activate conda environment
```
conda activate NGSpeciesID
```

1. Make a new directory and navigate to it
```
mkdir test_ngspeciesID
cd test_ngspeciesID
```

2. Download the test fastq file called "sample_h1.fastq" (filesize 390kb)

```
curl -LO https://raw.githubusercontent.com/ksahlin/NGSpeciesID/master/test/sample_h1.fastq
```

3. Run the NGSpecies command on test file. Outputs will be saved in "/test_ngspeciesID/sample_h1/", where the final polished consensus file ("consensus.fasta") is located in the "/test_ngspeciesID/sample_h1/medaka_cl_id_<cluster number>" directory.

```
NGSpeciesID --ont --fastq sample_h1.fastq --outfolder ./sample_h1 --consensus --medaka
```


USAGE
-------

NGSpeciesID needs a fastq file generated by an Oxford Nanopore basecaller.

```
NGSpeciesID --ont --consensus --medaka --fastq [reads.fastq] --outfolder [/path/to/output] 
```
The argument `--ont` simply means `--k 13 --w 20`. These arguments can be set manually without the `--ont` flag. Specify number of cores with `--t`. 


NGSpeciesID can also run with racon as polisher. For example

```
NGSpeciesID --ont --consensus --racon --racon_iter 3 --fastq [reads.fastq] --outfolder [/path/to/output] 
```
will polish the consensus sequences with racon three times.

### Filtering and subsampling

NGSpeciesID employs quality filtering of the reads based on read Phred scores. However, we recommend also removing reads much shorter or longer than the intended target, which often represent chimeras or contaminations. This can be done by specifying the `--m (intended target length)` and `--s (maximum deviation from target length)`. NGSpeciesID also has the feature of subsampling reads using parameter `--sample_size`. Altogether, if we want to filter out reads outside the length interval [700,800] and using a subset of 300 reads (if the dataset consists of more reads) we could run

```
NGSpeciesID --ont --sample_size 300 --m 750 --s 50 --consensus --medaka --fastq [reads.fastq] --outfolder [/path/to/output]
```

By default, length filtering and subsampling are not invoked if parameters are not specified.

### Removing primers

If customized primers are to be expected in the reads thay can be detected and removed. The primer file is expected to be in fasta format. Here is an example of a primer file:

```
>MCB869_ONT_R
CGATCAATCCCCTAACAAACTAGG
>MCB398_ONT_F
TACCATGAGGACAAATATCATTCTG
```
NGSpeciesID searches for primes in a window of Xbp (parameter, default 150bp) at the beginning and end of each consensus.


Trimming of primers is performed after consensus forming and can be invoked as
```
NGSpeciesID --ont --consensus --medaka --fastq [reads.fastq] --outfolder [/path/to/output] --primer_file [primers.fa]
```

`NGSpeciesID` can also remove universal tails. Trimming of tails is performed after consensus forming and can be invoked as

```
NGSpeciesID --ont --consensus --medaka --fastq [reads.fastq] --outfolder [/path/to/output] --remove_universal_tails
```

The two options are mutually exclusive, i.e., only one of them can be run.

### Output

The output consists of the polished consensus sequences along with some information about clustering.

* Polished consensus sequence(s). A folder named “medaka_cl_id_X”[/"racon_cl_id_X"] is created for each predicted consensus. Each such folder contains a sequence “consensus.fasta” which is the final output of NGSpeciesID. 
* Draft spoa consensus sequences of each of the clusters are given as consensus_reference_X.fasta (where X is a number).
* The final cluster information is given in a tsv file `final_clusters.tsv` present in the specified output folder.


In the cluster TSV-file, the first column is the cluster ID and the second column is the read accession. For example:

```
0 read_X_acc
0 read_Y_acc
...
n read_Z_acc
```
if there are n reads there will be n rows. Some reads might be singletons. The rows are ordered with respect to the size of the cluster (largest first).



CREDITS
----------------

Please cite [1] when using NGSpeciesID.

1. TBA



LICENCE
----------------

GPL v3.0, see [LICENSE.txt](https://github.com/ksahlin/NGSpeciesID/blob/master/LICENCE.txt).


