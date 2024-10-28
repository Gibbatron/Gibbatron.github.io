---
title: "RNAseq Data Processing & Analysing Course"
author: "Alex Gibbs"
#full-width: true
---

# Day 3 - Performing differential gene expression with nf-core/differentialabundance pipeline

---

## Recap from Day 2
---
---

- Yesterday we covered:
  - Outputs from Day 1 session (fetchngs pipeline)
  - Inputs needed to run the rnaseq pipeline
  - How to execute the rnaseq pipeline

---

<br>

---

## Outputs from the rnaseq pipeline
---
---

- We should all have had an email from *SCW HAWK - HPC SERVICES* notifying you of a successful pipeline run.
- Sometimes the email function doesn't work. We can just log in and check ourselves.
- To check, we need to log back onto HAWK, load the tmux module, and then open the session that we created.
- NOTE: When using tmux, we need to make sure we are logged into the correct node. When we covered the log ins yesterday, we logged onto the cl1 node. The tmux session we made will only be present on the cl1 node. If we were to log in to the cl2 node, we wouldnt be able to find the tmux session.

<br>

```
#log onto HAWK
c.c1234567@hawklogin01.cf.ac.uk
PASSWORD

#move to the working directory (scratch)
cd /scratch/c.c134567/rnaseq

#load tmux
module load tmux

#open our tmux session
tmux attach -t rnaseq
```

<br>

- The process took almost 6 hours. As all of us were using the pipeline at the same time, these times will likely vary. Download times will also depend on the size and number of files being downloaded.
- Some of you may have yours still running. Please let me know if this is the case and we can get you access to the outputs so you can continue.

### Population of the output directory

- All of the outputs from the pipeline will have been placed into the output directory.
- We should have the following: **fastqc, multiqc, pipeline_info, sortmerna, star_salmon, trimgalore**.

#### pipeline_info
- This directory contains all the files relevant to the pipeline that was run, such as a nextflow report, pipeline report.
- These files can be used to make a report if needed (handy when writing methods section).
- We don’t need any of these files for the next stages.

#### sortmerna
- This directory contains the log files for each sample during the ribosomal RNA removal step. 
- The log files contain information about the reads that matched the reference database(s).
- We do not need anything from this directory for the next stages.

#### trimgalore
- This directory contains the log files for each sample during the trimgalore step.
- Trimgalore tool is used to trim the adapter sequences from each sequencing file.
- The log files contain information about what sequencing adapters were removed from the samples etc.
- We don't need any of these files for the next stages.

#### fastqc and multiqc
- One thing we need to check is the quality of the sequencing reads.
- The way we do this is to perform FastQC on each sample, which produces a report for each fastq file.
- As you can imagine, this can be a painstaking process if we were to check every file one by one.
- This is where MultiQC comes in handy - it produces one report for all of the FastQC results.
- It also takes the data from the other processing steps and feeds it into the report, giving us an overall rnaseq pipeline sequencing statistics report.
- The easiest way to read the report is to transfer the webpage report from HAWK to our PCs.

<details>
<summary>Transfer MultiQC report to your PC</summary>

<br>

- The easiest way to do this is to transfer using Filezilla or MobaXterm.
- The MultiQC webpage report is located in `/scratch/c.c1234567/rnaseq/output/multiqc/star_salmon/'
- You will need to transfer the `multiqc_report.html' file.

- You can also transfer the file by using the following code in a unix shell onm your pc.

<pre><span style="color:crimson;">
#move to a directory where you want to download the file to. I reccommend Downloads
cd Downloads

#transfer file from HAWK to the current directory. Make sure you include the dot (.)!
scp c.c1234567@hawklogin.cf.ac.uk:/scratch/c.c1234567/rnaseq/output/multiqc/star_salmon/multiqc_report.html .
</span></pre>

</details>

