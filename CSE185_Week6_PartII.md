# Week 6: Proteomics (part 2)
Skills covered: More Git features, protein structure visualization

Today we'll get some practice visualizing protein structures and using structure-based phylogenetics approaches to study the history of tRNA synthetases, the enzymes responsible for delivering amino acids to ribosomes to be added to a protein.

## 8. Access GUI-based software on ieng6 server with VNCgnome

The software we will use this week analysis, VMD (Visual Molecular Dynamics), requires a graphical user interface (because the data is primarily graphical).

To access it on the `ieng6` servers outside of lab, you will have to connect though Virtual Network Computing to a GNOME (GNU Network Object Model Environment) desktop for the `ieng6` server that you can control and view at your workstation. Once you have the gnome desktop set up, if you need to work on things that don’t require the GUI, you can continue working at the command line in your original terminal window.

### Mac VNCgnome

* Log into one the `ieng6-###` servers listed [here](https://acms.ucsd.edu/info/vncgnome.html) using a tunnel:
```
ssh -X -L 5899:localhost:5900 username@ieng6-###.ucsd.edu
```
* Run
```
vncserver
```
* connect to the vnc client by clicking Finder>Go>Connect to server, and enter:
```
vnc://localhost:5000
```

This will open the gnome window (you will be prompted for your password). If you need to stop the
vnc session, type `vncserver -kill`.

### Windows VNCgnome
Follow the directions here:
http://academy.eng.ucsd.edu/spis/spis_2013/introcs/remote-access

## 9. Launch VMD and multiseq, configure multiseq
going to the gnome desktop, and launching VMD inside the gnome desktop by clicking applications > system tools > terminal then type:
```
vmd
```

This will launch two windows: the OpenGL display window, and then VNC main window. In the VNC main window, click: Extensions > Analysis > MultiSeq. Click OK then cancel to let multiseq choose a default directory for metadata storage, or specify a `week6` folder in your working directory. You should end up with an empty untitled multiseq window, which you can place over the VNC main window, since the remainder of the tutorial will be carried out in MultiSeq.

Configure MultiSeq for BLAST searches by pointing MultiSeq to the location of blast in our `public/bin`. In MultiSeq window: File > Preferences > Software > Blast Installation Directory Browse. You may have to navigate up one level to find the public directory, then enter the bin, and double click on `blast-2.2.20` and click OK, then close.

## 10. Introduction to amino-acyl tRNA synthetases and structure download

Today you will be exploring the evolution of translation, one of the most fundamental processes common to all extant life. Because translation is such an ancient process, the enzymes that carry it out have had billions of years to diverge from the last universal common ancestor. To understand how proteins with a history that long are related to one another, we will use sequence and structure-based
phylogeny methods.

The translation enzymes you will focus on today called amino-acyl tRNA synthetases. These enzymes ‘charge’ tRNAs with the correct amino acid, which is then delivered to the ribosome and added to a growing protein chain.

Start your exploration of the amino-acyl tRNA synthetases by taking a look at the glutamyl-tRNA synthetase in complex with its tRNA and glutamyl-adenylate (a version of glutamate which has been activated by addition of AMP from ATP). In the MultiSeq window: File > Import Data > check "From files" > navigate to the `public/week6` folder and find the file `1N78.pdb`.

This should cause 2 things to happen. First, in the MultiSeq window, you will see rows of sequence data corresponding to each macromolecule in the structure. Second, in the Open GL display, you will
see the structure itself. In this particular structure, the crystallographic unit (the repeating unit that packed into the crystal used for X-ray diffraction) contains two enzyme-tRNA complexes.

**How many total individual protein chains are in the structures you can see? How many total nucleic acid chains?**

**What organism is this particular amino acyl tRNA synthetase from? (Hint, try looking up 1N78 at the pdb database online)**

## 11. Basic VMD structure viewing tools and navigation, domain highlighting

Being able to navigate a VMD representation for the structure of a molecule you are interested in can be very useful in understanding bioinformatics results. (For example, it might help you understand the potential role of a mutation you discover via a deep sequencing experiment). It is also useful for making publication-quality figures that communicate your findings. We’ll demonstrate basic navigation with the tRNA synthetase we’ve loaded here. Click one of the amino acids in the sequence labeled 1n78A to highlight it. Try to find it in the structure. If it is too small, try changing the highlight style in the MultiSeq menu under View > Highlight Style. Choose “New Cartoon” to match the current style, “Bonds” to see individual residues, or “VDW” (van der Waals) to make it really big.

Now, instead, highlight two different tRNA synthetase domains, listed below.

* (1) Catalytic domain (catalyzes attachment of amino acid): 1-79 and 181-306
* (2) tRNA binding domain (recognizes anticodon of tRNA): 375 - 468


To highlight multiple regions at once (in the MultiSeq panel):

* Click the first residue of the first domain.
* Shift + click the last residue of the first domain.
* Control + click the first residue of the second domain
* Control + shift + click the last residue of the second domain.
* Control + click the first residue of the third domain
* Control + shift + click the last residue of the third domain…..

**Set the highlighted domains to new cartoon style, and make sure they are a color that stands out. Adjust your display so you have a good view of each domain in one of the complexes and take a screen shot (you may have to click outside of the gnome browser first). Paste this screen shot into your answers.**

## 12. Find anti-codon binding residues

Another way to use structures to help interpret results from bioinformatics analysis is to focus on particular regions you are interested in and find out what is nearby that may be interacting (for example, you may want to focus on a known antibiotic binding site and see if any nearby positions correspond to mutated positions that you observed in a drug-resistant molecule). In our demonstration here we will find specific residues in the amino-acyl tRNA synthetases that play a role in its function.

Each amino-acyl tRNA synthetase must charge its client tRNA with the correct amino acid. It does this, in part, by recognizing the same anticodon that the tRNA later uses to bind to mRNA in the ribosome. Lets zoom in on that interaction.

In MultiSeq window:

* “Mark” the protein sequence by clicking the checkbox next to 1n78_A (the next operations will only apply to marked sequences)
* highlight the CUC anticodon in the tRNA from positions 534-536 (tRNA is a highly processed molecule; the numbering probably refers to the original RNA transcript; here it starts at 500)
* click Search > Select Contact Shells > click select residues in Marked Sequences > set contact distance to 3.0 (angstroms) > OK

**Set the highlight style to ‘bonds’ and get a good view of the anti-codon binding residues in the tRNA synthetase and zoom in to take a screen shot. Paste it into your answer sheet.**

## 13. Get files for phylogenetic analysis with MultiSeq

Now that you have the basics of MultiSeq and VMD down, we’ll use it to explore the evolutionary relationships between 31 modern amino-acyl tRNA synthetases from all 3 domains of life. Our goal is understand when the different enzymes originated, and what their closest relatives are. There are 11 different amino acid specificities in this set, so not only will we be exploring how a given amino-acyl tRNA synthetase changed as it evolved in different lineages, but also how the different amino-acyl tRNA synthetases may have emerged from more primitive ancestors.

For this analysis, we will focus on the catalytic domains of the enzymes.

In the MultiSeq window: File > New Session (to clear out previous structures). Then, File > Import Data > check Data From Files > Browse > up one folder > `public` > `week6` > `class-1-synthetases` (shift+click all 31 files, may have to change file type from fasta to pdb) > OK > OK.

Loading all the proteins may take a minute or two, but you should see it progressing in the terminal window in gnome. when its done, you will see a list of the sequences, and a whole bunch of molecules, some of which are overlapped (but they look really messy), in the OpenGL display.

## 14. Sequence-based phylogenetic tree

First, we will try to construct a sequence-based phylogenetic tree for the amino acyl tRNA synthetases. Sequence-based phylogenies are very commonly used to infer evolutionary relationships, but generally, they are only reliable for comparisons between molecules or individuals that share at least 30% sequence identity. At very low sequence identities (<10%), you are approaching the level of similarity in protein sequence that may be expected from random chance. The similarity realm between 30% and 10% is often referred to as the ‘twilight zone’ of sequence identity, because it is usually still possible to use sequences to shed a little light on evolutionary relations. Below 10%, however is sometimes called the ‘midnight zone’ of sequence identity, because it is difficult to draw any conclusions at this level.

**Which is better for making phylogenies of molecules with 30% sequence identity? Protein sequence (amino acids) or DNA sequence (bases) and why? (you must say why for full credit, and may need to research this one)**

We will be making a phylogeny based on the protein sequence of the amino acyl tRNA synthetases. First, we have to carry out a multiple sequence alignment of the proteins.

In the MultiSeq menu:

Tools > Sequence Alignment > make sure ClustalW and Align All Sequences checked > OK

Tools > Phylogenetic Trees > Sequence tree using Percent Identity > OK

This should produce a tree. To make the tree easier to read, in the TreeViewer window:

View > Leaf Color > Taxonomy > Domain of Life

View > Leaf Text > Enzyme > Name

View > Leaf Text > Taxonomy > Species

**Paste a screen shot of your tree.**

**You should notice that synthetases with identical specificities (enzymes responsible for charging the same tRNA) tend to group together. Among the labeled proteins, find an example of a clade where this is NOT the case, with mixed tRNAs specificities, and list the proteins in it.**

**Maybe the tRNAs in the mixed clade are derived from a common ancestor and are used to charge tRNAs with amino acids that have similar properties. Use the colored chart at https://en.wikipedia.org/wiki/Amino_acid to list the key property of the amino acids in this clade, and state whether you think they are similar.**

We can use the tree to measure the actual distance (in sequence identity) between two proteins. Click on one protein, press control, then click on a second protein. It may take a few seconds but the distance between them (1 = 100% different, 0 = 100% identical) will show up in the bottom left corner.

**What is the IDENTITY between the valine synthetase from thermus thermophilus and the isoluecine synthetase from thermus thermophilus?**

**List one protein pair that MultiSeq placed in the same clade, even though there is with <10% identity.**

**Based on the labeled branches in your sequence phylogeny, the methionine group (2 clades) shares it’s most recent common ancestor with which other group?**

## 15. Multiple Structure Alignment

In the twilight zone of sequence identity, and maybe even beyond, it is possible to use structural information to try to predict evolutionary relationships. The thinking is that, even if so much time has passed that the amino acids in a protein have been gradually replaced one-by-one, there is a constant selection pressure to preserve function, especially in proteins that are essential for life. Since function is tightly linked to protein structure, one can assume that this selection pressure to preserve function also maintains structure, so proteins with similar structures may be derived from a common ancestor.

**It is important to point out the assumption that similar structures indicate common ancestry is just that - an assumption. What is another possible explanation for similar structures?**

We will use structural information to make a new phylogenetic tree for the amino-acyl tRNAsynthetases. The first step in finding out how the 31 amino-acyl tRNA-synthetases are related is to align their structures to one another so that the most similar regions are superimposed. We’ve done lots of sequence-based alignment; here, we are using the three-dimensional structural information with a program called STAMP.

In the MultiSeq Window:

Tools > STAMP Structural Alignment > make sure All Structures is checked (leave rest at
defaults) > OK

This command may take a few minutes to run, but watch the OpenGL display while you are waiting. When it is done, answer the question below.

**What happened to the structures in the OpenGL display?**

Lets try to make sense out of that new structure by color coding it. In the VMD Main menu, set the color scale: red = different, green = kindof similar, blue = identical

Graphics > Colors > Color Scale > RGB

In the MultiSeq menu, apply this color scale to a metric of structural similarity, called Qres:

View > Coloring > Apply to all > Qres

This may take a few minutes, but when it’s done, both the alignment and the structures should be color coded.

**Paste a screen shot of your color coded structures here.**

**Which part of the protein is most conserved, the core or the exterior?**

Now lets compare the structural similarity to the sequence identity. In the MultiSeq menu, apply this color scale to a metric of structural similarity, called Qres:

1View > Coloring > Apply to all > sequence identity.

**Which shows more similarity between the proteins, structure or sequence?**

## 15. Structure based phylogeny

Now lets use that structural alignment to make a structure-based phylogeny.

In the MultiSeq menu:

Tools> Phylogenetic Tree > check All Sequences > check Structural Tree using QH (uncheck sequence tree) > OK

**Paste the resulting tree here.**

**Based on the labeled branches in your sequence phylogeny, the methionine group (2 clades) shares it’s most recent common ancestor with which other group?**

**Did the amino-acyl tRNA synthetases with different specificities diverge before or after LUCA (the last universal common ancestor of bacteria, archaea, and eukaryotes)**

**Which tree (sequence-based or structure-based) do you think is more accurate and why?**

## 16. Submit your worksheet!

TODO pull request

**Acknowledgement: Adapted from a lab originally written by Dr. Katie Petrie.**