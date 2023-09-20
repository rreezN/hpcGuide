# hpcGuide
Quick DTU HPC Guide

# How to activate on HPC
This example will create a python virtual environment called `DeepLearning`.

$ are terminal commands
1. open terminal in same folder as a project and type the following commands (you can paste them into the terminal with middle mouse click)
2. ```$ module load python3/3.11.3```
3. ```$ module load cuda/11.7```
4. ```$ python3 -m venv DeepLearning```
5. ```$ source DeepLearning/bin/activate```
6. ```$ pip3 install -r requirements.txt```

# Example job script 
This example script requests one V100 gpu, you might also consider the queue `gpua100` to get a A100 gpu, save this as `submit.sh`. Run it with `bsub < submit.sh`
```sh
#!/bin/sh
#BSUB -J my_job_name
#BSUB -o my_job_name%J.out
#BSUB -e my_job_name%J.err
#BSUB -q gpuv100
#BSUB -gpu "num=1:mode=exclusive_process"
#BSUB -n 1
#BSUB -R "rusage[mem=8G]"
#BSUB -W 10:00
#BSUB -N
# end of BSUB options

# load a module
# replace VERSION 
module load python3/3.11.3

# load CUDA (for GPU support)
# load the correct CUDA for the pytorch version you have installed
module load cuda/11.7

# activate the virtual environment
# NOTE: needs to have been built with the same numpy / SciPy  version as above!
source project/env/bin/activate

python3 my_script.py
```
## batch scripts
You can call the same job script many times in a row with different parameters. Here's and example of calling the above script with 1, 2, and 4 cores. This syntax will overwrite whatever is in the original `submit.sh` file.
```
#!/bin/sh
bsub -n 1 < submit.sh
bsub -n 2 < submit.sh
bsub -n 4 < submit.sh
```

Now everything should be setup. Then see the ```submit.sh``` shell script for how it is activated. It should be run from the same path as the project.

# Using notebooks remotely

terminal 1
ssh s123456@login1.hpc.dtu.dk
// assuming you have created a shh key similar to gbar1 below
ssh hpc1
<password>
cd DLinCV/DL-COMVIS
02514sh
source ../DLCV/bin/activate
jupyter notebook --no-browser --port=7800 --ip=$HOSTNAME


terminal 2
// change the port (7800) and node number (n-62-11-16) accordingly
ssh s123456@login1.hpc.dtu.dk -g -L 7800:n-62-20-5:7800 -N
// or if you have set up a hpc1 shortcut
ssh hpc1 -g -L 7800:n-62-11-16:7800 -N

<password>


Datasæt mappe
(fra home, virker åbenbart ikke med tmux)
cd /dtu/datasets1/02514

transfer file from local to remote
scp s123456@login1.hpc.dtu.dk:/dtu/datasets1/02514/demo.ipynb .

CUDA_VISIBLE_DEVICES=0,1

// tensorboard
ssh s123456@login1.hpc.dtu.dk -g -L 6006:n-62-20-6:6006 -N
ssh hpc1 -g -L 6005:localhost:6005 -N


# Other useful commands

`watch -n 1 bstat` will continously monitor the job status

`bkill {job_id}` to kill a job (needed more often than you think).

`module avail`, you can e.g. use `module avail cuda` to list all cuda subversions available

`getquota_zhome.sh` to see disk usage

# Using VSCode remote to access the HPC
Assuming you have you private key saved in `~/.ssh/gbar`

Instruction for generating a private key and placing it on the server can be found [here](https://www.hpc.dtu.dk/?page_id=4317)

create the file `~\.ssh\config' with the content
```ssh
Host gbar1
User s123456
IdentityFile ~/.ssh/gbar
HostName login1.gbar.dtu.dk
```

# Copying folder and files with scp

To copy all from Local Location to Remote Location (Upload)

```scp -r /path/from/local username@hostname:/path/to/remote```
if you have setup the gbar1 profile as above you can do
```scp -r /path/from/local gbar1:/path/to/remote```
To copy all from Remote Location to Local Location (Download)

```scp -r username@hostname:/path/from/remote /path/to/local```

# How to extend terminal history
This is more necessary than you might think, for example calling the command `module avail` will give an output so long that you can scroll back far enough to see everything.

open the file `~./Xresources` and change the `saveLines` to something like the shown
```
# File : .Xresources
!xrdb -merge ~/.Xrescources to generate new settings
XTerm*background: black
XTerm*foreground: white
XTerm*saveLines: 1000
```
# Script for clearing trash
create the file in home folder using `touch cleartrash.sh` put this content in it
```bash
#!/bin/bash
# clearing trash
echo "Clearing trash"
cd ~
cd .local/share/Trash/files
rm -rf *
cd ~
cd .local/share/Trash/info
rm -rf *
```

now run it with `./cleartrash.sh` from `~`
