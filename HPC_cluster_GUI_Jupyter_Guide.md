# Access to Rollins HPC R environment with GUI, using Conda and Jupyter Server 

> By Dr. Liuhua Shi | Emory University, Dr. Pengfei Liu | Georgia Tech, and Wenhao Wang | Emory University
>> Contact: wenhao.wang@emory.edu

## Why Conda?

I. Installation of Anaconda on a remote server does not require root permission. Other solutions, such as Rstudio server will need root, and only cluster manager can install.

II. Anaconda is completely free

III. Graphic interface with Jupyter notebook.

## Install Conda and configure environment 
## you only need to install Conda and environment once. you'll need to activate the environment every time before starting Jupyter
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

It could be other 4-digit numbers too, eg 8111,8222,8333, 6820 (except 8888). Just make sure that you are using the same 4-digit numbers througout.

>Note: You should have already finished the onboard process of the HPC by resetting passowrd.

### 3. Download Miniconda

The latest version of Miniconda can be downloaded from the link below:

https://docs.conda.io/en/latest/miniconda.html

Create download dir:

`mkdir download`

`cd download`

`wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh`

`bash ./Miniconda3-latest-Linux-x86_64.sh`

Follow the instructions to install miniconda. You need to re-login to continue the following steps.

### 4. Install R

Creat an environment, named 'r-env', install r-base and r-essentials packages

`conda create -n r-env r-base r-essentials`

`conda activate r-env`

`conda install jupyter`

You have finished conda installation and configuration of an environment for R!

## You will need to follow the steps below every time you use Anaconda on the cluster 

If you have not login the cluster, please do so. Make sure you are in your home directory

### 5. Allocate Computation Resources on the HPC

### Attention: Do not run computation intensive jobs on the login node. We need to run R on the interactive-cpu partition.

You can type "sinfo" in the terminal to check which nodes are idle (i.e. available), and then log into that node. For example, the following command will apply for 1 CPU core, 1GB memory, 8 hours on the interactive-cpu partition. You may change these parameters according to your jobs. 

`salloc -p interactive-cpu -n 1 -t 0-8:00 --mem=1000`

Once you get the message "Granted job allocation XXXXXX"

`ssh -L XXXX:localhost:XXXX $SLURM_JOB_NODELIST`

Now you are on a computation node.

First, I would recommend check the memory size that is available using `top -o %MEM`. It can also tell you that whether your previous finished/killed jobs are still occupying the memory size or not. If yes, please "Ctrl-C" and then use `kill -9 PID` to manually kill the jobs so that the memory size can be released.
 
Activate your R environment:
`conda activate r-env`

Start Jupyter server:
Never run the following command before you have successfuly allocate resources on computation nodes. You'll get a warning email if you run Jupyter on log-in nodes. 
`jupyter notebook --NotebookApp.token='' --no-browser --port=8999`

Please double check all symbols in this line is in English (rather than in other languages), otherwise you may get some weird page asking for token/password.
You may save this line of command into a file under ~/bin/myjupyter, then add execute, chmod 755 myjupyter, next time you may just type myjupyter to start jupyter notebook.

### 6. Open Jupyter Notebook at Local Machine.
On you own PC (Mac, Linux, Windows, IPad, phone), open your browser, type at the address bar and visit:

[http://localhost:8999](http://localhost:8999/)

> You may need to enter the password if you set password at Step 6 (i.e., --NotebookApp.token='PASSWORD').

Jupyter Notebook should start in your browser.

### 7. Open R script to link the session
Click 'New --> R' at upper-right corner to srart an R notebook.

### 8. Open Terminal within Jupyter 
Click 'New --> Terminal' at upper-right corner to srart a terminal, so one can monitor the memory/CPU resources:

`top -o %MEM` or `top`

Click "Ctrl-C".

### 9. Manually kill the job in Terminal to release the memory/CPU resources
Manually kill a job according to its PID number, so that the resources will be properly released. In the terminal (step 8): 

`kill -9 PID` 

Type 'exit' to end the interactive-cpu node.

Type 'exit' again to end HPC connection.


