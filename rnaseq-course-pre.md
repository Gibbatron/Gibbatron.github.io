---
title: "RNAseq Data Processing & Analysing Course"
author: "Alex Gibbs"
#full-width: true
---

# Pre-course tasks

<br>

---
## Install Visual Studio Code (VSCode)
---
---

- For this course, we will be using VSCode for everything.
- VSCode is an Integrated Development Environment (IDE) for code development. It is essentially a customisable notebook for coders.
- The beauty of this tool is that we can install extensions onto the app which means we can do everything in one window.

<u><b>To install VSCode</u></b>
- First, head on over to [the download page](https://code.visualstudio.com/Download).
- Select the version for your operating system, download and install the software.
- Note: I am not sure how well this app works for Linux users.

- Tick all the boxes in the start up window and click 'Mark Done'.

---
---
#### Install tools and packages in VSCode
---

- Now that we have VSCode installed, we can now install the extensions needed for the whole course.

<u><b>Remote - SSH</b></u>
- This extension allows us to connect remotely to HAWK through a terminal window within the software.

- To install, click on the 'Extensions' button.
- Search for 'remote ssh'.
- Click on the 'Remote - SSH' extension and then 'Install' at the top.

<img src="/assets/img/pre-1.png" alt="install Remote-SSH" width="1000"/>

- Then, to setup the extension to login to your HAWK, click on 'Remote Explorer'.
- Then click on the '+' symbol to add our SSH target.
- You will see a drop down box appear at the top of the screen asking for your login details.
- Enter the following (add your username) and hit enter:

```
ssh c.c1234567@hawklogin.cf.ac.uk
```

- You will then be asked to select a SSH configuration file to update.
- Select the **top** one.

<img src="/assets/img/pre-2.png" alt="Setting up HAWK" width="1000"/>

- You will now see the host has been added to the 'Remote Explorer' window.

<br>

<u><b>Optional: Excel Viewer</u></b>
- These are not required, but can massively help when editing CSV files.
- Excel Viewer allows us to open CSV files as a table, rather than a text file.
- Click 'Extensions' button.
- Search for 'excel viewer', then 'Install' at the top.
- To open CSV files with this extension, right click on the file and click 'Open With...'. This opens a drop down menu at the top of the window.
- Click on the second option: 'CSV Editor'.

<img src="/assets/img/pre-5.png" alt="Installing Excel Viewer" width="1000"/>

<br>

---
## Connecting to HAWK
---
---
- To connect to HAWK, click on the 'Remote Explorer' button, then click 'Connect to Host in New Window' button. This opens a new VSCode window with the remote host.
- You will be prompted (top box) to enter the host operating platform.
- Click **Linux**.
- Then you will be prompted to continue, click **Yes**.
- Then you will be asked for your password. Do this and hit enter.
- You are now connected to HAWK.

<img src="/assets/img/pre-3.png" alt="Connecting to HAWK" width="1000"/>

<br>

---
## Download Gene Set Enrichment Analysis (GSEA) Application
---
---

- GSEA application is needed at the end of the course to analyse our DEGs.
- Go to the following webpage and download GSEA version 4.3.2 (latest version may not work for everyone so we are using this one): https://data.broadinstitute.org/gsea-msigdb/gsea/software/desktop/4.3/
- Make sure you click on the download relevant to your operating system: GSEA_MacApp for Mac, GSEA_Win for Windows, GSEA_Linux for Linux.
- Unzip the file by double clicking or right-clicking and selecting unzip.
- Now you should have, in the same location, your `GSEA_4.3.2.app`.
- You can move this app to anywhere you want and it will still work when you open it.
- Double-click the app to ensure that it is working correctly.

<img src="/assets/img/figure-48.png" alt="GSEA window" width="1000"/>


<br>

---
## Learning Materials
---
---

- We won't be covering how RNAseq technologies work or the specific details of the nf-core pipelines.
- Please see the following links to learning materials to cover them:

**Bulk RNA Sequencing**

- [Beginners guide to bulk RNA-seq analysis](https://www.youtube.com/watch?v=WW94W-DBf2U)
- [How Illumina sample prep and sequencing works](https://www.youtube.com/watch?app=desktop&v=fCd6B5HRaZ8)
- [A Gentle Introduction to: RNA-seq](https://www.youtube.com/watch?v=tlf6wYJrwKY)

**nf-core Pipelines**
- Details of the fetchngs pipeline, including what tools/packages etc are used can be found [here](https://nf-co.re/fetchngs/1.11.0).
- Details of the rnaseq pipeline, including what tools/packages etc are used can be found [here](https://nf-co.re/rnaseq/3.14.0/). Also a [YouTube video](https://www.youtube.com/watch?v=qMuUt8oVhHw) briefly explaining the pipeline.
- Details of the differentialabundance pipeline, including what tools/packages etc are used can be found [here](https://nf-co.re/differentialabundance/1.4.0).

**GSEA**
- [GSEA User Guide](https://www.gsea-msigdb.org/gsea/doc/GSEAUserGuideFrame.html?xtools_gsea_GseaPreranked). Scroll down for info on how to interpret the data.
- [GSEAPreranked Information Page](https://www.gsea-msigdb.org/gsea/doc/GSEAUserGuideFrame.html?xtools_gsea_GseaPreranked).