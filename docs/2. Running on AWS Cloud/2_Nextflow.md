---
layout: default
title: Nextflow
parent: Running Vulture on AWS cloud with Nextflow
nav_order: 2
---
# Overview

# Nextflow

[Nextflow](https://www.nextflow.io/about-us.html) is free open-source software distributed under the Apache 2.0 license developed by Seqera Labs. The software is used by scientists and engineers to write, deploy and share data-intensive, highly scalable, workflows on any infrastructure.

It helps us to define workflows to run the Vulture pipeline composed of multiple programs. The reason we build Vulture within Nextflow is that it can be applied to workflows in containers or on a cloud computing platform.

You can refer to the [Nextflow documentation](https://www.nextflow.io/docs/latest/getstarted.html) for more information about Nextflow syntax and usage.

# Nextflow
This section introduces the installation of Nextflow and its dependencies.

## <a name="installnf"></a>Install Nextflow

Nextflow can be installed by following the instructions at [Nextflow Installation Guide](https://www.nextflow.io/docs/latest/getstarted.html). If you are using OS X/Linux, you can run the commands below to install it immediately.

```shell
wget -qO- https://get.nextflow.io | bash
# run from current directory
chmod +x nextflow
# run from anywhere
sudo mv nextflow /usr/local/bin/
```

## <a name="installnfdep"></a>Install Nextflow dependencies for visualization

Nextflow is able to render graphs for which it needs graphviz to be installed. jq will help us deal with JSON files.

```shell
sudo yum install -y graphviz jq
```

## <a name="awsregion"></a>AWS Region


Even though we are depending on an IAM Role and not local permissions some tools depend on having the AWS_REGION defined as an environment variable - let's add it to our login shell configuration.


```shell
export AWS_REGION=us-east-2
echo "AWS_REGION=${AWS_REGION}" |tee -a ~/.bashrc
```