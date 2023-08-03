---
layout: default
title: Quick start
parent: Localrun
nav_order: 1
---

## <a name="Quick start"></a>Quick start

PLACEHOLDER

For Linux Machine 


## Install R

# Pick your CRAN/mirror to start installation
https://cran.r-project.org/mirrors.html

Here we choose Hong Kong mirror at https://mirror-hk.koddos.net/CRAN/. Click [Download R for Linux], then choose "ubuntu" in the next page. You will be directed to the download page where you will see prompts below.

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

## Install Docker
Installation methods can be found exactly at https://docs.docker.com/engine/install/ubuntu/.
You can install Docker Engine in different ways, depending on your needs:

We install using the apt repository. Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

```sh
# Set up the repository
# Update the apt package index and install packages to allow apt to use a repository over HTTPS:
 sudo apt-get update
 sudo apt-get install ca-certificates curl gnupg

# Add Docker’s official GPG key:
 sudo install -m 0755 -d /etc/apt/keyrings
 curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
 sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Use the following command to set up the repository:
 echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

```

# Install Docker Engine
```sh
# Update the apt package index:
 sudo apt-get update

# Install Docker Engine, containerd, and Docker Compose.
# To install the latest version, run:
 sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

#Verify that the Docker Engine installation is successful by running the hello-world image.
 sudo docker run hello-world
# This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.
```


sudo apt install build-essential


sudo apt-get update --fix-missing && \
    sudo apt-get install -y wget bzip2 ca-certificates \
    libglib2.0-0 libxext6 libsm6 libxrender1 curl grep sed dpkg libcurl4-openssl-dev libssl-dev libhdf5-dev \
    git mercurial subversion procps \
    libxml-libxml-perl pigz awscli uuid-runtime time tini

sudo apt-get install libblas-dev liblapack-dev

sudo apt-get install gfortran
# Install Nextflow
## Install Java
sudo apt install default-jdk

wget -qO- https://get.nextflow.io | bash

sudo apt-get install -y graphviz jq

# run from current directory
chmod +x nextflow
# run from anywhere
sudo mv nextflow /usr/local/bin/

# install DropletUtils


# Install samtools need following dependencies
# ncurses
sudo apt-get install libncurses5-dev

sudo apt-get install libbz2-dev

sudo apt-get install liblzma-dev

# dependenties other than R packages
wget https://github.com/alexdobin/STAR/archive/2.7.9a.tar.gz && \
tar -xzf 2.7.9a.tar.gz && \
echo "export PATH=/home/ubuntu/STAR-2.7.9a/bin/Linux_x86_64_static:\$PATH" >> ~/.bashrc && \
wget https://github.com/samtools/samtools/releases/download/1.13/samtools-1.13.tar.bz2 && \
tar -xf samtools-1.13.tar.bz2 && \
cd samtools-1.13 && \
./configure --prefix=/home/ubuntu/samtool
make &&     make install &&     echo "export PATH=/home/ubuntu/samtools/bin:\$PATH" >> ~/.bashrc 


# add bioconda https://bioconda.github.io/
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
conda config --set channel_priority strict



$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:deadsnakes/ppa
$ sudo apt-get update
$ sudo apt-get install python3.8
$ sudo apt-get install python3-pip
$ pip install kb-python boto3 awscli