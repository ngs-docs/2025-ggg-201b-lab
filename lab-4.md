---
tags: ggg, ggg2025, ggg201b
---

[toc] 

# GGG 201b WQ 2025 - Lab 4, Links and info - Jan 31, 2025

Reminder: [collaborative notes / Q&A](https://docs.google.com/document/d/16pZESeGgcJvrHy_nksiaY25HFcaUh8McvhUDYLelL88/edit?tab=t.0). No question is off topic :).

## Checking in: linking your homework on github

Could people give this a try:

[link github to hw](https://hackmd.io/56wVY5twSiGR4gSZ0whz3w?view=#Log-into-githubcom)

and let me know if they can't do it? I can end class a bit early to help people trouble shoot.

Also: office hours in DataLab:

* 2:45pm-3:45pm on Monday, Feb 3th (Shields 362)
* 3pm-4:30pm on Tuesday, Feb 4th (south side of DataLab, by main entrance)

## Note: lab 5 / Friday feb 7 / will be taught by Dr. Dani Soto

She will be teaching remotely from Chile. I have someone ready to set up zoom in the classroom - shall I do that? ("Yes" is a fine answer!) Please fill out [this form](https://docs.google.com/forms/d/1yIb4Cagt89TbCcZyKED04miftjCJoIDtL5xDGz8i7z4/preview) if you plan to attend in person (note: it's anonymous ;).

(I will be mildly peeved if people say yes, then decide not to come and don't tell me. You can change your mind to "no" and drop me a note via e-mail or canvas anytime before Thursday and I won't be upset!)

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
```

then:
```
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

And then running snakemake:
```
snakemake -p
```
What happens? Do we need to do anything else to make this work?

Hint: look at [what we did in previous labs](https://hackmd.io/8MUDxb3FTSqMod9Q9LHKWA#Starting-from-scratch---the-short-version).

::::spoiler
Fix the error by copying in the samples file:
```
cp ~ctbrown/data/ggg201b/SRR2584403_1.fastq.gz .
```
::::

Would this have worked if we weren't using snakemake?

### How do we look at the results of the new variant calling?

## Adjusting the Snakefile, round 2

How do we fix the "ploidy" error?

::::info
**Specific question:**

How do we modify line 65 of the Snakefile to avoid the "ploidy" error that is output by `bcftools call`?

```
Note: none of --samples-file, --ploidy or --ploidy-file given, assuming all sites are diploid
```
::::

Note: to force re-running of any one step, we can delete a file and then ask for that file to be rebuilt. For example,

```
rm outputs/SRR2584857_1.x.ecoli-rel606.bcf
snakemake outputs/SRR2584857_1.x.ecoli-rel606.bcf
```
will rerun whatever step(s) are necessary to rebuild the BCF.

You can force re-running of all steps by doing:
```
snakemake --delete-all-outputs
```

## Adjusting the Snakefile, round 3

Let's supposed we decide that the variant calling pipeline is too strict - we want to see MORE variants, if there are any to see. How might we adjust the Snakefile?

* where/what command does the actual variant calling?
* how do we change it that command? what do we change it to?
* how do we get new results?
* how do we see what is new?

## Side note: VCF vs gVCF

You will not want to use VCF files for cohort variant calling (as you will use in 201D). Why? You want to use a **gVCF file**.

From [this link](https://gatk.broadinstitute.org/hc/en-us/articles/360035531812-GVCF-Genomic-Variant-Call-Format#:~:text=The%20key%20difference%20between%20a,a%20cohort%20in%20subsequent%20steps.):

>The term GVCF is sometimes used simply to describe VCFs that contain a record for every position in the genome (or interval of interest) regardless of whether a variant was detected at that site or not (such as VCFs produced by UnifiedGenotyper with --output_mode EMIT_ALL_SITES). GVCFs produced by HaplotypeCaller in GATK versions 3.x and 4.x contain additional information that is formatted in a very specific way. Read on to find out more.

>GVCF files produced by HaplotypeCaller from GATK versions 3.x and 4.x are not substantially different. While we don't recommend mixing versions, and we have not tested this ourselves, it should be okay to use GVCFs made by different versions if the annotations and the GVCFBlock definitions (see below) are the same.

## Evaluating mapping

We'll evaluate mapping a bit more in two weeks, when we start talking about assembly, where it's useful for evaluating the quality of the assembly. For now, let's start by looking at the coverage of what mapped.

```
samtools coverage outputs/SRR2584857_1.x.ecoli-rel606.bam.sorted
```

You should see:
```
#rname  startpos        endpos  numreads        covbases        coverage       meandepth        meanbaseq       meanmapq
ecoli   1       4629812 2116548 4623406 99.8616 46.1237 35.2    58.5
```
Here the two numbers to pay attention to are:
* coverage: 99.8616
* meandepth: 46.1237

This means that 99.9% of the known reference genome had at least one read mapping to it ("coverage"), and the mean number of reads mapping to each base in the known reference genome is 46.  This is pretty darn good sequencing depth, and it means that the sample matches the reference really well, also.

BUT. We may also have things that are _new_ in the sample and don't match to the reference genome. Where would we find those?

## Evaluating mapping stats

Run:
```
samtools flagstat outputs/SRR2584857_1.x.ecoli-rel606.bam
```

What fraction of the reads mapped?

## Getting the unmapped reads

We can extract the unmapped reads:
```
samtools fasta -f 4 outputs/SRR2584857_1.x.ecoli-rel606.bam > unmapped.fq
```

...but what do we do with them?

## Bonus: identifying what's in a set of reads, without using assembly or choosing a specific reference genome

Suppose you had a file full of reads, and you wanted to know what was in them quickly. There's a tool for that!


Try running:
```
mamba create -n smash -y sourmash_plugin_branchwater
mamba activate smash
```

Now convert your read file into k-mers:
```
sourmash scripts singlesketch SRR2584857_1.fastq.gz \
    -p k=51,scaled=1000,abund \
    -o SRR2584857_1.sig.zip
```

And search those k-mers against every known organismal genome (Terms And Conditions May Apply):
```
sourmash scripts fastmultigather SRR2584857_1.sig.zip \
    /group/ctbrowngrp5/sourmash-db/entire-2025-01-21/entire-2025-01-21.rocksdb \
    -k 51 -s 10_000 -t 0 \
    -o SRR2584857_1.x.entire.csv
```

If you open the resulting CSV file and look at the `match_name` column, you'll see 'E. coli REL606'.

Why is this useful?

## Let's look at the reads that didn't map!

Convert your unmapped read file into k-mers:
```
sourmash scripts singlesketch unmapped.fq \
    -p k=51,scaled=1000,abund \
    -o unmapped.sig.zip
```

And, again, search those k-mers against every known organismal genome:
```
sourmash scripts fastmultigather unmapped.sig.zip \
    /group/ctbrowngrp5/sourmash-db/entire-2025-01-21/entire-2025-01-21.rocksdb \
    -k 51 -s 10_000 -t 0 \
    -o unmapped.x.entire.csv
```
What do we find?

(Yes, this is a surprise to me, too)
