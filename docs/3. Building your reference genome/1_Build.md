---
layout: default
title: Build our own reference genome
nav_order: 1
---



# Build Genome reference

## <a name="require"></a>Requirements
* Docker

Please refer to Section 1.4 or [Docker](https://docs.docker.com/engine/install/ubuntu/) for installation.

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

After the mkref job is done, you need to edit line in "nextflow/nextflow.config" file -> "params.ref" to the actual S3 path where your output reference genome files are i.e. in "s3://${BUCKET_NAME_RESULTS}/batchA" or you could download from the downloadable links below and store them into your own S3 bucket folder:
[human_host_viruses_microbes.viruSITE.NCBIprokaryotes.with_hg38.removed_amb_viral_exon.gtf](https://vulture-reference.s3.ap-east-1.amazonaws.com/human_host_viruses_microbes.viruSITE.NCBIprokaryotes.with_hg38.removed_amb_viral_exon.gtf)
[human_host_viruses_microbes.viruSITE.NCBIprokaryotes.with_hg38.fa](https://vulture-reference.s3.ap-east-1.amazonaws.com/human_host_viruses_microbes.viruSITE.NCBIprokaryotes.with_hg38.fa)
[human_host_viruses.viruSITE.with_hg38.removed_amb_viral_exon.gtf](https://vulture-reference.s3.ap-east-1.amazonaws.com/human_host_viruses.viruSITE.with_hg38.removed_amb_viral_exon.gtf)
[human_host_viruses.viruSITE.with_hg38.fa](https://vulture-reference.s3.ap-east-1.amazonaws.com/human_host_viruses.viruSITE.with_hg38.fa)

```shell
...
mkref {
process.container = 'public.ecr.aws/b6a4h2a6/scvh_mkref:latest'
params.ref = '$HOME/humangenome/'
}
...

```

```shell
nextflow run scvh_mkref.nf -profile mkref --outdir=s3://${BUCKET_NAME_RESULTS}/batchA -with-report mkref_$(date +%s).html -bg &>> mkref_$(date +%s).log;
```