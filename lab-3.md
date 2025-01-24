---
tags: ggg, ggg2025, ggg201b
---

[toc] 

# GGG 201b WQ 2025 - Lab 3, Links and info - Jan 24, 2025

Reminder: [collaborative notes / Q&A](https://docs.google.com/document/d/16pZESeGgcJvrHy_nksiaY25HFcaUh8McvhUDYLelL88/edit?tab=t.0). No question is off topic :).

## What did we do last lab?

[Lab 2 link](https://hackmd.io/YNa_9884S6SiUcahlNkYLA?view)

## Start RStudio

Connect to https://ondemand.farm.hpc.ucdavis.edu/ and log in.

Select RStudio.

Adjust the following entries:
* Account: ctbrowngrp
* Partition: low2
* Number of cores: 2
* Amount of memory: 5
* R version: r-4.3.3
* Number of hours: 2

Then select "Launch".

Once RStudio says it's running, connect to it and locate the Terminal. (Hint: middle left, a tab next to Console).

## Starting from scratch - the short version

:::danger
Ignore this if you successfully ran the things in Lab 2!)
:::

Run the following - you can copy/paste the whole thing into the Terminal and hit Enter:
```
# get the software
cd ~/
git clone https://github.com/ngs-docs/2025-ggg-201b-variant-calling/
cd ~/2025-ggg-201b-variant-calling

# copy reads & reference genome
cp ~ctbrown/data/ggg201b/SRR2584857_1.fastq.gz .

cp ~ctbrown/data/ggg201b/ecoli-rel606.fa.gz .

# install software and activate software environment
module load mamba
mamba create -y -n mapping minimap2 samtools vcftools bcftools snakemake-minimal
mamba activate mapping

# run the workflow
snakemake -p
```

## Picking up from where we left off

### Getting set up.

:::danger
Start here if you successfully ran the things in Lab 2.)
:::

```
module load mamba
mamba activate mapping

cd ~/2025-ggg-201b-variant-calling
```

### What happens when you run `snakemake` again?

Try running:
```
snakemake -p
```

What happens?

Try:
```
snakemake --delete-all-output
snakemake -p
```

### Look at the variants

Look at all the variants:
```
grep -v ^# outputs/SRR2584857_1.x.ecoli-rel606.vcf
```

Look at the full pileup of one the variants:
```
samtools tview outputs/SRR2584857_1.x.ecoli-rel606.bam.sorted outputs/ecoli-rel606.fa -p ecoli:3931002
```

Note that the VCF file is basically a _statistical summary_ of the tview.

## What does the workflow do?

### Boxes and arrows - how I think about workflows

Each box "takes in" one or more files, and outputs one or more files.

Each box is an isolated "step" (of one or more commands).

### How does the Snakefile work?

The [Snakefile](https://github.com/ngs-docs/2025-ggg-201b-variant-calling/blob/main/Snakefile) is written in Python syntax.

It contains "recipes" for constructing files.

The first "recipe" (`make_vcf`) is the request for the final product.

Anything in `{` and `}` is a "wildcard" that says, "replace me with the same set of values". Values operate only within each rule.

### What are the commands?

What does
```
snakemake -p
```
tell you?

Try:
```
snakemake --delete-all-output
snakemake -n
```

## Adding a sample to the Snakefile

The line
```
SAMPLES = ["SRR2584857_1"]
```
is a list of samples. Right now there's only one. We can add another - try editing this file in RStudio, and changing this line to:
```
SAMPLES = ["SRR2584857_1", "SRR2584403_1"]
```

And then running snakemake. What happens? Do we need to do anything else to make this work?

## Adjusting the Snakefile

Let's supposed we decide that the variant calling pipeline is too strict - we want to see MORE variants, if there are any to see. How might we adjust the Snakefile?

* where/what command?
* how do we change it?
* how do we get new results?
* how do we see what is new?



## Choosing your reference genomes

Questions were asked!

* When you find multiple reference genomes for a species, how to know which one is the best to use?
* How does a "reference" genome differ from the genome for a specific individual?
* In the case of multiple reference genomes of the same strain, can we combine the existing fastq data that made the multiple reference genomes to create a new reference genomes?

## How do you figure out the molecular impact of the variants?

Most variants are neutral and get fixed in the population by happenstance (either they "hitch-hike" with advantageous variants, or just b/c of stochasticity.)