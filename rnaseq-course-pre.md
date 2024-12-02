---
title: "RNAseq Data Processing & Analysing Course"
author: "Alex Gibbs"
#full-width: true
---

# Pre-course tasks

---
## Install Visual Studio Code (VSCode)

- For this course, we will be using VSCode for everything.
- VSCode is an Integrated Development Environment (IDE) for code development. It is essentially a customisable notebook for coders.
- The beauty of this tool is that we can install extensions onto the app which means we can do everything in one window.

**To install VSCode**
- First, head on over to [the download page](https://code.visualstudio.com/Download).
- Select the version for your operating system, download and install the software.
- Note: I am not sure how well this app works for Linux users.

---

<br>

---
---
## Install tools and packages
---

- Now that we have VSCode installed, we can now install the extensions needed for the whole course.

<br>

**Remote - SSH**
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

**R**
- We will need to use R at the end of the course to use the generated shinyApp.

- To install, click on the 'Extensions' button.
- Search for 'R'.
- Click on the 'R' extension and then 'Install' at the top.

<img src="/assets/img/pre-4.png" alt="Installing R" width="1000"/>

<br>

**Optional: Excel Viewer**
- These are not required, but can massively help when editing CSV files.
- Excel Viewer allows us to open CSV files as a table, rather than a text file.
- Click 'Extensions' button.
- Search for 'excel viewer', then 'Install' at the top.
- To open CSV files with this extension, right click on the file and click 'Open With...'. This opens a drop down menu at the top of the window.
- Click on the second option: 'CSV Editor'.

<img src="/assets/img/pre-5.png" alt="Installing Excel Viewer" width="1000"/>


## Connecting to HAWK

- To connect to HAWK, click on the 'Remote Explorer' button, then click 'Connect to Host in New Window' button. This opens a new VSCode window with the remote host.
- You will be prompted (top box) to enter your password. Do this and hit enter.
- You are now connected to HAWK.

<img src="/assets/img/pre-3.png" alt="Connecting to HAWK" width="1000"/>

















## Install R, RStudio, and packages
- We will need to use Rstudio at the end of the course to run the shiny application.
- Firstly, we need to install R, then we can install RStudio, and then the packages.

**Install R**
- Go to this [link](https://www.stats.bris.ac.uk/R/) and download the app.
- After installing, open R to initialise everything.

<img src="/assets/img/figure-pre-1.png" alt="R window" width="1000"/>

- Once it has finished opening and initialised, we can go ahead and close it.

**Install RStudio**
- Go to this [link](https://posit.co/download/rstudio-desktop/) and download the app.
- After installing, open RStudio.

<img src="/assets/img/figure-pre-2.png" alt="RStudio window" width="1000"/>








