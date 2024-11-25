---
title: "RNAseq Data Processing & Analysing Course"
author: "Alex Gibbs"
#full-width: true
---

---
# Day 2 - Processing RNAseq reads with nf-core/rnaseq pipeline

---

## Recap from Day 1
---
---

- Yesterday we covered:
  - How to use Unix.
  - How to get onto HAWK and navigate it.
  - How to execute the fetchngs pipeline.

---
## Outputs from the fetchngs pipeline
---
---

- We should all have had an email from *SCW HAWK - HPC SERVICES* notifying you of a successful pipeline run.
- Sometimes the email function doesn't work. We can just log in and check ourselves.
- To check, we need to log back onto HAWK, load the tmux module, and then open the session that we created.
- NOTE: When using tmux, we need to make sure we are logged into the correct node. When we covered the log ins yesterday, we logged onto the cl1 node. The tmux session we made will only be present on the cl1 node. If we were to log in to the cl2 node, we wouldnt be able to find the tmux session.

```
#log onto HAWK
c.c1234567@hawklogin01.cf.ac.uk
PASSWORD

#move to the working directory (scratch)
cd /scratch/c.c134567/rnaseq

#load tmux
module load tmux

#open our tmux session
tmux attach -t fetchngs
```

- Upon opening the session, we should have a window that looks like this:

<img src="/assets/img/figure-17.png" alt="tmux completed run" width="1000"/>

You should also see the following text at the end (we will cover the message shortly):

<span style="color:green;">Checksums match: All downloaded files are verified successfully.</span>


- In this situation, the process took 25 minutes. As all of us were using the pipeline at the same time, these times will likely vary. Download times will also depend on the size and number of files being downloaded.

---
#### Addition of the work directory
---
---

- If we list (`ls`) the contents of the file, we will see that there is a new `work` directory.
- This directory is created by the pipeline and uses it as a storage space for intermediate files etc.

---
#### Nextflow log file
---
---

- If we were to encounter an error whilst running a pipeline, we can check the log file that is created.
- To do this, we need to use the `ls -la` command to show hidden files.
- We will see a file named `.nextflow.log`. This file is what contains the pipeline running information.

---
#### fastq files
---
---

- When we run the pipeline yesterday, we asked for the fastq files to download to the input directory.
- Running `ls` on the input directory shows that there are 4 new directories that have been created: **fastq**, **metadata**, **pipeline_info**, and **samplesheet**.
- The fastq files can be found in the **fastq** directory. If we list the contents of the fastq directory, we will see that there is also a **md5** directory.
- This directory contains the md5 checksums for each fastq file.
- You would have noticed that there was a message at the end of the pipeline:

<span style="color:green;">Checksums match: All downloaded files are verified successfully.</span>

<details>
<summary><b>What is an md5 checksum?</b></summary>

<br>

An MD5 checksum is like a digital fingerprint for a file. When you create an MD5 checksum, it takes the contents of the file and runs it through a special formula to produce a unique string of characters. This string is always the same length, no matter how big or small the file is.

You can use this checksum to verify that the file hasn’t changed. For example, if you download a file and it comes with an MD5 checksum, you can run the same formula on your downloaded file. If your checksum matches the one provided, it means the file is intact and hasn’t been altered. If it doesn’t match, the file might be corrupted or tampered with.

</details>

- It is good practice to validate your checksums to ensure that we dont have any data loss.
- Running checksums can take some time, but luckily for you I have added the code the end of the `fetchngs.sh` script.


---
#### Samplesheet file
---
---

- We will also see that there is a samplesheet directory inside the input directory.
- This directory contains the information about the samples that we have downloaded.
- Specifically, the **samplesheet.csv** file contains all of the relevant information such as file location, sample ID etc that we need as input for the nf-core/rnaseq pipeline.
- This file should be formatted specifically for the rnaseq pipeline because we used the `nf_core_pipeline: rnaseq` option within the parameters file we created for the fetchngs pipeline.
- This specific formatting should make the first four columns of the file as followes: **sample, fastq_1, fastq_2, strandedness**.

<details>
<summary><b>Check the samplesheet.csv file is correctly formatted</b></summary>

<br>

<pre><span style="color:crimson;">
#check the header of the file, i.e. the columns
head -1 input/samplesheet/samplesheet.csv

#Output from console
"sample","fastq_1","fastq_2","strandedness","run_accession","experiment_accession","sample_accession","secondary_sample_accession","study_accession","secondary_study_accession","submission_accession","run_alias","experiment_alias","sample_alias","study_alias","library_layout","library_selection","library_source","library_strategy","library_name","instrument_model","instrument_platform","base_count","read_count","tax_id","scientific_name","sample_title","experiment_title","study_title","sample_description","fastq_md5","fastq_bytes","fastq_ftp","fastq_galaxy","fastq_aspera"
</span></pre>
</details>

---
#### Metadata directory
---
---

- The metadata directory contains re-formatted metadata files. These files contain the same metadata information as the samplesheet file, but doesn't contain the file location etc.
- We don't need to use the information from this directory.

