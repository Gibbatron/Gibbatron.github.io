---
title: "RNAseq Data Processing & Analysing Course"
author: "Alex Gibbs"
#full-width: true
---

# Pre-course tasks

---

## Install Cardiff University VPN
---
---

- We first need to install the universities VPN. This is needed to connect to the universities high performance compute (HPC) system, which is where we will be launching the pipelines from.
- We will need to have the Global Protect VPN installed on our PC's in order to connect to HAWK off campus.

---

For **MAC:**

  - Use this [link](https://software.cardiff.ac.uk) and log in to the Software Downloads website with your university log in.
  - Then click on *Global Protect*, then click the *MAC OS* link, then click on the *GlobalProtect-6.2.4.pkg* download button to initiate the download to your PC.
  - Once the file has downloaded, navigate to it and double-click on it to initiate installation onto your PC.
  - Navigate through the installer by clicking *Continue*.
  - Check boxes for *GlobalProtect* and *GlobalProtect System Extensions* and click *Continue*.
  - Click *Install*.
  - Enter your admin password when prompted and click *Install Software*.
  - Click *Close* once the installation has completed.
  - When prompted, choose to *Allow* the system extension in Security and Privacy preferences.
  - Click on the system notification to Allow notifications from GlobalProtect.

---
For **Windows:**

  - Use this [link](https://software.cardiff.ac.uk) and log in to the Software Downloads website with your university log in.
  - Then click on *Global Protect*, then click the *Windows* link, then click on the 32- or 64-bit client link. To find out what bit your system is, go to Control Panel > System and Security > System. In the System area, the System Type shows if the system is 32 bit or 64 bit.
  - Now click on the *GlobalProtect64-6.2.4.msi* download button to initiate the download to your PC.
  - Click *Run* to start installation. Accept any browser security messages if they pop up.
  - Click *Next* in the setup wizard and accept the default installation folder on your PC.
  - Click *Next* to install the Agent in the default location in *Program Files*.
  - Click *Next* to continue with the installation.
  - You will be notified once installation has completed, click *Close* to finish.

---
**Connecting to the VPN:**

- The VPN runs automatically after installation.
- Enter the portal address as *ras.cf.ac.uk*
- Enter your university username and password.
- Click *OK* when prompted to allow GlobalProtect access to your Desktop, Documents, and Downloads folders (three prompts).

---
---

## Install file transfer programs
---
---

#### Mac - FileZilla
- Mac users have a built-in Unix shell called bash. To open this, open the *Terminal* application/program.
- To transfer files to- and from the HAWK servers that we will be using, we can either use code in Terminal (advanced use), or use a file transfer program (FTP) called FileZilla.

**FileZilla Installation**

- Search for *FileZilla* on Google, or follow this [link](https://filezilla-project.org/download.php?type=client).
- Click on the *Download FileZilla Client* button to initiate the download.
- Once downloaded, double-clicking on the downloaded file will initiate installation.
- Once installed, open the software.

**To connect to HAWK:**

- Host: **hawklogin01.cf.ac.uk** 
- Username: **c.c123456**  (Your HAWK account username)
- Password: **XXXXXXX**  (Your HAWK account password)
- Once connected, you will see that you have two connections.
- Your PC files and folders are on the left box.
- Your HAWK files and folders are on the right box.
- **To transfer files, simply drag and drop from one side to the other.**
- You can also double-click on the respective folders to navigate to wherever you want to be.

<img src="/assets/img/figure-2.png" alt="FileZilla Log-in" width="1200"/>

---

#### Windows - MobaXterm
- Windows users don't have an integrated Unix shell, so you will need to download and install one. We will use **MobaXterm**.
- MobaXterm is an integrated shell, which combines command line with FTP all in one window/application.

**MobaXterm Installation**

- Search for *MobaXterm* on Google, or follow this [link](https://mobaxterm.mobatek.net/download.html).
- Click on the *Download Now* button on the Home Edition version to initiate the download.
- Once downloaded, click *Run* to initiate installation.
- Once installed, open the software.

**To connect to HAWK:**

# THIS BIT NEEDS COMPLETING
# MAKE SURE TO INCLUDE HAWKLOGIN01 TO TAKE USERS TO CL1 NODE!

---





















---
---

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

**Install required packages**

# update this section






