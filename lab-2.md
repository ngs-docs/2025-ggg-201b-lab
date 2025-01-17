---
tags: ggg, ggg2025, ggg201b
---

[toc] 

# GGG 201b WQ 2025 - Lab 2, Links and info - Jan 17, 2025

Today, we will run an actual variant calling workflow on a single file. This involves many steps!

* Reserving compute resources;
* Locating some data;
* Choosing a reference genome;
* Installing some software;
* Copying a workflow from an online repository;
* Running the basic mapping of reads to reference;
* Converting and indexing the results of the mapping;
* Summarizing the mapping and calling variants;
* Inspecting the called variants;
* Summarizing the likely effects of the variants;

Let's see how far we can get!

If it's overwhelming, that's ok. Relax, get what you can out of it, and write down your questions!

### Collaborative Q&A, and notes

This is going to be a pretty bare bones presentation of the overall workflow today - and I'm sure you'll have questions! Please put them [in this google doc](https://docs.google.com/document/d/16pZESeGgcJvrHy_nksiaY25HFcaUh8McvhUDYLelL88/edit?tab=t.0) and I can make sure to address them. No question is off topic :).

https://hackmd.io/YNa_9884S6SiUcahlNkYLA

<!-- ## Step 0: Visualizing the workflow -->

## Step 1: Start up RStudio

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

Flag me down if you run into any problems during any of this :).

Once RStudio says it's running, connect to it and locate the Terminal. (Hint: middle left, a tab next to Console).

:::success
**You should be at a prompt that ends with `$` and looks something like `ctbrown@cpu-6-92:~$`. Hit Enter a few times and make sure it responds ;)**.
:::

Resources and more information:

* Documentation on the slurm queuing system: [link to my GGG 298 tutorial from 2023](https://hackmd.io/p_d88X7HTFSXLmjB0fvuug?view)
* [How do HPCs work? An extended office-building/co-working space analogy](https://hackmd.io/L_ElguVkS7a_7iXTDBv06g?view#Digression-how-HPCs-work)
* We are using the `low2` "partition" or queue because we can "borrow" resources from across the entire cluster - but we are limited to jobs under 4 hours.

## Step 2: Copy workflow from GitHub

Copy and paste the below to the Terminal and hit Enter:
```
cd ~/
git clone https://github.com/ngs-docs/2025-ggg-201b-variant-calling/
cd ~/2025-ggg-201b-variant-calling
```

## Step 3: Copy FASTQ read data into our directory

```
cp ~ctbrown/data/ggg201b/SRR2584857_1.fastq.gz .
```

Q: How big is the file? What's in it?

We can look at this data with:
```
gunzip -c SRR2584857_1.fastq.gz  | head
```

## Step 4: Copy a reference genome into our directory

```
cp ~ctbrown/data/ggg201b/ecoli-rel606.fa.gz .
```

Q: How big is the file? What's in it?

Again, we can look at .gz files like so:
```
gunzip -c ecoli-rel606.fa.gz | head
```

Q: where do reference genomes come from??

## Step 5: Install software using conda

Load the mamba software installation system, and use it to install the necessary software:
```
module load mamba
mamba create -y -n mapping minimap2 samtools vcftools bcftools snakemake-minimal
mamba activate mapping
```

:::info
NOTE: you only need to run the `create` step once for each named environment (here, `mapping`). Afterwards, you can use that environment by running:
```
module load mamba
mamba activate mapping
```
:::

Q: How do you know what software to install?

## Step 6: Run the basic mapping of reads to reference; summarize mapping & call variants

```
snakemake -p
```

Q: What is this workflow running?

See: [the Snakefile on github](https://github.com/ngs-docs/2025-ggg-201b-variant-calling/blob/main/Snakefile)

## Step 7: Inspect the called variants

The net result of this workflow is this file:
```
grep -v ^# outputs/SRR2584857_1.x.ecoli-rel606.vcf
```

Q: how do we know that this is the end result of this workflow?

Q: What are the columns in this file?

To look at the variants, run:
```
samtools tview outputs/SRR2584857_1.x.ecoli-rel606.bam.sorted outputs/ecoli-rel606.fa -p ecoli:<location>
```
replacing `<location>` with the location of a region of interest.


## Step 9: Think about the possible (molecular) impacts of the variants

Which of these variants is likely to impact coding sequence? How would we tell?

## Step 10: Revisit what we did

Today we:

* logged into farm
* reserved compute resources for ourselves with srun
* downloaded some data
* installed some software with conda
* grabbed a simple variant calling snakemake workflow off of github
* ran the mapping part of the workflow
* explored the mapping output

Whew, this was a lot!

## Step 11: Celebrate

We ran something successfully! (hopefully.)

Next week we'll retrench a little bit, revisit some of these commands, and explore the concepts of shotgun sequencing and mapping, before moving on to talking about variant calling.

## Resources for you



