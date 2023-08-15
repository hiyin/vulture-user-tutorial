---
layout: default
title: Quick start
parent: Localrun
nav_order: 1
---

## <a name="Quick start"></a>Quick start

The instructions are tested on the following system:

```shell
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.2 LTS
Release:        22.04
Codename:       jammy
```

# Install denpendency for Vulture
## <a name="require"></a>Requirements
* Input data: 10x Chromium scRNA-seq reads
* R >= v4.0.0
* DropletUtils >= v1.10.2
* Samtools >= v1.13
* STAR >= v2.7.9a or
* cellranger >= 6.0.0 or
* Kallisto/bustools >= 0.25.1 or
* salmon/alevin >= v1.4.0

## For a quick start, you can install all above listed dependencies via pre-prepared bash scripts
Watch quickstart video at Youtube [Vulture - Quickstart](https://youtu.be/aGCMi87tVUI)

Download [vulture-quickstart.sh](https://vulture-reference.s3.ap-east-1.amazonaws.com/vulture-quickstart.sh) to install every dependencies in one click.

```sh
# run quickstart script
wget https://vulture-reference.s3.ap-east-1.amazonaws.com/vulture-quickstart.sh 
bash ./vulture-quickstart.sh
```

## Install Java and Nextflow

Nextflow is a workflow manager that enables the development of portable and reproducible workflows. It supports deploying workflows on a variety of execution platforms including local, HPC schedulers, AWS Batch, Google Cloud Life Sciences, Kubernetes, Slurm, Singularity, PBS, LSF, among others. It also supports most popular cluster schedulers including SGE, SLURM, PBS, LSF, IBM Spectrum LSF, Sun Grid Engine, HTCondor, among others.
We apply Nextflow v22.10.tar.gz in this tutorial because it is the latest version that is compatible with the Vulture pipeline.

```sh
## Install Java
sudo apt install default-jdk

## Install nextflow 22.10.0
wget https://github.com/nextflow-io/nextflow/archive/refs/tags/v22.10.0.tar.gz
tar xvf v22.10.0.tar.gz
cd nextflow-22.10.0/
echo "export PATH=/home/ubuntu/nextflow-22.10.0:\$PATH" >> ~/.bashrc
sudo apt-get install -y graphviz jq
```


## Install DropletUtils
In this tutorial, the major dependency of the R package is the DropletUtils. We first install R and then install DropletUtils from the CRAN repository.

### Pick your CRAN/mirror to start installation
https://cran.r-project.org/mirrors.html

Choose your a mirror at https://mirror-hk.koddos.net/CRAN/. Click [Download R for Linux], then choose "ubuntu" in the next page. You will be directed to the download page where you will see prompts below.

Run these lines (if root, remove sudo) to tell Ubuntu about the R binaries at CRAN.
```sh
# update indices
sudo apt update -qq
# install two helper packages we need
sudo apt install --no-install-recommends software-properties-common dirmngr
# add the signing key (by Michael Rutter) for these repos
# To verify key, run gpg --show-keys /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc 
# Fingerprint: E298A3A825C0D65DFD57CBB651716619E084DAB9
wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc
# add the R 4.0 repo from CRAN -- adjust 'focal' to 'groovy' or 'bionic' as needed
sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"
```

Also, we need to install BiocManager (with its dependancy) to install DropletUtils from the Bioconductor repository.

```sh
sudo apt install build-essential
sudo apt-get update --fix-missing && \
sudo apt-get install -y wget bzip2 ca-certificates \
    libglib2.0-0 libxext6 libsm6 libxrender1 curl grep sed dpkg libcurl4-openssl-dev libssl-dev libhdf5-dev \
    git mercurial subversion procps \
    libxml-libxml-perl pigz awscli uuid-runtime time tini
sudo apt-get install libblas-dev liblapack-dev
sudo apt-get install gfortran
# Need sudo to install packages otherwise error
sudo Rscript -e 'install.packages("BiocManager")'
sudo Rscript -e 'BiocManager::install("DropletUtils")'
```

## Install other sequence alignment methods for Vulture

### Install STAR and samtools
STAR is the primary aligner for Vulture. We install STAR and samtools from the source code. The installation of STAR requires the installation of samtools. We install samtools first and then install STAR.

```sh
cd $HOME
sudo apt-get install libncurses5-dev
sudo apt-get install libbz2-dev
sudo apt-get install liblzma-dev
wget https://github.com/alexdobin/STAR/archive/2.7.9a.tar.gz && \
tar -xzf 2.7.9a.tar.gz && \
echo "export PATH=$HOME/STAR-2.7.9a/bin/Linux_x86_64_static:\$PATH" >> ~/.bashrc && \
wget https://github.com/samtools/samtools/releases/download/1.13/samtools-1.13.tar.bz2 && \
tar -xf samtools-1.13.tar.bz2 && \
cd samtools-1.13 && \
./configure --prefix=$HOME/samtools
make && make install && echo "export PATH=$HOME/samtools/bin:\$PATH" >> ~/.bashrc
. ~/.bashrc 
cd $HOME
```

KB is the second aligner for Vulture. KB is a python based package. We install KB from the source code.

```sh
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python3.8
sudo apt-get install python3-pip
pip install kb-python boto3 awscli
```
## Download the reference genome and annotation files for Vulture

We have provied the latest human-microbes-virus genome for the Vulture pipeline. The reference genome and annotation files are stored in the S3 bucket. We can use the wget to download the files. Also, barcode whitelist files are provided in the S3 bucket. We can download the barcode whitelist files for the 10X Genomics platform.

```sh
mkdir vmh_genome_dir
cd vmh_genome_dir
wget https://vulture-reference.s3.ap-east-1.amazonaws.com/human_host_viruses_microbes.viruSITE.NCBIprokaryotes.with_hg38.removed_amb_viral_exon.gtf
wget https://vulture-reference.s3.ap-east-1.amazonaws.com/human_host_viruses_microbes.viruSITE.NCBIprokaryotes.with_hg38.fa
wget https://vulture-reference.s3.ap-east-1.amazonaws.com/human_host_viruses.viruSITE.with_hg38.removed_amb_viral_exon.gtf
wget https://vulture-reference.s3.ap-east-1.amazonaws.com/human_host_viruses.viruSITE.with_hg38.fa
wget https://vulture-reference.s3.ap-east-1.amazonaws.com/3M-february-2018.txt
wget https://vulture-reference.s3.ap-east-1.amazonaws.com/737K-august-2016.txt
```

## Download the example data for Vulture

We have provided the example data for the Vulture pipeline. The example data are stored in the S3 bucket. We can use the wget to download the files.

```sh
cd $HOME
wget https://vulture-reference.s3.ap-east-1.amazonaws.com/example/SRR12570425_1.fastq.gz
wget https://vulture-reference.s3.ap-east-1.amazonaws.com/example/SRR12570425_2.fastq.gz
```

## Run Vulture

### Clone the Vulture repository

We can run Vulture with the following command. The output files will be stored in the output directory. The output directory should be created before running the Vulture pipeline. We need to clone the Vulture repository from the GitHub repository. We can run the Vulture pipeline with the following command.

```sh
cd $HOME
git clone https://github.com/holab-hku/Vulture.git
# running command
# mkdir output directory otherwise error
mkdir $HOME/Vulture_output
mkdir $HOME/Vulture_output/SRR12570425
```

### Run Vulture with the example data

We can run Vulture with the following command. The output files will be stored in the output directory. The output directory should be created before running the Vulture pipeline. We can run the Vulture pipeline with the following command.

```sh
# Run sequence alignment commands with specific parameters
perl $HOME/Vulture/scvh_map_reads.pl -t 12 -o $HOME/Vulture_output/SRR12570425 $HOME/vmh_genome_dir $HOME/SRR12570425_2.fastq.gz $HOME/SRR12570425_1.fastq.gz --soloStrand "Forward" --whitelist "$HOME/vmh_genome_dir/3M-february-2018.txt" --soloCBlen 16 --soloUMIstart 1 --soloUMIstart 17 --soloUMIlen 12 -soloUMIlen 12

# Run the scvh_filter_matrix.r script to filter the matrix
Rscript $HOME/Vulture/scvh_filter_matrix.r $HOME/Vulture_output/SRR12570425

# Run the scvh_analyze_bam.pl script to analyze the bam file
perl $HOME/Vulture/scvh_analyze_bam.pl $HOME/Vulture_output/SRR12570425

```