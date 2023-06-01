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
