---
tags: ggg, ggg2025, ggg201b
---

[toc] 

# GGG 201b WQ 2025 - Lab 4, Links and info - Jan 31, 2025

Reminder: [collaborative notes / Q&A](https://docs.google.com/document/d/16pZESeGgcJvrHy_nksiaY25HFcaUh8McvhUDYLelL88/edit?tab=t.0). No question is off topic :).

## What did we do last lab?

[Lab 3 link](https://hackmd.io/8MUDxb3FTSqMod9Q9LHKWA?view)

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

## Picking up from where we left off

### Getting set up.

```
module load mamba
mamba activate mapping

cd ~/2025-ggg-201b-variant-calling
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
