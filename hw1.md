---
tags: ggg, ggg2025, ggg201b
---

[toc] 

# GGG 201b WQ 2025 - Lab Homework #1 - Jan 27, 2025

Due: noon on Wed, Feb 5th

Titus office hours in DataLab: 
* 2:45pm-3:45pm on Monday, Feb 3th (Shields 362)
* 3pm-4:30pm on Tuesday, Feb 4th (south side of DataLab, by main entrance)

## Summary of homework 1

Get set up:

* Create/log into an account on [github.com](https://www.github.com) and connect it to this assignment on GitHub Classroom by clicking [here](https://classroom.github.com/a/GuLYKzE1).
* Log into ondemand and start up RStudio Server; use the same settings as from [lab 3](https://hackmd.io/8MUDxb3FTSqMod9Q9LHKWA?view).
* Retrieve your ssh public key from your farm account and then upload it to GitHub.

A video of the steps for uploading your ssh key is here (from last year's class): [link](https://video.ucdavis.edu/media/t/1_ou2ju79j).

Then,
* clone your homework 1 repository into your home directory on farm;
* edit/adjust the Snakefile to analyze all four sequencing data sets (see below instructions);
* rerun the Snakefile;
* summarize the variants in `hw1.txt` per the instructions;
* add, commit, and push your edited `Snakefile` and `hw1.txt` file (and _only_ those files);
* verify that your updated `Snakefile` and your new `hw1.txt` file are visible via your GitHub repo for hw1. **If you don't see them on the Web interface for your github repository, they are not turned in.**


## A more detailed written walkthrough

### Log into ondemand and start up RStudio Server

[As in lab 3.](https://hackmd.io/8MUDxb3FTSqMod9Q9LHKWA?view)

### Log into github.com

Create a new account on github.com, or log in to an existing account.

Click on https://classroom.github.com/a/mlPFDg1W,  connect your class username to your GitHub account, and accept this assignment (hw1).

Wait for your copy of the homework to be created; when you get to a page with a URL that looks like this, `https://github.com/ngs-docs/2025-ggg-201b-hw1-USERNAME`, you're good!

## Use an ssh key to connect to GitHub

In RStudio Server, open a Terminal window and create an ssh public/private key pair:

```
ssh-keygen -t rsa
```
(accept the defaults by hitting enter repeatedly.)

Once you're back at the shell prompt, display your ssh public key by running:
```
cat ~/.ssh/id_rsa.pub
```
and you should see output like this:
>ssh-rsa AAAABxyz3NzaC1yc2EAAAADAQABAAABgQDEq4XY2EsXabESDEhc0I8viAVN1yp+jfogvdjJ55VocZH066F4BJJDxQtFDnonsense/tB4Q2MzPin5eneXSVP8u6ZUOMd+1/g19kAWT0jwa3ppg6G56Xix0PGlxdrv7XJdUWBqGCkNRdpgpm9qbZhejOOMrVK2lX8qEPvuPveDXi6lHhe3Mkai1GYV6mWbOiwi5QS+Hpp1rtBu5PWoCiHY/OF7rCBkkPCPMIuP+1O6uVDUmlfLPRx+lutPaq3OmGYmoh9HOc= USERNAME@farm

Select and copy this entire string (from `ssh-rsa` all the way through `USERNAME@farm` at the end.

Now, go to your github account settings, and find "SSH & GPG Keys."

Select "New SSH key".

Paste the ssh-rsa key string you copied above into the "Key" box.

For title, put something like "farm account key".

Save it.

## Cloning your GitHub repository with an ssh key


Go back to the Terminal in RStudio.

Then type:
```
cd ~/
```
to make sure you're in your home directory.

Type 'git clone' and then paste your repository ssh URL after it, e.g.
```
git clone git@github.com:ngs-docs/2025-ggg-201b-hw1-USERNAME/
```
You can get the repository ssh URL from the green "Code" dropdown on the main page of your github repository (or just edit the URL above by replacing `ctb` with your own github account name).

You should, computers willing, now have a copy of your github repository locally.

## Edit and run the Snakefile

You have been gifted three extra data sets! Let's run variant calling on them!

On farm, in `~ctbrown/data/ggg201b`, there are four FASTQ files:

```
SRR2584403_1.fastq.gz
SRR2584404_1.fastq.gz
SRR2584405_1.fastq.gz
SRR2584857_1.fastq.gz
```

All four of them are resequencing data sets for E. coli REL606; we've been working on the last one in class.

Next, call variants for all four data sets using the pipeline we've been running in class (which is on your hw1 directory).

To do this, change into your hw1 directory and edit the Snakefile as in [the lab 3 instructions](https://hackmd.io/8MUDxb3FTSqMod9Q9LHKWA?view#Adding-a-sample-to-the-Snakefile) (which we will go through in lab 4, sorry ;).

Please add the three extra samples to the Snakefile and run snakemake.

Note: You'll probably need to copy in some files in order to get rnakemake to run... and you will need to activate your `mapping` conda environment.

## Create `hw1.txt` and push changes

Create a new text file in RStudio, and save it to the name `hw1.txt` in your hw1 directory.

(You can also run `touch hw1.txt` in the hw1 directory, and then click on it in the file browser to edit it.)

Answer the following homework questions in this file:

- which variants are present in which samples?
- which variants are shared between two or more samples?

Name variants by location and give both the reference and sample alleles as in the VCF file.

Save the file.

At the terminal prompt, make sure you are in the hw1 working directory and add `hw1.txt` to git with:
```
git add hw1.txt
```

Then type
```
git commit -am "hw1"
```
to commit the changes. (If it says "nothing to commit", then you probably forgot to save the file in RStudio.)

Now run:
```
git push
```
and it will send a copy of your changes back to GitHub. Success! You've (probably) turned in your homework!

Note that you can repeat the above loop (edit, save, commit, push) as many times as you need. You only need to run add once, but you can run it more times if you like.

## Trust but verify

Go to your github repository on the Web site (e.g. https://github.com/ngs-docs/2025-ggg-201b-hw1-ctb/) and verify that `Snakefile` and `hw1.txt` on there is correct.

Congratulations! You've (definitely) turned in your homework!

## Need help?

See the canvas page / homework announcement for details on office hours etc. You can also e-mail Titus at ctbrown@ucdavis.edu.
