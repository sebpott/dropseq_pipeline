## dropseq_pipeline

This Drop-seq pipeline is designed to process data from fastq files to a digital expression matrix (dge).
The pipeline is based on Snakemake and is currently designed to run on the uchicago rcc cluster **midway2**. Below is a description of how to set up the project folders and to start the analysis.
Each experiment differs and the pipeline might need to be adjusted to accommodate such individual differences.

Below are steps that are common to all experiments and outlines of analysis that are commonly used.




## Setting up
#### 1. Start interactive session to set up the folders, data, and the dropseq pipeline
```bash
#log into midway2
ssh CnetID@midway2.rcc.uchicago.edu

#on midway2 start an interactive session
sinteractive  --partition=broadwl
```
#### 2. Create a compute environment using conda (This step can be skipped when re-running an analysis)

The environment needs to be created only once. It will be activated when running the dropseq pipeline.
```bash
module load Anaconda3

conda env create --file /project2/gilad/spott/Pipelines/dropseq_pipeline/environment.yaml
```

To update the environment, you can run the following command:
```bash
conda env update --file /project2/gilad/spott/Pipelines/dropseq_pipeline/environment.yaml
```

## Prepare data:
#### 1. Create a project directory in your directory on midway2
```bash
mkdir your_project
```
#### 2. Create directory fastq in 'your_project' directory and add fastq files
```bash
cd your_project

mkdir data
cd data/

mkdir fastq
cd fastq/
#only include the fastq files included in a single run, both read 1 and read2
cp path/to/fastq/*fastq.gz .

cd ../../
```



## Run dropseq pipeline

Creation of the annotation files used in the analyses below is described in **Preparation_of_annotation_data.md**, which can be found in this repo as well.

**Important:** this pipeline will fail in the event that the  number of cells in this sample could not be determined automatically. If this is the case re-run the command below and substitute with your best guess of the cell number. This depends on what you expected from the experiment. Also check out the plots *Whitelist_stats_cell_barcode_knee.png*, *Whitelist_stats_cell_barcode_count_density.png*, and *Whitelist_stats_cell_barcode_counts.png* in your project directory to help you choose your threshold.


### Option 1: Human samples, hg38

This command will run the Submit_snakemake.sh and pass the location of the Snakefile and the config file (for hg38).

```bash
/project2/gilad/spott/Pipelines/dropseq_pipeline/snakemake.batch "-s /project2/gilad/spott/Pipelines/dropseq_pipeline/Snakefile" \
 "--configfile /project2/gilad/spott/Pipelines/DropSeq/config_hg38.yaml" "--config proj_dir=/project2/PI/CNETID/Path/to/your/dir/"
```




```bash
#replace 5000 in cell_num with your desired cell number
/project2/gilad/spott/Pipelines/dropseq_pipeline/snakemake.batch "-s /project2/gilad/spott/Pipelines/dropseq_pipeline/Snakefile_fixed" \
"--configfile /project2/gilad/spott/Pipelines/dropseq_pipeline/config_hg38.yaml" \
"--config cell_num=5000" "--config proj_dir=/project2/PI/CNETID/Path/to/your/dir/"
```

### Option 2: mouse, mm10
This command will run the Submit_snakemake.sh and pass the location of the Snakefile and the config file (for mm10).

```bash
/project2/gilad/spott/Pipelines/dropseq_pipeline/snakemake.batch "-s /project2/gilad/spott/Pipelines/dropseq_pipeline/Snakefile" \
 "--configfile /project2/gilad/spott/Pipelines/DropSeq/config_mm10.yaml" "--config proj_dir=/project2/PI/CNETID/Path/to/your/dir/"
```




```bash
#replace 5000 in cell_num with your desired cell number
/project2/gilad/spott/Pipelines/dropseq_pipeline/snakemake.batch "-s /project2/gilad/spott/Pipelines/dropseq_pipeline/Snakefile_fixed" \
"--configfile /project2/gilad/spott/Pipelines/dropseq_pipeline/config_mm10.yaml" \
"--config cell_num=5000" "--config proj_dir=/project2/PI/CNETID/Path/to/your/dir/"
```


### Option 3: Human + Chimp mixture, hg38, panTro5





### Option 4: Zebrafish samples, danRer10

This analysis is geared towards Zebrafish cells that express both a copy of mouse c-myc and the fluorochromes EGFP or mCherry.

The sequence and transcript information for these genes has been added to the reference and STAR index. It should be fine to use these files if you use zebrafish in your experiments but do not express transgenes.



```bash

/project2/gilad/spott/Pipelines/dropseq_pipeline/snakemake.batch "-s /project2/gilad/spott/Pipelines/dropseq_pipeline/Snakefile" \
 "--configfile /project2/gilad/spott/Pipelines/DropSeq/config_zebrafish.yaml" "--config proj_dir=/project2/PI/CNETID/Path/to/your/dir/"


```


```bash
#replace 5000 in cell_num with your desired cell number
/project2/gilad/spott/Pipelines/dropseq_pipeline/snakemake.batch "-s /project2/gilad/spott/Pipelines/dropseq_pipeline/Snakefile_fixed" "--configfile /project2/gilad/spott/Pipelines/dropseq_pipeline/config_zebrafish.yaml" "--config cell_num=5000" "--config proj_dir=/project2/PI/CNETID/Path/to/your/dir/"
```
