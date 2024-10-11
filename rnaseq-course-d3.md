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

#### FastQC and MultiQC

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
- There is a great video that covers the outputs of the MultiQC report [here](https://www.youtube.com/watch?v=qPbIlO_KWN0)