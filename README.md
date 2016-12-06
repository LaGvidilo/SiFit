## Overview ##

**SiFit** is a tool for reconstructing phylogenetic tree from noisy mutation profile of single cells. Given an imperfect noisy genotype matrix from single cells, **SiFit** employs a heuristic search algorithm to infer the Maximum Likelihood (ML) phylogenetic tree under a finite-site model of evolution. Along with tree inference, error rates of the single-cell dataset can also be estimated.

## Dependencies ##

* PhyloNet ([https://bioinfo.cs.rice.edu/phylonet]())
* The Apache Commons Mathematics Library ([http://commons.apache.org/proper/commons-math]())
* Parallel Colt ([https://mvnrepository.com/artifact/net.sourceforge.parallelcolt/parallelcolt]())

## Installation ##

The binary **SiFit.jar** is directly downloadable.

## Usage ##

To run SiFit for tumor phylogeny inference, the genotype matrix should be obtained from single-cell DNA sequencing data. An example genotype matrix **dataset_20cells.txt** is provided in the ***exampleFiles*** directory. The corresponding true phylogenetic tree is provided in the file **Tree_20cells.newick** in newick format. To infer the maximum likelihood (ML) tree from this given dataset, SiFit can be executed as follows.

```
java -jar SiFit.jar -m 20 -n 446 -fp 0.002 -fn 0.2 -iter 10000 -df 1 -ipMat exampleFiles/dataset_20cells.txt
```
SiFit outputs the ML tree in newick format and also outputs the ML false negative rate.

## Input Files ##
### Genotype Matrix ###
The mutational profile of single cells are represented as a genotype matrix and it has to be provided as input. The first column of the matrix contains the position of the mutation. Each of the rest of the columns represents the mutation profile of a single cell. Each row represents one mutation. The first entry of each row contains the position of the mutation. The other entries represent the genotype of the corresponding cell for the given mutation. Columns are separated by a white space character. The genotype matrix can be binary/ternary.

a)*** Binary:*** 
The genotype information represents the presence/absence of a mutation. The entry at position [i, j+1] should be

* 0 if mutation i is not observed in cell j,
* 1 if mutation i is observed in cell j, or
* 3 if the genotype information is missing.

a)*** Ternary:*** 
The genotype information represents homozygous reference, heterozygous variant and homozygous variant respectively. The entry at position [i, j+1] should be

* 0 if mutation i is not observed in cell j,
* 1 if mutation i is heterozygous variant in cell j, 
* 2 if mutation i is homozygous variant in cell j, or
* 3 if the genotype information is missing.

### Cell Names (Optional) ###
A file specifying the names of the single cells. The names should appear in a row in the same order as in the genotype matrix separated by white space character.

### The True Tree (Optional) ###
For simulated datasets, for which the true tree is known, the true tree can be provided to SiFit for comparing the inferred tree. The true tree should be provided as an input file in Newick format. If the true tree is provided, SiFit will also report the tree reconstruction error.

## Output File ##
### Inferred Tree ###
The ML tree inferred by SiFit is written to file in Newick format. The output file obtains the base name from the input file and adds an extension ```_mlTree.newick```.

## Arguments ##
* ```-m <Integer>``` Replace <Integer> with the number of cells in the dataset.
* ```-n <Integer>``` Replace <Integer> with the number of mutations (rows) in the dataset.
* ```-ipMat <filename>``` Replace <filename> with the path to the file containing the genotype matrix.