NGSpeciesID
===========

NGSpeciesID is a tool for clustering and consensus forming of targeted ONT reads. This repository is a modified version of [isONclust](https://github.com/ksahlin/isONclust), where consensus and polishing feautures have been added.

NGSpeciesID is distributed as a python package supported on Linux / OSX with python v>=3.4. [![Build Status](https://travis-ci.org/ksahlin/NGSpeciesID.svg?branch=master)](https://travis-ci.org/ksahlin/NGSpeciesID).

Table of Contents
=================

  * [INSTALLATION](#INSTALLATION)
    * [Using conda](#Using-conda)
    * [Using pip](#Using-pip)
    * [Downloading source from GitHub](#Downloading-source-from-github)
    * [Dependencies](#Dependencies)
    * [Testing installation](#testing-installation)
  * [USAGE](#USAGE)
    * [Iso-Seq](#Iso-Seq)
    * [Oxford Nanopore](#Oxford-Nanopore)
    * [Output](#Output)
    * [Parameters](#Parameters)
  * [CREDITS](#CREDITS)
  * [LICENCE](#LICENCE)



INSTALLATION
----------------

**Currently [medaka](https://github.com/nanoporetech/medaka) needs to be installed separately.**

### Using conda
Conda is the preferred way to install NGSpeciesID.

1. Create and activate a new environment called NGSpeciesID

```
conda create -n NGSpeciesID python=3 pip 
source activate NGSpeciesID
```

2. Install NGSpeciesID 

```
pip install NGSpeciesID
```
3. You should now have 'NGSpeciesID' installed; try it:
```
NGSpeciesID --help
```

Upon start/login to your server/computer you need to activate the conda environment "NGSpeciesID" to run NGSpeciesID as:
```
source activate NGSpeciesID
```

4. Install [medaka](https://github.com/nanoporetech/medaka).

### Using pip 

To install NGSpeciesID, run:
```
pip install  NGSpeciesID
```
`pip` will install the dependencies automatically for you. `pip` is pythons official package installer and is included in most python versions. If you do not have `pip`, it can be easily installed [from here](https://pip.pypa.io/en/stable/installing/) and upgraded with `pip install --upgrade pip`. 


### Downloading source from GitHub

#### Dependencies

Make sure the below listed dependencies are installed (installation links below). Versions in parenthesis are suggested as NGSpeciesID has not been tested with earlier versions of these libraries. However, NGSpeciesID may also work with earliear versions of these libaries.
* [parasail](https://github.com/jeffdaily/parasail-python)
* [pysam](http://pysam.readthedocs.io/en/latest/installation.html) (>= v0.11)

In addition, please make sure you use python version >=3.4. NGSpeciesID will not work with python 2.

With these dependencies installed. Run

```sh
git clone https://github.com/ksahlin/NGSpeciesID.git
cd NGSpeciesID
./NGSpeciesID
```

### Testing installation

**TBD**



USAGE
-------

NGSpeciesID needs a fastq file generated by an Oxford Nanopore basecaller.

```
NGSpeciesID --ont --consensus --medaka --fastq [reads.fastq] --outfolder [/path/to/output] 
```
The argument `--ont` simply means `--k 13 --w 20`. These arguments can be set manually without the `--ont` flag. Specify number of cores with `--t`. 


### Output

The output consists of clustering and consensus information.

* The final cluster information is given in a tsv file `final_clusters.tsv` present in the specified output folder.
* Draft spoa consensus sequences of each of the clusters are given as consensus_reference_X.fasta (where X is a number).
* A folder named “medaka_cl_id_X” is created for each spoa consensus. Each medaka outfolder contains medakas output, including the final polished consensus named (by medaka) as “consensus.fasta”.


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


