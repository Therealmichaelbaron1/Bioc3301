# Bioc3301
This is my Bioc3301 collection of Cirrus scripts

You will find underneath a workflow representing the order in which I used the scripts, and what for

![img](https://i.imgur.com/Jux6VRG.png)



![img](https://i.imgur.com/fvHkZg0.png)

<details>
  <summary>Click to expand SPLIT PICK OTU</summary>

# Split and Pick OTUS

#!/bin/bash --login

#PBS  -l walltime=01:00:00

#PBS -l select=1:ncpus=8

#PBS -N 2018_new_try_split_pick2

#PBS -A d411-training

cd $PBS_O_WORKDIR

module load miniconda/python2

#loading virtualenv

echo "loading virtualenv"

#NB qiime1 and not qiimel (number one not letter l)

source activate qiime1

#setting temporary directory

export TMPDIR=~/qiime_tmp

#splitting libraries

echo "splitting libraries"

time split_libraries_fastq.py --barcode_type 12 -i Read1.fastq.gz -b Index.fastq.gz -o slout -m map.tsv

#counting sequences

echo "Counting sequences"

time count_seqs.py -i slout/seqs.fna

#picking OTUs

echo "Picking OTUs with open reference"

time pick_closed_reference_otus.py -i slout/seqs.fna -o otus -a\
-O 16

source deactivate
</details>


![img](https://i.imgur.com/a8BvZCR.png)

<details>
  <summary>Click to expand CORE DIVERSITY ANALYSIS </summary>

# core_diversity_analysis

#!/bin/bash --login

#PBS  -l walltime=01:00:00

#PBS -l select=1:ncpus=8

#PBS -N 2018_data_core_diversity

#PBS -A d411-training

cd $PBS_O_WORKDIR

module load miniconda/python2

#loading virtualenv

echo "loading virtualenv"

#NB qiime1 and not qiimel (number one not letter l)

source activate qiime1

#setting temporary directory

export TMPDIR=~/qiime_tmp

#core diverisity analysis

core_diversity_analyses.py -o cdout -i otus/otu_table.biom -m 2018_02_smb/map.tsv -t otus/97_otus.tree -e 18284 

source deactivate
</details>
  

![img](https://i.imgur.com/ZXZikWw.png)

<details>
  <summary>Click to expand GROUP SIGNIFICANCE</summary>

# group_significance

#!/bin/bash --login

#PBS -l walltime=01:00:00

#PBS -l select=1:ncpus=4

#PBS -N 2018_compare

#PBS -A d411-training

cd $PBS_O_WORKDIR

module load miniconda/python2

#loading virtualenv

echo "loading virtualenv"

#NB qiime1 and not qiimel (number one not letter l)

source activate qiime1

#setting temporary directory

export TMPDIR=~/qiime_tmp

group_significance.py  -i /lustre/home/d411/zcbtaol/otus/otu_table.biom -m map.txt -c SamplePh -o groupsig.txt

source deactivate
</details>


![img](https://i.imgur.com/O4ndbdA.png)

<details>
  <summary>Click to expand JACKKNIFED BETA DIVERSITY</summary>

# jackknifed_beta_diversity

#!/bin/bash --login

#PBS -l walltime=00:30:00

#PBS -l select=1:ncpus=16

#PBS -N _2017_cr_nojoin_no_golay_parallel

#PBS -A d411-training

cd $PBS_O_WORKDIR

module load miniconda/python2

#loading virtualenv

echo "loading virtualenv"

#NB qiime1 and not qiimel (number one not letter l)

source activate qiime1

#setting temporary directory

export TMPDIR=~/qiime_tmp

jackknifed_beta_diversity.py -i otus/otu_table.biom -m map.tsv -t otus/97_otus.tree -e 18284 -o jackout

source deactivate
</details>


![img](https://i.imgur.com/mK1Xy7E.png)

<details>
  <summary>Click to expand OBSERVATION METADATA CORRELATION</summary>

# observation_metadata_correlation

#!/bin/bash --login

#PBS -l walltime=01:00:00

#PBS -l select=1:ncpus=4

#PBS -N 2018_compare2

#PBS -A d411-training

cd $PBS_O_WORKDIR

module load miniconda/python2

#loading virtualenv

echo "loading virtualenv"

#NB qiime1 and not qiimel (number one not letter l)

source activate qiime1

#setting temporary directory

export TMPDIR=~/qiime_tmp

observation_metadata_correlation.py -s pearson -i /lustre/home/d411/zcbtaol/otus/otu_table.biom -m map.txt -c SamplePh -o spearmanmetadataout.txt

source deactivate
</details>
