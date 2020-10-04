# Access to Rollins HPC R environment, using Conda an Jupyter Server 

> Original version developed by Dr. Pengfei Liu | Georgia Inistitute of Technology, Dr. Liuhua Shi | Emory University.

## Why Conda?

I. Installation of Anaconda on a remote server does not require root permission. Other solutions, such as Rstudio server will need root, and only cluster manager can install.

II. Anaconda is completely free

III. Graphic interface with Jupyter notebook.

## Instruction
### 1. Before Process
Before your login to the Rollins HPC. Make sure you can login to the Emory VPN. You may also need to change your default login shell for your Emory ID to 'bash'. Please see links below for details:

https://vpn.emory.edu

https://mynetid.emory.edu/IDMProv/portal/cn/DefaultContainerPage/WelcometoMyNetID

### 2. Set ssh tunnel and HPC login
#### Windows Putty User

In putty, click ssh -> tunnel, add tunnel 8999:localhost:8999

>Windows Users are highly recommended to install PowerShell from Microsoft Software Store, so that you can have terminal like mac users.
#### Mac/Linux/Window Powershell Users
In terminal, type:

`ssh -L 8999:localhost:8999 username@clogin01.sph.emory.edu`

>Note: You should have already finished the onboard process of the HPC by resetting passowrd.

### 3. Download Miniconda

The latest version of Miniconda can be downloaded from the link below:

https://docs.conda.io/en/latest/miniconda.html

Create download dir:

`mkdir download`

`cd downloaed`

`wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh`

`bash ./Miniconda3-latest-Linux-x86_64.sh`

Follow the instructions to install miniconda. You need to re-login to continue the following steps.

### 4. Install R

Creat an environment, named 'r-env', install r-base and r-essentials packages

`conda create -n r-env r-base r-essentials`

`conda activate r-env`

`conda install jupyter`

### 5. Allocate Computation Resources on the HPC

> Attention: Do not run computation intensive jobs on the login node. We need to run R on the interactive-cpu partition.

For example, the following command apply 4 CPU cores, 32GB memory on the interactive-cpu partition.

`sallow -p interactive-cpu -n 4 -6 1-00:00 --mem=32000`

`ssh -L 8999:localhost:8999 $SLURM_JOB_NODELIST`

> You may fail to applicate 4 cores, you may need to change the CPU number to 1 for a authorization from the HPC.

### 6. Start Jupiter Server Under Home Directory:
> You may need to re-login to the HPC and activate the r-env to make the jupyter command accessible.

` jupyter notebook --NotebookApp.toke=‘PASSWORD’ --no-browser --port=8999`

>Please set adequate password to maintain secure link to jupyter server.

You may save this line of command into a file under ~/bin/myjupyter, then add execute, chmod 755 myjupyter, next time you may just type myjupyter to start jupyter notebook.

### 7. Open Jupyter Notebook at Local Machine.
On you own PC (Mac, Linux, Windows), open your browser, type at the address bar and visit:

[http://localhost:8999](http://localhost:8999/)

> You may need to enter the password you set at Step 6.

Jupyter Notebook should start in your browser.

### 8. Open R script to link the session
Click 'New --> R' at upper-right corner to srart an R notebook.

After R session, use Crtl-C to end the Jupyter server.

Type 'exit' to end the interactive-cpu node.

Type 'qstat' to make sure the session is not running.

Then, you can use 'exit' again to end HPC connection.