**Assessing the MultiQC report**
- There is a great video that covers the outputs of the MultiQC report [here](https://www.youtube.com/watch?v=qPbIlO_KWN0).
- I won't cover the whole report as there is too much to cover in one session. 
- I will cover what I believe to be the most important parts, but please do have a good look through the whole report if you are interested.

<br>

<details>
<summary>STAR_SALMON DESeq2 sample similarity</summary>

- This section provides the user with a heatmap of sample-sample distances.
- It allows us to quickly check the similarity, and differences, between our samples.
- As we have the same condition between two different cell lines, we should expect there to be an obvious difference between them.
- As you can see, there is a high degree of similarity within the groups (as depicted by the red sections), and a low degree of similarity between the groups (as depicted by blue sections).
- If we were to analyse the whole dataset, which consists of 2 cell lines that have hypoxic and normoxic conditions (12 samples total, 6 samples per cell line, 3 samples per condition), we would expect a clear difference in similarities between the two cell lines, with only a slight difference in similarities between the conditions in the same cell line.
- We can use this plot to also quickly see if there are any potential outliers in our data.

<b>INSERT FIGURE</b>

</details>

<br>

<details>
<summary>STAR_SALMON DESeq2 PCA Plot</summary>

- This Principal Component Analysis (PCA) plot is used to show us how similar or different each sample is to each other.
- PCA analysis is a method of simplifying multi-dimensional data.
- The analysis takes the gene expression data for each sample, which is a data table containing thousands of expression values for each gene, and represents this as one data point for each principle component.
- This data point represents the most variation in the dataset for the given principle component. The algorithm is then adjusted and reiterated to give another set of data points for the next principle component, and so on.
- This plot shows us the data points for principle component 1 vs principle component 2 (PC1 vs PC2).
- Don't worry if you don't quite get how the analysis works, [here](https://www.youtube.com/watch?v=FgakZw6K1QQ) is a great video that nicely covers the method.
- All you need to know is that the closer the points are, the more similar the samples are. The more dispersed/further away the points are, the less similar the samples are.
- Here we can see that we have a nice cluster of points for one condition along both axes, indicating that we have a high degree of similarity between these 3 samples.
- We can also see on the left hand side, that for at least the PC1 axis, we have a hight degree of similarity for the other 3 samples. Not so much when we look at the PC2 axis.
- As we are only analysing the 2 groups and can see that there is a clear separation between them, this is as far as we need to go for this analysis.
- We will keep in mind, though, that we may have some outliers for the other sample group as they are more dispersed from each other.

<b>INSERT FIGURE</b>

</details>

<br>

<details>
<summary>QualiMap</summary>

**Genomic origin of reads**
- This tools is useful for giving us information about the mapped reads.
- As we have RNAseq samples, we would expect to get a lot of reads mapping to the exonic regions.
- In a good experiment, we would expect to get >80% of reads mapping to the exonic regions.
- I would say <60% reads mapping to the exonic regions would be a cause for concern.

</details>

<br>

<details>
<summary>FastQC Raw & Trimmed</summary>

- The pipeline performs FastQC on the raw reads and then on the trimmed reads.
- It is good practice to check both raw and trimmed fastQC results to ensure sample quality is improved or consistent after trimming.

**Sequence Quality Histograms**
- This is the most handy output from the report.
- The tool generates a quality score for each sample and reports it as a histogram.
- What we are looking for is high quality scores across the length of the sequences.
- Here we can see that we have a mean score of 30 for all samples for both raw and trimmed.

<b>INSERT FIGURE</b>

**Per Sequence GC Content**
- This plot shows us the average GC content of reads for each sample.
- We expect this value to be between 40% and 60% in humans and 40% and 50% in mice.
- Here we can see our GC content falls around 50% in this dataset for both raw and trimmed reads.

<b>INSERT FIGURE</b>

</details>

#### star_salmon

- This directory contains all of the main outputs for the pipeline.
- Here we will find the processed sample files as well as various other files such as files that contain the stats outputs from the various tools used during the pipeline. These files are used to generate the multiqc report that we just covered.
- I won't go over each of these outputs here, but a full description of them can be found on the output section of the [pipelines webpage](https://nf-co.re/rnaseq/3.14.0/docs/output/).
- For those who don't plan on running the differentialabundance pipeline (completely fine and up to you!), the files you may need to run your downstream differential analyses are:
  - ```SAMPLE/quant.genes.sf``` : Salmon gene-level quantification of the sample, including feature length, effective length, TPM, and number of reads.
  - ```SAMPLE/quant.sf```: Salmon transcript-level quantification of the sample, including feature length, effective length, TPM, and number of reads.

---

## nf-core/differentialabundance pipeline
---
---

Required files:

**resources/diff-abundance-samplesheet.csv**
- We can use the samplesheet that we used for the rnaseq pipeline, but we need to make a few edits.
- We need to add an extra column (or more depending on your comparisons).
- The column format is as follows to compare between the two groups:

sample|fastq_1|fastq_2|strandedness|conditionOne
|-----|-------|-------|------------|------------|
sampleA_1|path/to/file|path/to/file|auto|GroupA
sampleA_2|path/to/file|path/to/file|auto|GroupA
sampleA_3|path/to/file|path/to/file|auto|GroupA
sampleB_1|path/to/file|path/to/file|auto|GroupB
sampleB_2|path/to/file|path/to/file|auto|GroupB
sampleB_3|path/to/file|path/to/file|auto|GroupB

- In order to run this pipeline, we need to provide it with a condition column. As we are only using two groups, one condition column will suffice.
- However, The whole dataset has 2 cell lines with 2 conditions. If we wanted to use the whole dataset and run different comparisons, we would need to add extra columns.


<details>
<summary>See here how to add extra columns to perform extra comparisons</summary>

<br>

- If we wanted to compare firstly between the two cell lines regardless of conditions, we would use the `conditionOne` column and compare GroupA vs GroupB.
- If we wanted to compare between the conditions, irrespective of the cell line, we would add a `conditionTwo` column and compare ConditionA vs ConditionB:

sample|fastq_1|fastq_2|strandedness|conditionOne|conditionTwo
|-----|-------|-------|------------|------------|-----------|
sampleA_1|path/to/file|path/to/file|auto|GroupA|ConditionA
sampleA_2|path/to/file|path/to/file|auto|GroupA|ConditionA
sampleA_3|path/to/file|path/to/file|auto|GroupA|ConditionA
sampleA_4|path/to/file|path/to/file|auto|GroupA|ConditionB
sampleA_5|path/to/file|path/to/file|auto|GroupA|ConditionB
sampleA_6|path/to/file|path/to/file|auto|GroupA|ConditionB
sampleB_1|path/to/file|path/to/file|auto|GroupB|ConditionA
sampleB_2|path/to/file|path/to/file|auto|GroupB|ConditionA
sampleB_3|path/to/file|path/to/file|auto|GroupB|ConditionA
sampleB_4|path/to/file|path/to/file|auto|GroupB|ConditionB
sampleB_5|path/to/file|path/to/file|auto|GroupB|ConditionB
sampleB_6|path/to/file|path/to/file|auto|GroupB|ConditionB

- If we wanted to comapre the two conditions within the same samples, we would add a `conditionThree` column and compare GrpAConA vs GrpAConB and GrpBConA vs GrpBConB:

sample|fastq_1|fastq_2|strandedness|conditionOne|conditionTwo|conditionThree
|-----|-------|-------|------------|------------|------------|-------------|
sampleA_1|path/to/file|path/to/file|auto|GroupA|ConditionA|GrpAConA
sampleA_2|path/to/file|path/to/file|auto|GroupA|ConditionA|GrpAConA
sampleA_3|path/to/file|path/to/file|auto|GroupA|ConditionA|GrpAConA
sampleA_4|path/to/file|path/to/file|auto|GroupA|ConditionB|GrpAConB
sampleA_5|path/to/file|path/to/file|auto|GroupA|ConditionB|GrpAConB
sampleA_6|path/to/file|path/to/file|auto|GroupA|ConditionB|GrpAConB
sampleB_1|path/to/file|path/to/file|auto|GroupB|ConditionA|GrpBConA
sampleB_2|path/to/file|path/to/file|auto|GroupB|ConditionA|GrpBConA
sampleB_3|path/to/file|path/to/file|auto|GroupB|ConditionA|GrpBConA
sampleB_4|path/to/file|path/to/file|auto|GroupB|ConditionB|GrpBConB
sampleB_5|path/to/file|path/to/file|auto|GroupB|ConditionB|GrpBConB
sampleB_6|path/to/file|path/to/file|auto|GroupB|ConditionB|GrpBConB

- Hopefully you can see how we can use these additional `condition` columns to compare different samples and groups etc.

</details>

<br>

- Now that we have covered how the condition column works, we can now edit the samplesheet.csv file to contain it.
- We can use the samplesheet.csv as is and add the condition column to the end. This can be quite messy for some though, as there is a lot of columns in the file, which can lead us to typos etc.
- To make our lives a bit easier, we can use a nice bash one-liner to take the first 4 columns from the samplesheet.csv file and make a new file for us:

```
cd resources
cut -d',' -f1-4 ../input/samplesheet/samplesheet.csv > ./diff-abundance-samplesheet.csv
```
<details>
<summary>Explanation of the cut command</summary>

- The cut command extracts information from a file at specified locations.
- The `-d','` option specifies the comma deliminator of the file. i.e. cut at the commas.
- `-f1-4` option specifies the fields. When the file is cut at the commas, each field represents a column in the table. So here we are specifying the first 4 fields.

</details>

- Now if we open this new samplesheet in the nano editor, we can add the extra column with ease.

```
nano diff-abundance-samplesheet.csv

sample,fastq_1,fastq_2,strandedness,conditionOne
SRX19363186,../input/fastq/SRX19363186_SRR23454126_1.fastq.gz,../input/fastq/SRX19363186_SRR23454126_2.fastq.gz,auto,HK-2
SRX19363187,../input/fastq/SRX19363187_SRR23454125_1.fastq.gz,../input/fastq/SRX19363187_SRR23454125_2.fastq.gz,auto,HK-2
SRX19363188,../input/fastq/SRX19363188_SRR23454124_1.fastq.gz,../input/fastq/SRX19363188_SRR23454124_2.fastq.gz,auto,HK-2
SRX19363192,../input/fastq/SRX19363192_SRR23454119_1.fastq.gz,../input/fastq/SRX19363192_SRR23454119_2.fastq.gz,auto,786-0
SRX19363193,../input/fastq/SRX19363193_SRR23454122_1.fastq.gz,../input/fastq/SRX19363193_SRR23454122_2.fastq.gz,auto,786-0
SRX19363194,../input/fastq/SRX19363194_SRR23454118_1.fastq.gz,../input/fastq/SRX19363194_SRR23454118_2.fastq.gz,auto,786-0

ctrl + x
enter
```
- **Note: most of you will have each variable encased in quotation marks (""), if this is the case then make sure you stick with that format!**


**resources/contrasts.csv**

- We will also need to provide a contrasts file that is used by the pipline to perform the differential gene expression analyses.
- This file tells the pipeline what columns to use for the testing, and which groups to compare in that column.
- The file contains 4 columns: **id, variable, reference, target**

```
nano resources/contrasts.csv

id,variable,reference,target
786-0_vs_HK-2,conditionOne,HK-2,786-0

#save and exit
ctrl + x
y
enter
```

- Here, `id` is used to give the analysis an id. This can be whatever you want. To keep things simple, we have kept with the cell line names. We could also name this analysis cancer_vs_control if we wanted.
- `variable` specifies which column in the samplesheet.csv file the pipeline needs to look at to select samples for comparison. We have said to use the `conditionOne` column.
- `reference` specifies which group in the `conditionOne` column is going to be the reference group. i.e. The group we are comparing against.
- `target` specifies the gorup in the `conditionOne` column that will be used as the target group. i.e. The group that we use to compare.


**resources/Homo_sapiens.GRCh38.110.gtf**
- We also need to specify the human gene reference file.
- This is used to annotate the genes during the analysis.
- We already have this file downloaded.


**resources/diff-abundance-params.yaml**
- As with the other pipelines, we need to make a parameter file containing each option for the pipeline.

```
nano resources/diff-abundance-params.yaml

input: resources/diff-abundance-samplesheet.csv
outdir: output
-profile: rnaseq
matrix: output/star_salmon/salmon.merged.gene_counts.tsv
transcript_length_matrix: output/star_salmon/salmon.merged.gene_lengths.tsv
contrasts: resources/contrasts.csv
differential_min_fold_change: 1.5
gtf: resources/Homo_sapiens.GRCh38.110.gtf
email: <b>YOUR-EMAIL@cardiff.ac.uk</b>
study_name: <b>MY-STUDY-NAME</b>
study_type: rnaseq

#save and exit
ctrl + x
y
enter
```

- Note that we have included `study_name` and `study_type` in here, this is to make the outputs neater and is good practice.
- In some cases, you may want to run the pipieline a few times with different comparisons etc, by changing the `study_name` parameter each time, we are ensuring a new directory is created isntead of overwriting the previous analysis.

**resources/my.config**
- We created this configuration file yesterday.
- We do not need to edit this file any further.

**bin/script.sh**
- As with yesterday, lets update this file so we have as record of the code that we have used.

<details>
<summary>Edit the script.sh file</summary>
 
<br>
 
<pre><span style="color:crimson;">
#open the file using nano editor
nano bin/script.sh
 
#copy and paste the following to the end of the file
#05
#execute differentialabundance pipeline
nextflow run nf-core/differentialabundance -params-file resources/diff-abundance-params.yaml -profile singularity -c resources/my.config
#if pipeline fails for whatever reason, rerun using -resume command
nextflow run nf-core/differentialabundance -params-file resources/diff-abundance-params.yaml -profile singularity -c resources/my.config -resume

#save and exit
ctrl + x
y
enter
</span></pre>
</details>

- Our scratch directory should now look like the following:

```
.
└── rnaseq/
    ├── bin/
    │   └── script.sh
    ├── resources/
    │   ├── my.config
    │   ├── Homo_sapiens.GRCh38.110.gtf
    │   ├── diff-abundance-samplesheet.csv
    │   ├── diff-abundance-params.yaml
    │   └── contrasts.csv
    ├── input/
    │   ├── fastq
    │   ├── metadata/
    │   ├── pipeline_info/
    │   └── samplesheet/
    └── output/
        ├── fastqc/
        ├── multiqc/
        ├── pipeline_info/
        ├── sortmerna/
        ├── star_salmon/
        │   ├── salmon.merged.gene_counts.tsv
        │   └── salmon.merged.gene_lengths.tsv
        └── trimgalore/
```

---

## Executing the pipeline
---
---

- Now that we have the files ready for the pipeline, we can go ahead and execute it.
- First of all, lets exit the rnaseq tmux session and create a new one for the rnaseq pipeline.

```
#detach from the fetchngs session
ctrl + b
d

#create new tmux session
tmux new -s diff-abundance
```

- Now we can run the pipeline

```
#load modules
module load nextflow/23.10.0
module load singularity/singularity-ce/3.11.4

#execute pipe
nextflow run nf-core/differentialabundance -params-file resources/diff-abundance-params.yaml -profile singularity -c resources/my.config

#leave pipeline running for a few minutes to ensure its working, then we can close the session:
crtl +b
d
```

- We will cover the outputs from this pipeline during the Day 3 session.