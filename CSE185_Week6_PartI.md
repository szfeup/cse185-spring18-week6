# Week 6: Proteomics (part 1)
Skills covered: Advanced Git features, mass spec analysis

This week, to give you a small break before we go into final project mode, you won't have to write a lab report. But you will have to go through the lab tutorials and fill in your lab notebook as usual. Instead of writing a report, answer the bolded questions in a worksheet file in your Github repository (see instructions below).

The goal of this week is to get a gentle introduction to various aspects of protein bioinformatics. Most of our work for this lab will involve exploring datasets using graphical user interfaces (GUIs). Today, you will explore some mass spectrometry data from proteins that bind to the telomere regions of chromosomes. On Thursday, we'll explore and compare the structure of various proteins as they have evolved over time.

## 0. Forking the Git repository

This week we'll be working with Git repositories outside of the Github classroom interface, more like you would in the real research world.

One of the primary features of Git is the ability to have different "branches", or versions of the code being edited simultaneously, perhaps by different people, and then merged back together again when each person is done working on whatever they were doing. The main line is called the "master" branch, but additional branches can diverge from master and be branched in later. See [this helpful tutorial](https://medium.com/@igor_marques/git-workflow-basics-d405746f6205).

Let's first start off by clarifying some Git terminology:

* **Repository**: can be thought of as like a folder that contains all elements for a certain project. For our class, we have had one repository for each assignment.
* **Clone**: is like a copy of a repository. You make a clone of a repository to create a copy that lives on your computer rather than on the Github server. You can "push" local changes to the main Github repository to keep the clone up to date with what's on Github, as we've been doing in previous labs.
* **Fork**: is your own copy of another user's repository. When you "fork" someone else's repository, you create a copy on your own account. This allows you to make changes to your own copy. When you are ready to merge changes with the main repository, you can submit a "pull request" to the original author to update the main repository with your changes. You can also "fetch" updates from the original repository to keep yours up to date. We'll get experience with both of these things this week.

This week, you will create a "fork" of the main assignment repository to make your own local copy. You can do this by going to the the repository on Github (https://github.com/gymreklab/cse185-spring18-week6), and clicking the "Fork" button on the top right. Make sure you are signed in to Github before you do this. Wait a couple seconds and your own copy will be made (https://github.com/\<username>/cse185-spring18-week6).

You can now create a "clone" of this repository as we have done in previous weeks. On `ieng6` in your course home directory, run:
```
git clone https://github.com/<username>/cse185-spring18-week6
```

You can commit, push, and pull just like you have done in the past. Put all of your answers in the file `CSE185_Week6_Worksheet.md`. Although as usual there will be no "submission" process, you should have your worksheet done by 11am next Tuesday (May 14).

A couple of notes before moving on:

* Everyone's repositories are public. If you wanted, you could go look up all of your friend's answers by going to their copy of the repository. Although you may work together, **you are not allowed to go poking around other people's repositories for the assignment**. We're using the honor system here and trusting you.
* You'll notice the Thursday instructions aren't in the repository yet. The main repository will be updated on Thursday. The first section of Part 2 will be learning how to "fetch" these changes to update your fork.

## 1. Exploring the PRIDE database using the PRIDE inspector
Just like NCBI hosts the SRA for next generation sequencing data, EMBL-EBI (the European Molecular Biology Laboratory - European Bioinformatics Institute), hosts the PRIDE database for proteomics data from mass spectroscopy. Most proteomics data on PRIDE is stored in some version of an XML format, which is not easy to read directly. So we will use PRIDE’s in-house developed tool, called PRIDE Inspector, for viewing the data in a graphical user interface.

You can download the PRIDE inspector here: https://github.com/PRIDE-Toolsuite/pride-inspector.

## 2. Inspect the raw XML data file

We will be working with the file `public/week6/telomere_ms.mzXML`, which contains MS/MS spectra from proteins isolated from telomere regions. Our goal is to identify which proteins were contained in this mixture. Use:

```
less -S PATH/TO/telomere_ms.mzXML
```
to scroll a bit through this file. This is in an XML format, which be difficult for humans to read. If you've used HTML before some of the syntax of this file might be familiar. This is a particular XML format called [mzXML](https://www.nature.com/articles/nbt1031) developed specifically for representing mass spectrometry data.

The top contains a bit of metadata bout the file, and then there are individual entries for each peptide that entered the mass spec, which is reported as a scan. Because this is tandem MS data, there were two different scan levels. Level 1 reports the m/z peaks for fragments that resulted when the peptide was first ionized to convert it to the gas phase (these fragments are often called "precursors").

In this experiment, the top 9 most abundant peaks from the first scan were subjected to additional fragmentation into smaller pieces with a process called CID (collision induced dissociation). One m/z peak at a time, each group of precursor fragments is accelerated and then the pressure is increased so that the gaseous ions collide with one another and break up. These new, smaller fragments are scanned again in Level 2, to give additional information about each original peptide.

In each reported scan, the m/z ratio and the corresponding intensity (which is proportional to abundance) of each fragment is reported as a sequence of ASCII encoded pairs.

Answer the questions below in the worksheet by manually inspecting the file:

**What level is the first scan?**

**How many peaks are in the first scan?**

**How many scans are in the entire file?** (Hint, `cat telomere_ms.mzXML | grep "scan num="` will be helpful)

## 3. Load PRIDE Inspector and visualize spectra

Now switch over to the PRIDE inspector. Go to Open -> Open Files -> and navigate to the file `public/week6/telomere_ms.mzXML`. It may take a minute or two to load.

Click on the "spectrum" tab. The bottom window shows a list of every scan from this sample (only the first 100 are shown, you can load more in 100-scan batches with the load more button). The top window shows the spectra corresponding to that scan. Each peak represents an individual fragment. The y axis shows its abundance, and the x-axis shows its mass/charge ratio. Use pride inspector (you may have to look around the window or try other tabs) to answer the following questions:

**In the first spectrum that loaded (spectrum ID 4970), what is the m/z ratio of the most intense peak? (You can see this by hovering your mouse over that peak).**

**In the summary tab, compute the summary charts. Then: what are the two most abundant precursor ion charges?**

## 4. Convert and filter spectra

The pride inspector lets us visualize the raw data, but to understand which peptides these fragmented ions correspond to, we need to search them against a database of predicted fragmentation spectra for known proteins.

The database we will use is called mascot. It is a free, online server, but it requires a specific format and can only handle 1200 scans at time. Ideally, we would search all of our spectra, but to reduce the size of our file, we will take a subset of spectra with an intermediate number of peaks. The hope is that the data will be rich enough to reconstruct individual peptide sequences, but not dominated by noise from 100s of low-abundance fragments.

We will use the tool `msconvert` from the ProteoWizard suite to convert our data to the correct format and extract only a fraction of the spectra:

```
./msconvert telomere_ms.mzXML --filter "index [0,1000]" --mgf -e 300_500.mgf 
```

This filters our data to keep only scans 0-1000. This will output a file (in a new format!) `PRIDE_Exp_Complete_Ac_31251.pride300_500.mgf`, which is a simpler format that mostly contains a list of peaks for each scan. This file can be both analyzed using PRIDE and used to search databases using the Mascot tool.

Go back to the PRIDE inspector and load our filtered `.mgf file.

**How many spectra are in our filtered file?**

## 5. Search spetra against protein digest databses with mascot

In a web browser, go to the mascot database at http://www.matrixscience.com. Find the free search, then navigate to the MS/MS ions search. Enter:

* Your name, email, and a search title.
* Select the SwissProt database, as well as "contaminants".
* Set the taxonomy to Mus Musculus.
* Set the digestion enzyme to trypsin
* Allow up to 5 missed digestion sites
* Add the following fixed modifications: Carbamidomethyl (C)
* Add the following variable modifications: Acetyl (N-term), Oxidation (M)
* Set peptide mass tolerance to 10 ppm.
* Set Fragment mass tolerance to 0.8 Da.
* Set peptide charge to 2+ and 3+
* Check monoisotopic
* Set data format to mascot generic
* Upload your filtered `.mgf` file.
* Start search (may take 2-3 minutes)

## 6. Analyze mascot results

Your search results will include some summary information at the top, and the blue-ish chart will show you the threshold for significance based on how well your spectra agree with the listed peptide sequence.

Beneath this, in the Protein Family Summary, go to the "report builder". You will see a numbered list of protein hits, with columns giving the progein name, match score, and additional info. Click on a family number to see more details about the peptide(s) that were identified for each protein.

**The top hit is a contaminant! What is it? Give one sentence explanation of why this shows up in our list.**

**Sort the list by score. List the top 5 scoring protein hits (not contaminants!)**

**How many peptides were found to match the top hit, NUCL_MOUSE (Nucleolin)?**

**Briefly investigate the function of the top 5 genes. Are any known to be related to telomere function?** (3pts)

***That's it for today! Next time we'll fetch the instructions for part II and learn how to visualize protein structures. The beginning of part 2 is below in Section 7, but don't start working on that until Thursday.***

## 7. Fetch instructions for Part 2

The second part of this week's tutorial has been added to the main repository, see: https://github.com/gymreklab/cse185-spring18-week6/blob/master/CSE185_Week6_PartII.md

Before we start let's fetch those changes to our fork. From your `cse185-spring18-week6` folder on `ieng6`, do:
```
# Configure a "remote" to point to the original repository (you only ever have to do this once)
git remote add upstream https://github.com/gymreklab/cse185-spring18-week6/

# Fetch changes from "upstream", which is the repository you forked from
# and merge those into your local master branch (do this every time you need to update changes
# from the original repo)
git fetch upstream
git checkout master
git merge upstream/master

# Push the changes to Github, then go to the repository on Github to see that the new Part II file is there.
git push
```

Once you see the Part II tutorial, continue working from that document.
