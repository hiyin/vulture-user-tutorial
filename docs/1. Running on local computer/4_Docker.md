---
layout: default
title: Docker
parent: Localrun
nav_order: 4
---
# Run Vulture on local machines using docker 

The following instructions are for running Vulture on local machines using docker. The instructions are tested on the following system:

```shell
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.2 LTS
Release:        22.04
Codename:       jammy
```

## Install docker    

Place holder for docker installation

## Specify docker container in Nextflow config file

Open the "nextflow.config" file in the vulture/nextflow directory. This following snippet shows how to specify the docker container for the pipeline. The docker container is hosted on Docker Hub and can be pulled by Nextflow automatically. 

```shell
...
dockerlocal {
    docker.enabled = true
    process.container = 'junyichen6/vulture:0.0.1'
    docker.fixOwnership = true
    docker.containerOptions = "--user root"

}
...

```
## Specify the configuration file for an analysis

Before we start our analysis, we need to creat a configuration file for the analysis. Here is a snippet of how the "params.yaml" file looks like:

```shell
...
soloStrand: "Forward"
alignment: "STAR"
technology: "10XV3"
virus_database: "viruSITE.NCBIprokaryotes"
soloMultiMappers: "EM"
soloFeatures: "GeneFull"
inputformat: "fastq"
sampleSubfix1: "_1"
sampleSubfix2: "_2"
ref: [The full path of your reference genome direcory, e.g. /home/user/data/references]
samplepath: [The full path of your fastq samples, e.g /home/user/data/fastq]
read2urls:
- [The full path of your _2.fastq.gz file, e.g. /home/user/data/fastq/SRR12570125_2.fastq.gz]
read1urls:
- [The full path of your _1.fastq.gz file, e.g. /home/user/data/fastq/SRR12570125_1.fastq.gz]
reads:
- [An unique ID of your sample, e.g SRR12570125]
...
```

Execute the command below to start the main analysis of Vulture.

```shell
cd vulture/nextflow
nextflow run scvh_docker_local.nf -profile dockerlocal -params-file params.yaml --outdir=your_output_directory -with-report nextflow_report_$(date +%s).html -bg &>> nextflow_log_$(date +%s).log
```

A successful run will generate the following files in the output directory:

```shell
...
nextflow_report_1628188800.html
nextflow_log_1628188800.log
...
```
The "nextflow_report_1628188800.html" file is a report of the analysis. The "nextflow_log_1628188800.log" file is the log file of the analysis. A successful run will also generate the following files in the nextflow_log_1628188800.log:

```shell
N E X T F L O W  ~  version 21.10.6
Launching `scvh_docker_local.nf` [cheeky_brown] - revision: 59a6446081
S C V H - N F   P I P E L I N E
===================================
transcriptome: /mnt/d/scvh_files/vmh_genome_dir/references
reads        : [SRR12570125]
outdir       : /mnt/d/output/
database:    : viruSITE.NCBIprokaryotes
threads      : 10 
ram          : 128 
alignment    : STAR 
whitelist    : 3M-february-2018.txt 
soloCBlen    : 16 
soloCBstart  : 1 
soloUMIstart : 17 
soloUMIlen   : 12 
soloStrand   : Forward 
soloMultiMappers: EM 
soloFeature : GeneFull 
outSAMtype   : BAM SortedByCoordinate 
technology   : 10XV3 
pseudoBAM    : 
inputformat    : fastq 
sampleSubfix1    : _1 
sampleSubfix2    : _2 

[SRR12570125, /mnt/d/scvh_files/EXAMPLES/SRR12570125_1.fastq.gz, /mnt/d/scvh_files/EXAMPLES/SRR12570125_2.fastq.gz]
[88/9dd405] Submitted process > Map (1)

```

<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io/docs/3_Nextflow/NextflowInstall.html){: .btn }
[Next Step](https://juychen.github.io/docs/6_Local/Localusage.html){: .btn .btn-purple }
</div>