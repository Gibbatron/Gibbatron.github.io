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
- 