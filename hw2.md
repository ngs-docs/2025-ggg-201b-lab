---
tags: ggg, ggg2025, ggg201b
---

# GGG 201(b), Lab Homework 2 - 2025

([Permanent link on github](https://github.com/ngs-docs/2025-ggg-201b-lab/blob/main/hw2.md))

Due by 10pm, Thursday Feb 27th.

I will have office hours in DataLab/Shields 362 at these times:
* 2:45-3:45pm - Monday, Feb 24
* 11:30am-12:30pm - Tuesday, Feb 25
Happy to zoom in that time frame as well, or to arrange zoom sessions at some other time.

Please note that you may freely work together with others, but you must hand it in separately.

---

## Goals

This homework starts from [Lab 5/6](https://hackmd.io/lXft6jRiRIq9Vdh8OKACKg?view) to do a bit more with assembly.

The goal of this homework is to examine and evaluate at _what coverage_ genome assembly collapses. To this end, you will calculate assemblies for several different subset sizes of reads using a Snakefile, and report the result to a central form.

Warning: the computation here will take about 20-30 minutes to run once you have the Snakefile all set up.

## 1. Connect to GitHub Classroom and clone the hw2 repository

As per [Lab homework #1](https://hackmd.io/56wVY5twSiGR4gSZ0whz3w), connect to [GitHub classroom for HW #2](https://classroom.github.com/a/Z8lRAyCw) and clone this assignment's repository to farm.

You'll also need to [copy in the data files](https://hackmd.io/lXft6jRiRIq9Vdh8OKACKg?view#Lets-run-an-assembler).

## 2. Edit the Snakefile and save to GitHub

Update the Snakefile to calculate at least three different subset assemblies, choosing estimated coverages between 2x and 60x. The default target for the Snakefile should start from a directory containing only the two read files (`SRR2584857_1.fastq.gz` and `SRR2584857_2.fastq.gz`) and the Snakefile, and compute all three assemblies plus their quast statistics and prokka annotations.

Get the Snakefile working, verify that it works in an empty directory (using `--delete-all-output`), and then commit and push, as in homework 1. Please do not  add the read data files or any other files into the git repository or GitHub, thanks!!

If you work with others, please select different subset sizes between all of you. Thanks!! (It won't take any extra time ;)

## 3. Submit assembly results to this Google form

Submit at least three _different_ assembly entries to [this form](https://docs.google.com/forms/d/1t_OhR80Of64k_tCoDvUjkT6aK1hyYmS_osQGGOuQn6Q/preview), which will ask for the following information:
1. Your GitHub ID.
2. The filename of one assemby produced by your Snakefile.
3. The number of reads used in the assembly.
4. Your estimate of the coverage (use 4.5 Mb as the genome size).
5. The N50 of the assembly.
6. The total bp in contigs > 1kb for the assembly.
7. The total number of contigs > 1kb for the assembly.
8. The total number of protein coding genes (records in the annotation .faa file) for the assembly.

Note that the total number of lines in a single gzipped FASTQ file can be calculated like so:
```
gunzip -c FILENAME | wc -l
```

and the total number of records in a FASTA file (e.g. the .faa file output by prokka) can be calculated by doing
```
grep ^'>' FILENAME | wc -l
```
You can inspect the records in a FASTA file with
```
gunzip -c FILENAME | head
```
which will let you calculate the sequence length - note that all the sequences in the FASTQ file are the same length.

## 4. Relax in knowledge of a job well done.

Please do inspect the Snakefile on github via the Web interface to make sure it has all your changes!

**You must turn this in via GitHub, and I will primarily be looking at GitHub for grading.**

## Questions? Problems?

If you run into any trouble with submission, that's ok - just let me know.

You can reach me via e-mail at ctbrown@ucdavis.edu, or in UC Davis slack under @ctitusbrown.

## Appendix - snakemake details

If you're interested in understanding some of what's going on in the Snakefile, here is some reading!

### wildcards and expand

General background reading on what we're doing with wildcards in this Snakefile: [Wildcards are determined by the desired output](https://ngs-docs.github.io/2023-snakemake-book-draft/beginner+/wildcards.html#wildcards-are-determined-by-the-desired-output).

### "|| true"

The `|| true` in the `subset_reads` rule is explained in this draft chapter: [Never fail me - how to make shell commands always succeed](https://ngs-docs.github.io/2023-snakemake-book-draft/recipes/never-fail-me.html?highlight=exit%20codes#never-fail-me---how-to-make-shell-commands-always-succeed).

### `{subset}` - how it works and what it's used for

In the `subset_reads` rule, there's some tricky stuff with `{subset}` being used to specify the number of lines being extracted with `head`. See [Using wildcards to determine parameters to use in the shell block](https://ngs-docs.github.io/2023-snakemake-book-draft/beginner+/wildcards.html#using-wildcards-to-determine-parameters-to-use-in-the-shell-block) for an explanation of this.