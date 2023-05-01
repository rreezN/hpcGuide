# hpcGuide
Quick DTU HPC Guide

# How to activate on HPC
$ are terminal commands
1. open terminal in same folder as this project and type the following commands (you can paste them into the terminal with middle mouse click)
2. ```$ module load python3/3.9.6```
3. ```$ module load cuda/11.3```
4. ```$ python3 -m venv DeepLearning```
5. ```$ source DeepLearning/bin/activate```
6. ```$ pip3 install -r requirements.txt```

Now everything should be setup. Then see the ```submit.sh``` shell script for how it is activated. It should be run from the same path as the project.

# Other useful commands

`watch -n 1 bstat` will continously monitor the job status

`bkill {job_id}` to kill a job (needed more often than you think).

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
