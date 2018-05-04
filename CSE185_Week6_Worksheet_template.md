# CSE 185 Lab 6 Worksheet

This worksheet is worth the same amount as a lab report (based on the percent of points scored for each question below scaled to a 10 point scale). 

## Part 1: Mass spectrometry

**What level is the first scan?** (1pt)
msLevel=2

**How many peaks are in the first scan?** (1 pt)
205

**How many scans are in the entire file?** (1 pt)
21409

**In the first spectrum that loaded (spectrum ID 4970), what is the m/z ratio of the most intense peak? (You can see this by hovering your mouse over that peak).** (1pt)
555.056

**In the summary tab, compute the summary charts. Then: what are the two most abundant precursor ion charges?** (1pt)
2,3

**How many spectra are in our filtered file?** (1pt)
1,001

**The top hit is a contaminant! What is it? Give one sentence explanation of why this shows up in our list.** (1 pt)
Trypsin. This enzyme is used during the mass spec process to fragment proteins (trypsinization)

**List the top 5 scoring protein hits (not contaminants!)** (1 pt)
NUCL, DHX9, TR150, EF1A1, PRP8

**How many peptides were found to match the top hit, NUCL_MOUSE (Nucleolin)?** (1pt)
7

**Briefly investigate the function of the top 3 genes. Are any known to be related to telomere function?** (3pts)
Nucleolin - http://www.jbc.org/content/279/49/51508.full
DHX9 - https://www.ncbi.nlm.nih.gov/pubmed/24990949
TR150 - https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4889927/


# My link
http://www.matrixscience.com/cgi/master_results_2.pl?file=..%2Fdata%2F20180503%2FFTmmffcmE.dat;pr.show=reportbuilder;qo.sort=score;qo.sortdir=desc;report=50;sessionID=guest_guestsession

## Part 2: De novo peptide sequencing

**How many peaks are in this file? What is the range of m/z ratios? (i.e. min and max). How does the max ion mass here compare to the precursor ion mass? What is the most abundant m/z ratio?** (4pts)
1108 peaks; min=279.199249, max=1958.194306; max is about same as the precursor ion mass; 768.640625 is most abundant peak with abundance=7950.795898

**Include your table in your worksheet. Explain your answer for at least two more amino acids.** (3 pts - 1 for including table, 2 for explanation of at least 2 more AAs)

**This was pretty tedious! If you were going to write a program to automate this process, how would you do it? (max 2-3 sentences about your idea.** (2 pts)
Many possibilities:

* try all possible peptide sequences and compare spectra (would take forever though! 18^20 possibiolities...
* use shifts to guess as many AAs as possible then do something like the above
* match to database of spectra for known peptides

**What is the op scoring sequence? How close is it to your guess? Note: neither answer is probably completely correct!** (1 pt)
PQELLSQLVQYTGK
Not super close...
Might be missing some modifications or using incorrect model

**Hypothesize why some peaks are missing from our spectrum file.**

Not all fragments are created with equal probability
Some randomly dropped out of the experiemnt?
Biology is noisy...

**Suggest a way we could modify our mass spec experiment to distinguish between amino acids with near identical masses, such as Isoleucine and Leucine.**
If we had a way to add a PTM to one of them so it had a different mass, that could help.