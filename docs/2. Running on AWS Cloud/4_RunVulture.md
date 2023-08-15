---
layout: default
title: Running Vulture on AWS cloud with Nextflow
has_children: false
nav_order: 1
---

# Nextflow and scRNA-Seq processing

---
## Setup the AWS CLI

Install the AWS CLI and prepare the access key and secret access key ahead by following instructions at [Obtaining AWS Credentials](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-appendix-sign-up.html).

```shell
pip install awscli
aws configure
# here will prompt you to enter access key and secret access key
```

## Clone Vulture source code

Clone the source code of Vulture into your local computer

```shell
git clone https://github.com/holab-hku/Vulture.git
# change into directory below
cd Vulture/nextflow
```

## Create S3 Bucket to store results

```shell
# specify bucket names and save them into bash environment variables
export BUCKET_NAME_TEMP=vulture-temp
export BUCKET_NAME_RESULTS=vulture-results
echo "BUCKET_NAME_TEMP=${BUCKET_NAME_TEMP}" |tee -a ~/.bashrc
echo "BUCKET_NAME_RESULTS=${BUCKET_NAME_RESULTS}" |tee -a ~/.bashrc
# create S3 buckets with specified bucket names 
aws --region ${AWS_REGION} s3 mb s3://${BUCKET_NAME_TEMP}
aws --region ${AWS_REGION} s3 mb s3://${BUCKET_NAME_RESULTS}
```

## Run Vulture pipeline - 1. Build Genome reference

Now we are about to run the first step of the Vulture pipeline i.e. mkref (genome reference making), execute the command below in your favourite terminal or powershell and wait it to be finished

```shell
nextflow run scvh_mkref.nf -profile mkref -bucket-dir s3://${BUCKET_NAME_TEMP} --outdir=s3://${BUCKET_NAME_RESULTS}/batchA -with-report mkref_$(date +%s).html -bg &>> mkref_$(date +%s).log;
```

The input data required for mkref stage are available in the downloadable links below, you can save them into your own S3 bucket folder:

[hg38.fa](https://vulture-reference.s3.ap-east-1.amazonaws.com/humangenome/hg38.fa)
[hg38.unique_gene_names.gtf](https://vulture-reference.s3.ap-east-1.amazonaws.com/humangenome/hg38.unique_gene_names.gtf)
[prokaryotes.csv](https://vulture-reference.s3.ap-east-1.amazonaws.com/humangenome/prokaryotes.csv)
[viruSITE_human_host.txt](https://vulture-reference.s3.ap-east-1.amazonaws.com/humangenome/viruSITE_human_host.txt)

Also you can generate the files yourself following instructions below:

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
aws.region = 'us-east-2'
process.container = 'public.ecr.aws/b6a4h2a6/scvh_mkref:latest'
process.executor = 'awsbatch'
process.queue = 'vulture-stdq'
# this line needs to be changed
params.ref = 's3://vulture-reference/humangenome/'
}
...

```

## Run Vulture pipeline - 2. Start main analysis

Before we start our analysis, we need to edit "nextflow/params.yaml" file to include the reads of your interest. Here is a snippet of how the "params.yaml" file looks like

```shell
...
soloStrand: "Forward"
alignment: "STAR"
technology: "10XV2"
virus_database: "viruSITE"
soloMultiMappers: "EM"
soloFeatures: "GeneFull"
inputformat: "bam"
soloInputSAMattrBarcodeSeq: "CB UB"
barcodes_whitelist: "None"
reads: 
- "SRR6885502"
- "SRR6885503"
- "SRR6885504"
- "SRR6885505"
- "SRR6885506"
- "SRR6885507"
- "SRR6885508"
...
```

Execute the command below to start the main analysis of Vulture.

```shell
nextflow run scvh_full.nf -profile batchfull -params-file params.yaml -bucket-dir s3://${BUCKET_NAME_TEMP} --outdir=s3://${BUCKET_NAME_RESULTS}/batchD -with-report report_bam_$(date +%s).html -bg &>> submitnf_bam_$(date +%s).log
```
