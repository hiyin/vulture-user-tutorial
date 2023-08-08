---
layout: default
title: Build our own reference genome
nav_order: 1
---



# Build Genome reference

## <a name="require"></a>Requirements
* Docker

Please refer to Section [1.Docker](https://hiyin.github.io/vulture-user-tutorial/1.%20Running%20on%20local%20computer/4_Docker/) or [Docker](https://docs.docker.com/engine/install/ubuntu/) for installation.

Now we are about to run the first step of the Vulture pipeline i.e. mkref (genome reference making), execute the command below in your favourite terminal or powershell and wait it to be finished.


## Download the input data for mkref stage
The input data required for mkref stage are available in the downloadable links below, you can save them into your own S3 bucket folder:

* The human genome : [hg38.fa](https://vulture-reference.s3.ap-east-1.amazonaws.com/humangenome/hg38.fa)

* The human genome annotation : [hg38.unique_gene_names.gtf](https://vulture-reference.s3.ap-east-1.amazonaws.com/humangenome/hg38.unique_gene_names.gtf)

* The list of human-host microbe genome : [prokaryotes.csv](https://vulture-reference.s3.ap-east-1.amazonaws.com/humangenome/prokaryotes.csv)

* The list of human-host virus genome : [viruSITE_human_host.txt](https://vulture-reference.s3.ap-east-1.amazonaws.com/humangenome/viruSITE_human_host.txt)

Alternatavly, you can generate the files yourself following instructions below:

[VirusSITE](http://virusite.org/index.php?nav=search&fields=1&query1=Human&wc1=on&field1=virus.host&search=Search&query2=&wc2=on&field2=virus.name&query3=&wc3=on&field3=virus.name&search_nav=virus&sort=name&order=asc&rows=25&page=1)
(viruSITE human host)
Click "Format: CSV"

[NCBI Prokaryotes](https://www.ncbi.nlm.nih.gov/genome/browse#!/prokaryotes/)
Filters -> Host (Homo sapiens) -> Assembly level (Complete) -> RefSeq category (representative) -> Download [prokaryotes.csv]

Notice that you can edit the of virus or Prokaryotes in viruSITE_human_host.txt and prokaryotes.csv by any text editor to further customize the genome. Then, the folder of your reference input should have the following files:

```shell
#The input path e.g. /home/user/data/refinput should have:
hg38.fa  hg38.unique_gene_names.gtf  prokaryotes.csv  viruSITE_human_host.txt
```

We then edit the mkref profile in the Vulture/nextflow/nextflow.config:

```shell
...
    mkref {
        process.container = 'public.ecr.aws/b6a4h2a6/scvh_mkref:latest'
		docker.enabled = true
        params.ref = '[The full path of you pull your reference genome input, e.g. /home/user/data/refinput]'
        params.humanfa = 'hg38.fa'
        params.humagtf = 'hg38.unique_gene_names.gtf'
        params.viruSITE = 'viruSITE_human_host.txt'
        params.prokaryotes = 'prokaryotes.csv'
		params.outdir = '[The full path of you pull your reference genome output, e.g. /home/user/data/genome]'
    }  
...

```

Afterwards, run the following command to make your own reference genome:

```shell
nextflow run scvh_mkref.nf -profile mkref -with-report mkref_$(date +%s).html -bg &>> mkref_$(date +%s).log;
```

The output files should be in the folder you specified in the nextflow.config file, e.g. /home/user/data/genome. There will be a subfolder named "newref" in the output folder, which contains the following files:

```shell
3M-february-2018.txt
737K-august-2016.txt
human_host_viruses_microbes.viruSITE.NCBIprokaryotes.with_hg38.fa
human_host_viruses_microbes.viruSITE.NCBIprokaryotes.with_hg38.removed_amb_viral_exon.gtf
human_host_viruses_reference_set
```

These files can be the input of the next step of the Vulture pipeline. They are also available in the downloadable links below:

* Hg38 human genome with virus and microbes: [human_host_viruses_microbes.viruSITE.NCBIprokaryotes.with_hg38.fa](https://vulture-reference.s3.ap-east-1.amazonaws.com/human_host_viruses_microbes.viruSITE.NCBIprokaryotes.with_hg38.fa)
* Hg38 human genome with virus: [human_host_viruses.viruSITE.with_hg38.fa](https://vulture-reference.s3.ap-east-1.amazonaws.com/human_host_viruses.viruSITE.with_hg38.fa)
* Hg38 human genome annotaion with virus and microbes: [human_host_viruses_microbes.viruSITE.NCBIprokaryotes.with_hg38.removed_amb_viral_exon.gtf](https://vulture-reference.s3.ap-east-1.amazonaws.com/human_host_viruses_microbes.viruSITE.NCBIprokaryotes.with_hg38.removed_amb_viral_exon.gtf)
* Hg38 human genome annotaion with virus: [human_host_viruses.viruSITE.with_hg38.removed_amb_viral_exon.gtf](https://vulture-reference.s3.ap-east-1.amazonaws.com/human_host_viruses.viruSITE.with_hg38.removed_amb_viral_exon.gtf)
