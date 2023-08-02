---
layout: default
title: Nextflow
parent: Localrun
nav_order: 3
---

# Install the dependencies for the pipeline
PLACE HOLDER
## <a name="require"></a>Requirements
* Input data: 10x Chromium scRNA-seq reads
* DropletUtils >= v1.10.2
* STAR >= v2.7.9a or
* cellranger >= 6.0.0 or
* Kallisto/bustools >= 0.25.1 or
* salmon/alevin >= v1.4.0
* Nextflow >= v21.04.3

The instructions are tested on the following system:

```shell
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.2 LTS
Release:        22.04
Codename:       jammy
```

## Specify the local envrionment in the Nextflow config file

Open the "nextflow.config" file in the vulture/nextflow directory. This following snippet shows how to disable the docker container for the pipeline.

```shell
...
batchlocal {
    docker.enabled = false
}
...

```
## Specify the configuration file for an analysis reading files from the local computer

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
nextflow run scvh_full_local.nf -profile batchlocal -params-file params.yaml --outdir=your_output_directory -with-report nextflow_report_$(date +%s).html -bg &>> nextflow_log_$(date +%s).log
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
...
Completed at: 14-Jul-2023 16:56:53
Duration    : 16m 8s
CPU hours   : 57.6 (1.4% failed)
Succeeded   : 3

```

## Specify the configuration file for an analysis reading files from the SRA database

Alternatively, you can also download the fastq files from the SRA database. Here is a snippet of how the "params.yaml" file looks like:

```shell
alignment: STAR
codebase: [The full path of your Vulture direcory, e.g. /home/user/code/Vulture]
inputformat: fastq
ram: 128
ref: [The full path of your reference genome direcory, e.g. /home/user/data/references]
reads:
- SRR14736914
- SRR14736920
- SRR14736921
- SRR14736923
- SRR14736925
- SRR14736934
- SRR14736927
- SRR14736936
soloFeatures: GeneFull
soloStrand: Reverse
technology: 10XV3
virus_database: viruSITE.NCBIprokaryotes
```

Execute the command below to start the main analysis of Vulture.

```shell
cd vulture/nextflow
nextflow run scvh_full_local.nf -profile batchlocal -params-file params.yaml --outdir=your_output_directory -with-report nextflow_report_$(date +%s).html -bg &>> nextflow_log_$(date +%s).log
```

The analysis will launch the SRA-tools and dump fastq files from the SRA database and start the analysis. A successful run will generate the following files in the output directory:

```shell
N E X T F L O W  ~  version 21.10.6
Launching `scvh_full_local.nf` [backstabbing_austin] - revision: a459ae3e2c
S C V H - N F   P I P E L I N E
===================================
transcriptome: [The full path of your Vulture direcory, e.g. /home/user/code/Vulture]
reads        : [SRR14736914, SRR14736920, SRR14736921, SRR14736923, SRR14736925, SRR14736934, SRR14736927, SRR14736936]
outdir       : [The full path of your Vulture direcory, e.g. /home/user/output/Vulture]
database:    : viruSITE.NCBIprokaryotes
threads      : 10 
ram          : 128 
alignment    : STAR 
whitelist    : 3M-february-2018.txt 
soloCBlen    : 16 
soloCBstart  : 1 
soloUMIstart : 17 
soloUMIlen   : 12 
soloStrand   : Reverse 
soloMultiMappers: EM 
soloFeature : GeneFull 
outSAMtype   : BAM SortedByCoordinate 
technology   : 10XV3 
pseudoBAM    : 
inputformat    : fastq 
sampleSubfix1    : _1 
sampleSubfix2    : _2 

SRR14736914
SRR14736920
SRR14736921
SRR14736923
SRR14736925
SRR14736934
SRR14736927
SRR14736936
executor >  local (1)
[2f/894239] process > Dump (6) [  0%] 0 of 8
[-        ] process > Map      -
[-        ] process > Filter   -
[-        ] process > Analysis -

executor >  local (1)
[2f/894239] process > Dump (6) [  0%] 0 of 8
[-        ] process > Map      -
[-        ] process > Filter   -
[-        ] process > Analysis -

executor >  local (2)
[f6/245efd] process > Dump (7) [  0%] 0 of 8
[-        ] process > Map      -
[-        ] process > Filter   -
[-        ] process > Analysis -

executor >  local (2)
[2f/894239] process > Dump (6) [ 12%] 1 of 8
[-        ] process > Map      -
[-        ] process > Filter   -
[-        ] process > Analysis -

executor >  local (2)
[2f/894239] process > Dump (6) [ 12%] 1 of 8
[-        ] process > Map      -
[-        ] process > Filter   -
[-        ] process > Analysis -

executor >  local (2)
[2f/894239] process > Dump (6) [ 12%] 1 of 8
[-        ] process > Map      [  0%] 0 of 1
[-        ] process > Filter   -
[-        ] process > Analysis -

....

Completed at: 14-Jul-2023 16:56:53
Duration    : 3h 36m 8s
CPU hours   : 57.6 (1.4% failed)
Succeeded   : 8
Ignored     : 8
Failed      : 8
```

<div class="code-example" markdown="1">
[Previous Step](https://juychen.github.io/docs/3_Nextflow/NextflowInstall.html){: .btn }
[Next Step](https://juychen.github.io/docs/6_Local/Localusage.html){: .btn .btn-purple }
</div>
