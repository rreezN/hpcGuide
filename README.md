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
