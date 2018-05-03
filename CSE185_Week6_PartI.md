# Week 6: Proteomics (part 1)
Skills covered: Advanced Git features, mass spec analysis

This week, to give you a small break before we go into final project mode, you won't have to write a lab report. But you will have to go through the lab tutorials and fill in your lab notebook as usual. Instead of writing a report, answer the questions in bold in this tutorial in a worksheet file in the Github repository.

Today, you will explore some mass spectrometry data from proteins that bind to the telomere regions of chromosomes. On Thursday, we'll explore and compare the structure of various proteins as they have evolved over time.

## 0. Forking the Git repository

This week we'll be working with Git repositories outside of the Github classroom interface, more like you would in the real research world.

One of the primary features of Git is the ability to have different "branches", or versions of the code being edited simultaneously, perhaps by different people, and then merged back together again when each person is done working on whatever they were doing. The main line is called the "master" branch, but additional branches can diverge from master and be branched in later. See the diagram below, taken from [this helpful tutorial](https://medium.com/@igor_marques/git-workflow-basics-d405746f6205).
![git branches](https://cdn-images-1.medium.com/max/1600/1*XJmL7fWV4Coxt12AzHeG-A.png).

Let's first start off by clarifying some Git terminology:

* **Repository**: can be thought of as like a folder that contains all elements for a certain project. For our class, we have had one repository for each assignment.
* **Clone**: is like a copy of a repository. You make a clone of a repository to create a copy that lives on your computer rather than on the Github server. You can "push" local changes to the main Github repository to keep the clone up to date with what's on Github, as we've been doing in previous labs.
* **Fork**: is your own copy of another user's repository. When you "fork" someone else's repository, you create a caopy on your own account. This allows you to make changes to your own copy. When you are ready to merge changes with the main repository, you can submit a "pull request" to the original author to update the main repository with your changes. You can also "fetch" updates from the original repository to keep yours up to date. We'll get experience with both of these things this week.

This week, you will create a "fork" of the main assignment repository to make your own local copy. You can do this by going to the the repository on Github (https://github.com/gymreklab/cse185-spring18-week6), and clicking the "Fork" button on the top right. Make sure you are signed in to Github before you do this. Wait a couple seconds and your own copy will be made (https://github.com/<username>/cse185-spring18-week6).

You can now create a "clone" of this repository as we have done in previous weeks. On `ieng6` in your course home directory, run:
```
git clone https://github.com/<username>/cse185-spring18-week6
```

## 1. Exploring the PRIDE database using the PRIDE inspector
