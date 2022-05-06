
# Notebooks for Managing NeurIPS 2014 and Analyzing the NeurIPS Experiment

### 28th October 2014 Neil D. Lawrence

#### Updated September 2021 with missing notebooks for retrospective analysis.

The notebook for tracking the final destination of rejected papers is [here](https://github.com/lawrennd/neurips2014/blob/master/notebooks/where-do-rejected-papers-go.ipynb). The notebook for simulating the conference with 50% subjectivity is [here](https://github.com/lawrennd/neurips2014/blob/master/notebooks/neurips-simulation.ipynb).

#### Updated June 2021 with the retropsective analysis of the NeurIPS Experiment

In June 2021 Neil gave a talk at the Computer Lab which showed a retrospective analysis of the papers in the 2014 conference, tracking what their citations were, and also tracking the fate of rejected papers. The notebook for tracking the citations of papers is [here](https://github.com/lawrennd/neurips2014/blob/master/notebooks/Measuring%20Impact%20of%20Papers%20Using%20Semantic%20Scholar.ipynb).

#### Updated May 2021 with pip installable cmtutils library

#### Updated July 2021 as pip installable libary.

As well as pandas and the standard numpy/scipy stack, the library has a dependency on the `cmtutils` module.

The notebooks can also be installed locally with pip install.

```
pip install git+https://github.com/lawrennd/neurips2014.git
```

In 2014 [Corinna Cortes](http://research.google.com/pubs/author121.html) and I
were NIPS program Co-Chairs. Alan Saul was our Program Manager. As part of the
process we wrote a lot of scripts for processing the data. The scripts I wrote
used the `IPython notebook` (now [Project Jupyter](http://jupyter.org/)) and
`pandas`. It was always my intention to summarise this work in case others find
it useful. It is also quite a good document for summarising what is involved in
program chairing a major conference like NIPS.

This notebook gives a guide to the scripts and utilities used for managing the
[conference management toolkit](http://cmt.research.microsoft.com/cmt/) (CMT).
It is based around a library, `cmtutils`, which manages the submissions.  For
reviewer management (which was the first thing written) the scripts are based
around a local mirror of the CMT user data base in SQLite. For review management
we moved things much more towards `pandas` and used CMT as the central
repository of reviews, exporting them on a daily basis.

A lot of communication is required between CMT through imports and exports. Some
of the links used for CMT exports are available [here](http://nbviewer.ipython.org/github/lawrennd/conference/blob/master/notebooks/Useful%20Links.ipynb).
The various tasks are structured in IPython notebooks below. The code used was
first written for the NIPS 2014 conference, but ideas were based on experience
from using CMT for AISTATS 2012 and some preliminary code written then (for
example for importing the XML formatted version of Excel that CMT uses).

Right from the start it was felt that being able to import and export
information to Google spreadsheets would be very useful. With this in mind an
interface between `pandas` and Google sheets was created (initially just for
reading, then later for updating). This made it much easier to import reviewer
suggestions and export information about paper statuses to reviewers. That
software has been spun out as part of a suite of tools for [Open Data
Science](http://inverseprobability.com/2014/07/01/open-data-science/) that is
[available on github here](https://github.com/sods/ods). These notebooks are
also available in their own [github repository for conference
software](https://github.com/lawrennd/conference).

A note on the code. A lot of this code was written 'live' as reviews were coming
in or as a crisis required averting. The original code for sharing information
via Google spreadsheets was written across two or three days whilst on a family
holiday in the Catskills. Much of the code could do with rewriting, and this is
an ongoing process that I hope other conference chairs or program managers will
contribute to. It is shared here as a record of the work required for a
conference like NIPS as well as in the hope that it will be useful for others.
It is not shared as an example of 'best practice' in python coding. There are
some parts I'm proud of and others I'm not. However, I think it *is* a very good
example of how the notebook can be used with python and `pandas`to do 'live'
data processing of some importance whilst under a great deal of pressure. I
can't imagine having done it quite like this with a different suite of tools.

## 1 Before Submissions are Received

### Creating the Program Committee

You need to have some discussion with your fellow program chairs about who to
include on the committee. If you do this by a google document then you can pull
the results of that discussion down to a local data base, ready for invitation.
[This notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Import%20Meta%20Reviewer%20Suggestions.ipynb) does that.

### Reviewer Data Base Construction
To build up the set of reviewers for your conference you will have several
sources.

* One is the old list of users from a CMT export from an older conference. To
import these to your local data base you use [this notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Add%20Reviewers%20to%20Database.ipynb). It also describes how to import suggestions from Area Chairs,
assuming these are stored in a Google Doc (a good way to do this is to provide a
form that the area chairs can fill in).

### Reviewer Database Update

Reviewers keep on updating their accounts, to get a local refresh of updated
information from CMT use [this notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Update%20DB%20from%20CMT%20Export.ipynb).

### Who's Missing from TPMS

Once you've got all your reviewers, then you can ask Laurent Charlin of the
Toronto Paper Matching System to match them to the TPMS accounts. [This
notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Remind%20Missing%20TPMS.ipynb) downloads the output from Laurent's
script and reports email addresses that have are unmatched. You can use it to
produce semi-colon separated list of emails for mailing reviewers from CMT.

## 2 After Submissions are Received

### Reviewer and Author Duplicate Accounts

It is very easy in CMT to create a new account for an author, even if this
author is already in the system. This leads to a lot of accounts where
authorship and reviewer are separate. [This notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Find%20duplicate%20users.ipynb) tries to find the duplicate users. In 2014, however, there were so
many that we decided the best thing to do was to warn all authors who hadn't
entered their conflict domains to do so. When we first checked for this, there
were around 1500 authors without conflict domains entred. This meant that there
were a large number of potentially uncrecognised conflicts in the system.
Several requests from Corinna reduced this number considerably. We could see
this was having a large effect on any potential allocations because these
conflicts were being entered simultaneously to Area Chair paper bidding. Many
papers that were initially allocated for bidding by our system were being
dropped from bidding lists.

### Paper Duplication

In 2014 we chose to duplicate 10% of papers and have them independently reviewed
in an effort to check calibration. To do this we split the program committee
into two parts, allocating reviewers randomly between parts. For area chairs we
ensured that the expertise was balanced across the two parts. [This
notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Duplicate%20Papers.ipynb) randomly selects 10% of the papers for
duplication and randomly allocates reviewers to one of two groups for reviewing
the papers.

### Shotgun Submissions

Some reviewers submit several papers on very similar topics. These must be
weeded out and either allocated to the same area chair or rejected outright.
[This notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Identify%20Shotgun%20Submissions.ipynb) tries to identify such
papers from the CMT abstract download. It also loads in the 40 papers that
Corinna predicted as shotgun submissions and computes the score associated with
those.

### Generating Bidding Lists

Before allocation of papers to Area Chairs and Reviewers, we'd like to get some
feedback on the sort of papers they are likely to be allocated. This is the
bidding stage. [This notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Generate%20Bidding%20Lists.ipynb) contains code for
allocating papers to Area Chairs and Reviewers for bidding.


### Paper Allocation

Now that we hopefully have the full TPMS matching score matrix, and assuming
that area chairs and reviewers have entered their subject areas (make sure this
is forced when they log in as reviewers!) we can compute a similarities to match
area chairs and reviewers to papers. The responsibility for paper allocation
really falls with area chairs, and the responsibility for matching area chairs
to papers falls with the program chairs. But we can perform a preliminary
allocation with scripts based on similarity scores. [This notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Paper%20Allocation.ipynb) attempts to allocate papers to area chairs and reviewers.

I also wrote a [blog post](http://inverseprobability.com/2014/06/28/paper-allocation-for-nips/) 
on the paper allocation as performed with Corinna for NIPS 2014 which has more information.

### Notifying Reviewers of Paper Reallocation

The area chairs have a few days to tidy up their allocations and make sure each
of their papers has three reviewers that they are happy with. Then, the
allocations are released to reviewers. At this point you should download the
allocation of papers to reviwers via the 'automatic assignment data download
route'. Reviewers are asked to check their allocations as soon as possible. You
can check whether reviewers have logged into CMT to check using [this link]()
and email reviewers to remind them to take a look. You want reviewers to look in
case an allocation is inappropriate or a potential conflict of interest. If
either of these situations arises then you need to email the area chair to
reallocate the paper (this is the area chair's responsibility, but unfortunately
CMT provides no way for the reviewer to know who the area chair for a given
paper is). However, there is also no formal mechanism within CMT to inform newly
allocated reviewers that they have gained a paper. [This notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Assignment%20Diffs.ipynb) computes differences in allocation between the initial allocation
file (as provided to the reviewers after edits by the area chairs). It then
gives a list of emails for reviewers who have gained a paper. These reviewers
can then be emailed to let them know they have a new paper in their allocation.

### Reviewer Expertise

We worked hard to try and create a body of reviewers with the right expertise.
But did it work? In [this notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Reviewer%20Expertise.ipynb) we explore the
expertise of the NIPS 2014 reviewing body. We imported the number of papers
associated with each reviewer since 2007 using [this notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Update%20with%20NIPS%20Paper%20Publications.ipynb).

## 3 After Reviews are Received

Once papers are received we can import reviewer scores for analysis. To do this
you first export from CMT by going to [this page](https://cmt.research.microsoft
.com/NIPS2014/Protected/Chair/PaperDecisionMaking/PaperDecision.aspx) end
`Export->Reviews to Excel`. Then they can be analyzed in [this
notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Import%20Review%20Scores.ipynb).

The program chairs have a lot of information about the whole conference. One of
the roles of the program chairs is to distribute this information as widely as
possible to the area chairs, to ensure that there is good calibration across the
conference. Because of conflicts of interest, many papers aren't visible to many
people. The program chairs act like a hub at the centre of the conference that
can distribute this information. A similar thing happens at the area chair
level. The area chair has more knowledge about the papers than the individual
reviewers. The area chair can use this context to help the reviewers in their
discussion. The following notebooks are designed to facilitate the process of
information sharing.

### Calibration of Reviewers

Each reviewer may have their own idea about the scale of the reviews, but what
is their idea? The program chairs have access to all reviews and can try to tell
if one reviewer is (on average) more negative than another. In [this
notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Reviewer%20Calibration.ipynb) we try and correct for individual
offsets from reviewers. The quality scores are calibrated and converted to a
probability of acceptance (based on quality scores only) that allows for
calibration across the whole conference. This information can be returned to the
area chairs who can make use of it as they see fit in their discussions with
reviewers. A blog post on the reviewer calibration process  for NIPS 2014 is
[available here](http://inverseprobability.com/2014/08/02/reviewer-calibration-
for-nips/).

### Trouble Report

It is often not very convenient for area chairs to trawl through all the pages
of CMT checking for everything that might have gone wrong. To help with this
job, we might want to automate some description of where the problems are. In
[this notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Attention%20Report.ipynb) an automated report is constructed
for distribution amoungst area chairs to highlight any potential issues in their
allocation (e.g. short reviews, low confidence reviews, papers at the borderline
of acceptance). This is particularly useful before reviews are returned to
authors, to try and ensure that authors are responding to a coherent set of
reviews.

## 4 After Author Feedback is Received

Once author feedback is received we can introduce a bit of additional oversight
into the process. For NIPS 2014 we formed buddy pairs between area chairs.

### Spreadsheet Reports for Groups

The spreadsheet reports are an evolution of the emailed reports. These reports
are designed for sharing information between the area chairs and the program
chairs. They are, if you will, a message passing interface. The program chairs
place information about the calibration of the reviwer scores and any
automatically generated warnings in the spreadsheet. The area chairs can then
augment the spreadsheet with notes and the current position on accepts and
rejects. [This notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Spreadsheet%20Reports.ipynb) creates and shares (via
Google docs) summary spreadsheet reports of paper status to groupings provided
by the program chairs. These groupings may be 'buddy pairs' or teleconference
groups. The groupings are downloaded form a different Google spreadsheet. Once
those spreadsheet reports are created they can be updated with [this
notebook](http://nbviewer.ipython.org/github/lawrennd/nips2014/blob/master/notebooks/Update%20Spreadsheets.ipynb) (assuming the area chairs haven't
played with the formatting!).

## 5 Decision Time

Final decisions were made by discussions that took place with teams of four
reviwers. A [description of this process for NIPS 2014 is available
here](http://inverseprobability.com/2014/09/13/nips-decision-time/).

#### Further Acknowledgements

Thanks a lot to the CMT team at Microsoft Research for making the software
available and responding with patience to our requests for support. Further
thanks to the IPython team for creating the notebook. And particular thanks to
[Fernando Perez](http://fperez.org/) who I first met at the Open Source Software
workshop at NIPS 2013 and who visited Sheffield in April 2014. He even
contributed to the code for the conference processing whilst he was here!
Finally, thank you to [Wes McKinney](http://blog.wesmckinney.com/) for creating
`pandas`. By tracking indices, and providing easy access to data columns pandas
saved me an enormous amount of risky work in 'index book keeping'. When I
started, it sometimes felt like finding the right pandas command did take some
time, but once I'd found it each command saved probably an equivalent amount of
time in coding it myself and came with the knowledge that the index bookkeeping
was being safely done for me. Partially in thanks, I just purchased [Python for
Data Analysis](http://shop.oreilly.com/product/0636920023784.do) but of course
the real reason I bought it was because I expect it to be a damn good read!


    