---
#### pipeline_info directory
---
---

- This directory contains all the files relevant to the pipeline that was run, such as a nextflow report, pipeline report.
- These files can be used to make a report if needed (handy when writing methods section).
- We don't need any of these files for the next stages.

---
## nf-core/rnaseq pipeline
---
---

Required files:

---
#### input/samplesheet/Samplesheet.csv
---
---

- As asforementioned, this file is generated by the nf-core/fetchngs pipeline. We have already checked to ensure the formatting of the first four columns is correct.

---
#### resources/reference genome files
---
---

- We will need to provide a reference genome for the pipeline to use.
- The reference genome is used by the pipeline as a template for the sequencing reads. This allows for a more accurate assembly of our sequencing reads.
- More information about the mapping process can be found in this [video](https://www.youtube.com/watch?v=tlf6wYJrwKY).
- Luckily, reference genomes are readily available (if using commonly-researched species) for us to use.
- Our samples are human, therefore we will need to find and download the latest human reference genome.
- Reference genomes can be found [here](https://ftp.ensembl.org/pub/release-110/fasta/) and [here](https://ftp.ensembl.org/pub/release-110/gtf/).

<details>
<summary><b>Download the human reference genome files</b></summary>

<br>

<pre><span style="color:crimson;">
#download the human GRCh38 reference DNA sequence
#paste this chunk into your console and run. Make sure you are in the resources directory
wget https://ftp.ensembl.org/pub/release-110/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna_sm.primary_assembly.fa.gz && gunzip Homo_sapiens.GRCh38.dna_sm.primary_assembly.fa.gz

#download the human GRCh38 reference gene information file
#paste this chunk into your console and run. Make sure you are in the resources directory
wget https://ftp.ensembl.org/pub/release-110/gtf/homo_sapiens/Homo_sapiens.GRCh38.110.gtf.gz && gunzip Homo_sapiens.GRCh38.110.gtf.gz

</span></pre>
</details>

---
#### resources/rnaseq-params.yaml
---
---

- We need to create another parameter file for this pipeline.

<details>
<summary><b>Make the rnaseq-params.yaml file</b></summary>

<br>

<pre><span style="color:crimson;">
#create and open the file using nano editor
nano rnaseq-params.yaml

#copy and paste the following
input: input/samplesheet/samplesheet.csv
outdir: output
aligner: star_salmon
save_align_intermeds: false
save_unaligned: true
remove_ribo_rna: true
save_non_ribo_reads: false
save_trimmed:  false
save_reference: false
gtf: resources/Homo_sapiens.GRCh38.110.gtf
fasta: resources/Homo_sapiens.GRCh38.dna_sm.primary_assembly.fa
email: <b>my.email@cardiff.ac.uk</b>

#save and exit
crtl + x
y
enter
</span></pre>
</details>

---
#### resources/my.config
---
---

- We created this configuration file yesterday.
- We do not need to edit this file any further.


- Our scratch directory should now look like the following:

```
.
└── rnaseq/
    ├── bin/
    │   └── script.sh
    ├── resources/
    │   ├── rnaseq-params.yaml
    │   ├── my.config
    │   ├── Homo_sapiens.GRCh38.dna_sm.primary_assembly.fa
    │   └── Homo_sapiens.GRCh38.110.gtf
    ├── input/
    │   ├── fastq/
    │   │   ├── SRX19363186_SRR23454126_1.fastq.gz
    │   │   ├── SRX19363193_SRR23454122_1.fastq.gz
    │   │   ├── SRX19363186_SRR23454126_2.fastq.gz
    │   │   ├── SRX19363193_SRR23454122_2.fastq.gz
    │   │   ├── SRX19363187_SRR23454125_1.fastq.gz
    │   │   ├── SRX19363194_SRR23454118_1.fastq.gz
    │   │   ├── SRX19363187_SRR23454125_2.fastq.gz
    │   │   ├── SRX19363194_SRR23454118_2.fastq.gz
    │   │   ├── SRX19363188_SRR23454124_1.fastq.gz
    │   │   └── SRX19363188_SRR23454124_2.fastq.gz
    │   ├── metadata/
    │   ├── pipeline_info/
    │   └── samplesheet/
    │       └── samplesheet.csv
    └── output/
```

---

## Executing the pipeline
---
---

- Now that we have the files ready for the pipeline, we can go ahead and execute it.
- First of all, lets exit the prefetch tmux session and create a new one for the rnaseq pipeline.

```
#detach from the fetchngs session
ctrl + b
d

#create new tmux session
tmux new -s rnaseq
```

- Now we can run the pipeline

```
#load modules
module load nextflow/23.10.0
module load singularity/singularity-ce/3.11.4

#execute pipe
nextflow run nf-core/rnaseq -profile singularity -c resources/my.config -params-file resources/rnaseq-params.yaml

#leave pipeline running for a few minutes to ensure its working, then we can close the session:
crtl +b
d
```

- We will cover the outputs from this pipeline during the Day 3 session.