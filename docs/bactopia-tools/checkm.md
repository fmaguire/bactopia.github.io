---
title: checkm
description: A Bactopia Tool which uses CheckM to assess the quality of microbial genomes recovered from isolates.
---
# Bactopia Tool - `checkm`
The `checkm` module is used [CheckM](https://github.com/Ecogenomics/CheckM) to assess the quality of microbial
genomes recovered from isolates, single cells, and metagenomes.


## Example Usage
```
bactopia --wf checkm \
  --bactopia /path/to/your/bactopia/results \ 
  --include includes.txt  
```

## Output Overview

Below is the default output structure for the `checkm` tool. Where possible the 
file descriptions below were modified from a tools description.

```{bash}
checkm/
├── <SAMPLE_NAME>
│   ├── bins/
│   ├── logs
│   │   └── checkm
│   │       ├── nf-checkm.{begin,err,log,out,run,sh,trace}
│   │       ├── checkm.log
│   │       └── versions.yml
│   ├── storage/
│   ├── lineage.ms
│   ├── <SAMPLE_NAME>-genes.aln
│   └── <SAMPLE_NAME>-results.txt
├── checkm.tsv
├── logs
│   ├── csvtk_concat
│   │   ├── nf-csvtk_concat.{begin,err,log,out,run,sh,trace}
│   │   └── versions.yml
│   └── custom_dumpsoftwareversions
│       ├── nf-custom_dumpsoftwareversions.{begin,err,log,out,run,sh,trace}
│       └── versions.yml
├── nf-reports
│   ├── checkm-dag.dot
│   ├── checkm-report.html
│   ├── checkm-timeline.html
│   └── checkm-trace.txt
├── software_versions.yml
└── software_versions_mqc.yml

```



### Results

#### CheckM

Below is a description of the _per-sample_ results from [CheckM](https://github.com/Ecogenomics/CheckM).


| Filename | Description |
|----------|-------------|
| bins/ | A folder with inputs (e.g. proteins) for processing by `CheckM`  |
| storage/ | A folder with intermediate results from `CheckM` processing |
| lineage.ms | Output file describing marker set for each bin |
| &lt;SAMPLE_NAME&gt;-genes.aln | Alignment of multi-copy genes and their AAI identity |
| &lt;SAMPLE_NAME&gt;-results.txt | Final results of [CheckM's lineage_wf](https://github.com/Ecogenomics/CheckM/wiki/Workflows#lineage-specific-workflow) |





### Audit Trail

Below are files that can assist you in understanding which parameters and program versions were used.

#### Logs 

Each process that is executed will have a `logs` folder containing helpful files for you to review
if the need ever arises.

| Filename                      | Description |
|-------------------------------|-------------|
| nf-&lt;PROCESS_NAME&gt;.begin | An empty file used to designate the process started |
| nf-&lt;PROCESS_NAME&gt;.err   | Contains STDERR outputs from the process |
| nf-&lt;PROCESS_NAME&gt;.log   | Contains both STDERR and STDOUT outputs from the process |
| nf-&lt;PROCESS_NAME&gt;.out   | Contains STDOUT outputs from the process |
| nf-&lt;PROCESS_NAME&gt;.run   | The script Nextflow uses to stage/unstage files and queue processes based on given profile |
| nf-&lt;PROCESS_NAME&gt;.sh    | The script executed by bash for the process  |
| nf-&lt;PROCESS_NAME&gt;.trace | The Nextflow [Trace](https://www.nextflow.io/docs/latest/tracing.html#trace-report) report for the process |
| versions.yml                  | A YAML formatted file with program versions |

#### Nextflow Reports

These Nextflow reports provide great a great summary of your run. These can be used to optimize
resource usage and estimate expected costs if using cloud platforms.

| Filename | Description |
|----------|-------------|
| checkm-dag.dot | The Nextflow [DAG visualisation](https://www.nextflow.io/docs/latest/tracing.html#dag-visualisation) |
| checkm-report.html | The Nextflow [Execution Report](https://www.nextflow.io/docs/latest/tracing.html#execution-report) |
| checkm-timeline.html | The Nextflow [Timeline Report](https://www.nextflow.io/docs/latest/tracing.html#timeline-report) |
| checkm-trace.txt | The Nextflow [Trace](https://www.nextflow.io/docs/latest/tracing.html#trace-report) report |


#### Program Versions

At the end of each run, each of the `versions.yml` files are merged into the files below.

| Filename                  | Description |
|---------------------------|-------------|
| software_versions.yml     | A complete list of programs and versions used by each process | 
| software_versions_mqc.yml | A complete list of programs and versions formatted for [MultiQC](https://multiqc.info/) |

## Parameters


### Required Parameters
Define where the pipeline should find input data and save output data.

| Parameter | Description | Default |
|---|---|---|
| `--bactopia` | The path to bactopia results to use as inputs |  |

### Filtering Parameters
Use these parameters to specify which samples to include or exclude.

| Parameter | Description | Default |
|---|---|---|
| `--include` | A text file containing sample names (one per line) to include from the analysis |  |
| `--exclude` | A text file containing sample names (one per line) to exclude from the analysis |  |


### CheckM Parameters


| Parameter | Description | Default |
|---|---|---|
| `--checkm_unique` | Minimum number of unique phylogenetic markers required to use lineage-specific marker set. | 10 |
| `--checkm_multi` | Maximum number of multi-copy phylogenetic markers before defaulting to domain-level marker set. | 10 |
| `--aai_strain` | AAI threshold used to identify strain heterogeneity | 0.9 |
| `--checkm_length` | Percent overlap between target and query | 0.7 |
| `--full_tree` | Use the full tree (requires ~40GB of memory) for determining lineage of each bin. |  |
| `--skip_pseudogene_correction` | Skip identification and filtering of pseudogene |  |
| `--ignore_thresholds` | Ignore model-specific score thresholds |  |
| `--checkm_ali` | Generate HMMER alignment file for each bin |  |
| `--checkm_nt` | Generate nucleotide gene sequences for each bin |  |
| `--force_domain` | Use domain-level sets for all bins |  |
| `--no_refinement` | Do not perform lineage-specific marker set refinement |  |
| `--individual_markers` | Treat marker as independent |  |
| `--skip_adj_correction` | Do not exclude adjacent marker genes when estimating contamination |  |


### Optional Parameters
These optional parameters can be useful in certain settings.

| Parameter | Description | Default |
|---|---|---|
| `--outdir` | Base directory to write results to | ./ |
| `--run_name` | Name of the directory to hold results | bactopia |
| `--skip_compression` | Ouput files will not be compressed | False |
| `--keep_all_files` | Keeps all analysis files created | False |

### Max Job Request Parameters
Set the top limit for requested resources for any single job.

| Parameter | Description | Default |
|---|---|---|
| `--max_retry` | Maximum times to retry a process before allowing it to fail. | 3 |
| `--max_cpus` | Maximum number of CPUs that can be requested for any single job. | 4 |
| `--max_memory` | Maximum amount of memory (in GB) that can be requested for any single job. | 32 |
| `--max_time` | Maximum amount of time (in minutes) that can be requested for any single job. | 120 |
| `--max_downloads` | Maximum number of samples to download at a time | 3 |

### Nextflow Configuration Parameters
Parameters to fine-tune your Nextflow setup.

| Parameter | Description | Default |
|---|---|---|
| `--nfconfig` | A Nextflow compatible config file for custom profiles, loaded last and will overwrite existing variables if set. |  |
| `--publish_dir_mode` | Method used to save pipeline results to output directory. | copy |
| `--infodir` | Directory to keep pipeline Nextflow logs and reports. | ${params.outdir}/pipeline_info |
| `--force` | Nextflow will overwrite existing output files. | False |
| `--cleanup_workdir` | After Bactopia is successfully executed, the `work` directory will be deleted. | False |

### Nextflow Profile Parameters
Parameters to fine-tune your Nextflow setup.

| Parameter | Description | Default |
|---|---|---|
| `--condadir` | Directory to Nextflow should use for Conda environments |  |
| `--registry` | Docker registry to pull containers from. | dockerhub |
| `--singularity_cache` | Directory where remote Singularity images are stored. |  |
| `--singularity_pull_docker_container` | Instead of directly downloading Singularity images for use with Singularity, force the workflow to pull and convert Docker containers instead. |  |
| `--force_rebuild` | Force overwrite of existing pre-built environments. | False |
| `--queue` | Comma-separated name of the queue(s) to be used by a job scheduler (e.g. AWS Batch or SLURM) | general,high-memory |
| `--cluster_opts` | Additional options to pass to the executor. (e.g. SLURM: '--account=my_acct_name' |  |
| `--disable_scratch` | All intermediate files created on worker nodes of will be transferred to the head node. | False |

### Helpful Parameters
Uncommonly used parameters that might be useful.

| Parameter | Description | Default |
|---|---|---|
| `--monochrome_logs` | Do not use coloured log outputs. |  |
| `--nfdir` | Print directory Nextflow has pulled Bactopia to |  |
| `--sleep_time` | The amount of time (seconds) Nextflow will wait after setting up datasets before execution. | 5 |
| `--validate_params` | Boolean whether to validate parameters against the schema at runtime | True |
| `--help` | Display help text. |  |
| `--wf` | Specify which workflow or Bactopia Tool to execute | bactopia |
| `--list_wfs` | List the available workflows and Bactopia Tools to use with '--wf' |  |
| `--show_hidden_params` | Show all params when using `--help` |  |
| `--help_all` | An alias for --help --show_hidden_params |  |
| `--version` | Display version text. |  |

## Citations
If you use Bactopia and `checkm` in your analysis, please cite the following.

- [Bactopia](https://bactopia.github.io/)  
    Petit III RA, Read TD [Bactopia - a flexible pipeline for complete analysis of bacterial genomes.](https://doi.org/10.1128/mSystems.00190-20) _mSystems_ 5 (2020)
  

- [CheckM](https://github.com/Ecogenomics/CheckM)  
    Parks DH, Imelfort M, Skennerton CT, Hugenholtz P, Tyson GW [CheckM: assessing the quality of microbial genomes recovered from isolates, single cells, and metagenomes.](http://dx.doi.org/10.1101/gr.186072.114) _Genome Res_ 25, 1043–1055 (2015)
  