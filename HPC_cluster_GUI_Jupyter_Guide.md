# Access to Rollins HPC R environment with GUI, using Conda and Jupyter Server 

> By Dr. Pengfei Liu | Georgia Tech, Dr. Liuhua Shi and Wenhao Wang | Emory University
> Contact: wenhao.wang@emory.edu

## Why Conda?

I. Installation of Anaconda on a remote server does not require root permission. Other solutions, such as Rstudio server will need root, and only cluster manager can install.

II. Anaconda is completely free

III. Graphic interface with Jupyter notebook.

## Install Conda and configure environment 
# you only need to install Conda and environment once. you'll need to activate the environment every time before starting Jupyter
### 1. Connect Emory VPN
Before your login to the Rollins HPC, please make sure you have connected Emory VPN. You can download the "BIG-IP Edge Client" app for VPN. You may also need to change your default login shell for your Emory ID to 'bash'. Please see links below for details:

https://vpn.emory.edu

https://mynetid.emory.edu/IDMProv/portal/cn/DefaultContainerPage/WelcometoMyNetID

### 2. Set ssh tunnel and HPC login
#### Windows Putty User

In putty, click ssh -> tunnel, add tunnel 8999:localhost:8999

>Windows Users can alternatively install PowerShell from Microsoft Software Store, so that you can have terminal like mac users.

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

You have finished conda installation and configuration of an environment for R!

## You will need to follow below steps every time you use Anaconda on the cluster 

If you have not login the cluster, please do so. Make sure you are in your home directory

### 5. Allocate Computation Resources on the HPC

### Attention: Do not run computation intensive jobs on the login node. We need to run R on the interactive-cpu partition.

For example, the following command will apply for 1 CPU core, 16GB memory, 8 hours on the interactive-cpu partition. You may change these parameters according to your jobs. 

`salloc -p interactive-cpu -n 1 -t 0-8:00 --mem=16000`

Once you get the message "Granted job allocation XXXXXX"

`ssh -L 8999:localhost:8999 $SLURM_JOB_NODELIST`

Now you are on a computation node.

Activate your R environment:
`conda activate r-env`

Start Jupyter server:
Never run the following command before you have successfuly allocate resources on computation nodes. You'll get a warning email if you run Jupyter on log-in nodes. 
`jupyter notebook --NotebookApp.toke='' --no-browser --port=8999`

You may save this line of command into a file under ~/bin/myjupyter, then add execute, chmod 755 myjupyter, next time you may just type myjupyter to start jupyter notebook.

### 7. Open Jupyter Notebook at Local Machine.
On you own PC (Mac, Linux, Windows, IPad, phone), open your browser, type at the address bar and visit:

[http://localhost:8999](http://localhost:8999/)

> You may need to enter the password if you set password at Step 6 (i.e., --NotebookApp.toke='PASSWORD').

Jupyter Notebook should start in your browser.

### 8. Open R script to link the session
Click 'New --> R' at upper-right corner to srart an R notebook.

After R session, use Crtl-C to end the Jupyter server.

Type 'exit' to end the interactive-cpu node.

Type 'qstat' to make sure the session is not running.

Then, you can use 'exit' again to end HPC connection.


